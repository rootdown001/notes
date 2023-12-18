2 tricks before starting next section

## Trick to cause 100 ms delay

- Trick to cause 100 ms delay (to emulate slow component)

```jsx
const now = performance.now();
while (now > performance.now() - 100) {
  // Do nothing
}
```

### Find button boundaries

- `buttonRef.current.getBoundingClientRect()`
- returns object of button boundaries

## useMutationLogger() CUSTOM hook

- logs to console EVERYTIME DOM HAS BEEN CHANGED
- will show how many changes of Dom in each action

```jsx
import { useEffect } from "react";

export default function useMutationLogger() {
  useEffect(() => {
    const observer = new MutationObserver(() => {
      console.log("DOM Changed");
    });

    observer.observe(document.documentElement, {
      childList: true,
      characterData: true,
      subtree: true,
      attributes: true,
    });

    return () => {
      observer.disconnect();
    };
  }, []);
}
```

## useLayoutEffect Hook

Works like `useEffect` but `useLayoutEffect` runs BEFORE DOM is painted to screen

- `useEffect` runs after above and below code and jsx are run and DOM is painted to screen
- `useLayoutEffect` runs inline between rendering component and showing it to user

  - if a layout state has changed, runs other code and jsx again THEN paints to screen

When to use `useLayoutEffect`

- In the scenario where you need to look at value of things on screen and put other things on the screen based on those values

  - ex measure 1 element, or move and element based on measurements of another element

- Figure out by seeing if items are flashing on screen in wrong place before showing up in right place

Look at below code...

1.  when "Show" button is clicked, `isOpen` is set to true and button shows
2.  when `isOpen` changes (`useEffect` array dependency) ...

    - if `!isOpen` (or null) `setPopupTop(0)` (doing this so everytime click Show it shows same error as it very forst time)
    - if `isOpen`, gets the `bottom` location using `buttonRef.current.getBoundingClientRect()` and sets popupTop to bottom + 25

`App`

```jsx
import { useEffect, useRef, useState } from "react";
import useMutationLogger from "./useMutationLogger";

export default function App() {
  const [isOpen, setIsOpen] = useState(false);
  const [popupTop, setPopupTop] = useState(0);
  const buttonRef = useRef(null);

  useEffect(() => {
    if (buttonRef.current == null || !isOpen) return setPopupTop(0);
    console.log(buttonRef.current.getBoundingClientRect());
    const { bottom } = buttonRef.current.getBoundingClientRect();
    setPopupTop(bottom + 25);
  }, [isOpen]);

  return (
    <>
      <button ref={buttonRef} onClick={() => setIsOpen((o) => !o)}>
        Show
      </button>
      {isOpen && (
        <div
          style={{
            position: "absolute",
            top: `${popupTop}px`,
            border: "1px solid black",
          }}
        >
          Tooltip
        </div>
      )}
    </>
  );
}
```

NOW, notice that when click "Show" the "Tooltip" flashes very temporapily right over Show button and then moves itself down

Does this because

- by default `popupTop` is set to 0
- so Tooltip `div` `style` top: is breifly set to 0px (so briefly shows Tooltip starting at top of 0px)
- THEN, after rendering code, runs `useEffect` (where it is then told to put at Show `bottom` + 25)
- then Tooltip moves down

If you use the `performance.now()` code, it is slowed by 100 ms and you can see

```jsx
const now = performance.now();
while (now > performance.now() - 100) {
  // Do nothing
}
```

If we use the `useMutationLogger` custom hook, we see that when we push Show, the DOM changes 2x bc of way `useEffect` works

- when component first runs it runs all that code above and below `useEffect`, and renders jsx
- THEN `useEffects` run - `useEffect`s are the LAST THING THAT RUNS AFTER EVERYTHING PAINTED TO SCREEN
- THEN, runs all the above and below code again, and paints from jsx

- 1st - render Tooltip, where it starts at 0px
- 2nd - after useEffect where it renders at bottom +25px

`useLayoutEffect` Works like `useEffect` but runs BEFORE DOM is painted to screen

`App`

```jsx
// ...
useLayoutEffect(() => {
  if (buttonRef.current == null || !isOpen) return setPopupTop(0);
  const { bottom } = buttonRef.current.getBoundingClientRect();
  setPopupTop(bottom + 25);
}, [isOpen]);
//...
```

So the order of events for `useLayoutEffect`...

1.  Runs code above and below, and jsx
2.  HOLDS on changing DOM
3.  runs `useLayoutEffect` - sees a state is changing
4.  runs all code above and below and jsx again
5.  NOW paints to screen (updates DOM)

So if we use `useLayoutEffect` with previous code, as below

- we note that the Tooltip no longer starts in the above position first
- we see that our `useMutationLogger` shows DOM only changes once (not twice)

`App`

```jsx
import { useLayoutEffect, useRef, useState } from "react";
import useMutationLogger from "./useMutationLogger";

export default function App() {
  useMutationLogger();

  const [isOpen, setIsOpen] = useState(false);
  const [popupTop, setPopupTop] = useState(0);
  const buttonRef = useRef(null);

  useLayoutEffect(() => {
    if (buttonRef.current == null || !isOpen) return setPopupTop(0);
    const { bottom } = buttonRef.current.getBoundingClientRect();
    setPopupTop(bottom + 25);
  }, [isOpen]);

  const now = performance.now();
  while (now > performance.now() - 100) {
    // Do nothing
  }

  return (
    <>
      <button ref={buttonRef} onClick={() => setIsOpen((o) => !o)}>
        Show
      </button>
      {isOpen && (
        <div
          style={{
            position: "absolute",
            top: `${popupTop}px`,
            border: "1px solid black",
          }}
        >
          Tooltip
        </div>
      )}
    </>
  );
}
```

Why have 2 alomst identical hooks? Why not just use `useLayoutEffect` all of the time

- Because it is synchronous inline, so if it's slow or a lot of code, it will slow down application

  - none of the code can render to screen until `useLayoutEffect` finishes
  - so ONLY USE when need to
  - use `useEffect` for every situation except one where `useLayoutEffct` is needed

## `useDebugValue` Hook

- `useDebugValue` is a React Hook that lets you add a label to a custom Hook in React DevTools
- custom hooks don't have the value automaticaaly next to component in DevTools

Take the following code

`App`

```jsx
import { useState } from "react";
import { useOnlineStatus } from "./useOnlineStatus";
import { useLocalStorage } from "./useLocalStorage";

export default function App() {
  const isOnline = useOnlineStatus();
  const [name, setName] = useLocalStorage("Name", "");
  const [age, setAge] = useState(0);

  return (
    <>
      <h3>{isOnline ? "Online" : "Offline"}</h3>
      <input value={name} onChange={(e) => setName(e.target.value)} />
      <br />
      <br />
      <input
        value={age}
        onChange={(e) => setAge(e.target.value)}
        type="number"
      />
    </>
  );
}
```

`useLocalStorage`

```javascript
import { useState, useEffect } from "react";

export function useLocalStorage(storageKey, initialValue) {
  const [value, setValue] = useState(() => {
    const tempGet = localStorage.getItem(storageKey);
    // we need to see if a value for this key exists in localStorage
    if (tempGet === null) {
      // if not we need to decide if we are passing initialValue as function or variable
      if (typeof initialValue === "function") {
        return initialValue();
      } else {
        return initialValue;
      }
      // if it DOES exist, return that value as initial value
      // change back to JSON object with JSON.parse()
    } else {
      return JSON.parse(tempGet);
    }
  });

  // when value changes, we setItem( to localStorage)
  useEffect(() => {
    // see if key-value changes bc now undefined
    if (value === undefined) {
      localStorage.removeItem(storageKey);
    } else {
      localStorage.setItem(storageKey, JSON.stringify(value));
    }
  }, [storageKey, value]);

  return [value, setValue];
}
```

`useOnlineStatus`

```javascript
import { useEffect, useState } from "react";

export function useOnlineStatus() {
  const [isOnline, setIsOnline] = useState(navigator.onLine);

  useEffect(() => {
    function handler() {
      setIsOnline(navigator.onLine);
    }

    window.addEventListener("online", handler);
    window.addEventListener("offline", handler);

    return () => {
      window.removeEventListener("online", handler);
      window.removeEventListener("offline", handler);
    };
  }, []);

  return isOnline;
}
```

- CODEGPT Note: "The navigator.onLine property is a Boolean value that represents the online status of the user's device. It indicates whether the device is currently connected to the internet or not."

To debug these components, we go to out DevTools React Components.

- Click on App (only component)
- we see our hooks `use...`

  - OnlineStatus
  - LocalStorage
  - State

- it is hard to read because no value next to `useOnlineStatus` or `useLocalStorage`
- when change one of these, no value being logged out (unless you open dropdown)

![DevTools Image](/assets/img/useDebugValue_before.png "useDebugValue_before")

- `useDebugValue` allows you to easily see a value next to component in Dev Tools

To use, just open your hook, and add `useDebugValue()` and pass any value you want

- this is what will show up next to component

Example:

`useOnlineStatus`

```javascript
export function useOnlineStatus() {
  const [isOnline, setIsOnline] = useState(navigator.onLine);

  // add useDebugValue
  useDebugValue(isOnline);

//...
```

![DevTools Image](/assets/img/useDebugValue_after.png "useDebugValue_after")

Can call multiple times, like in `useLocalStorage` below

`useLocalStorage`

```javascript
  });

  // call multiple useDebugValue
  useDebugValue(storageKey);
  useDebugValue(value);

  // when value changes, we setItem( to localStorage)
  useEffect(() => {

```

![DevTools Image](/assets/img/useDebugValue_multiple.png "useDebugValue_multiple")

You can pass as an array, and it shows in DevTools as Array

```javascript
// call multiple useDebugValue
useDebugValue([value, storageKey]);
```

![DevTools Image](/assets/img/useDebugValue_array.png "useDebugValue_array")

You can pass as an object, and it shows in DevTools as object

```javascript
// call multiple useDebugValue
useDebugValue({ storageKey, value });
```

![DevTools Image](/assets/img/useDebugValue_object.png "useDebugValue_object")

One caveat to `useDebugValue`

- shouldn't use too often because increases overhead in production
- most useful for...

  - debugging complex hooks
  - creating library with complex debugging
  - usually remove before production

If you want to leave in, can USE THE FUNCTION VERSION

- with function version, it only runs if dev tools is open and has to log something to it

`useLocalStorage`

```javascript
//...  In component
// call as function
// pass function as second parameter, automatically calls with value
useDebugValue(value, getValue);

///... Outside component
function getValue(value) {
  return `Computed: ${value}`;
}
```

![DevTools Image](/assets/img/useDebugValue_function.png "useDebugValue_function")

Rules...

1.  Can only use inside of custom hooks
2.  Can call `useDebugValue` multiple times in a row and returns as if array
3.  Can call `useDebugValue` with values in an array and returns an array
4.  Can call `useDebugValue` with values in an object and returns an object
5.  Shouldn't use too often bc of overhead in production
6.  use FUNCTION VERSION if leaving in production - only runs if DEV Tools open

## `useId` hook

- Easy to understand
- Makes sure "id" (ex, in input or label) are unique
- `useId` gives a unique id to each instance
- it doesn't take parameters, and returns 1 thing (id)
- in React, you cant use same "id" in multiple areas in same component
- in HTML should never have same id in more than 1 element
- use any time you need to hook up different html elements with unique identifiers (like `htmlFor` or `aria-`)
- DO NOT USE to generate unige keys etc - only for id / htmlFor / aria-

  - causes problems - ex, click on one to type and it selects the other

Take code below

`App`

```jsx
import EmailForm from "./EmailForm";

export default function App() {
  return (
    <>
      <EmailForm />
      <p>lots of text</p>
      <EmailForm />
    </>
  );
}
```

`EmailForm`

```jsx
import { useState } from "react";

export default function EmailForm() {
  const [email, setEmail] = useState("");

  return (
    <div>
      <label htmlFor="email">Email</label>
      <input
        type="email"
        id="email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
      />
    </div>
  );
}
```

![without useId](/assets/img/useId_before.png "useId_before")

The `label` and `input` with `id="email"` are used twice

PROBLEM: can't reuse on same page

- when we do this we can see there is a problem. When click on bottom label, goes to top input
- in React, bad practice to hardcode id's like this
- can't solve with using `Math.random()` bc if you do, everytime you re-render the ids are regenerated
- also, React can render on client and server. If render on server and send down to client, everything has to hook up and be the same.

  - if use the `Math.random()` (instead of `useId()`) then will get one id on server and diff one on client

- `useId` gives a unique id to each instance
- it doesn't take parameters

`EmailForm`

```jsx
export default function EmailForm() {
  const [email, setEmail] = useState("");

  //   add useId()
  const id = useId();

  return (
    <div>
      <label htmlFor={id}>Email</label>
      <input
        type="email"
        // set id={id}
        id={id}
        value={email}
        onChange={(e) => setEmail(e.target.value)}
      />
    </div>
  );
}
```

When we set `const id = useId()` and then in form set `id={id}` and we inspect, we see a different id for each instance on page (here it's r1 & r3)

- if we refresh page, we get SAME IDs

![with useId](/assets/img/useId_after.png "useId_after")

Now say we have multiple inputs (email AND name)

- don't call `useId()` twice
- instead, call `id` as a literal with a modifier... Ex:

      ```jsx
      id = {`${id}-email`}
      ```

  Full code...

`EmailForm`

```jsx
export default function EmailForm() {
  const [email, setEmail] = useState("");

  const id = useId();

  return (
    <div>
      <label htmlFor={`${id}-email`}>Email</label>
      <input
        type="email"
        // id with sufix
        id={`${id}-email`}
        value={email}
        onChange={(e) => setEmail(e.target.value)}
      />
      <br />

      <label htmlFor={`${id}-name`}>Name</label>
      <input
        type="text"
        // id with suffix
        id={`${id}-name`}
      />
    </div>
  );
}
```

Output

![with useId](/assets/img/useId_suffix.png "useId_suffix")

Here we can see that

- for first Email & Name (top one), the ids are ":r1:-email" & ":r1:-name"
- for the second (bottom) Email & Name, the ids are ":r3:-email" & ":r3:-name"

## `useImperitiveHandle` Hook

(Recall using `forwardRef()` to Child components)

`useImperitiveHandle` let's us change what the ref is actually pointing to

- by default when you use `forwardRef()`, you can only set the ref to some kind of Component (ex, input component etc)
- `useImperitiveHandle` if you want ref to be something else
- should not be using very often - it changes you from a declarative way of programming to a more imperative way of programming
- to use, you pass it

  - ref (that was passed to Child component, THAT YOU WANT TO OVERWRITE)
  - function (that returns what NEW ref is going to be)
  - dependancy array (if leave it off, runs everytime page renders)

```jsx
useImperativeHandle(ref, () => {}, []);
```

`CODEGPT`

```
"`useImperativeHandle` is a React Hook that allows you to customize the instance value that is exposed to parent components when using `ref`.

  - By default, when you create a `ref` in a child component and use it in a parent component, the parent component can directly access the child component's instance. However, sometimes you may want to limit or customize the exposed functionality."
```

Below is example of just `forwardRef()`

- in it, the parent component (`App`) can directly access the child components (`Input`) ref

`App`

```jsx
import { useRef } from "react";
import { Input } from "./Input";

export default function App() {
  const inputRef = useRef();

  return (
    <>
      <button onClick={() => inputRef.current.focus()}>Focus</button>
      <Input ref={inputRef} />
    </>
  );
}
```

`Input`

```jsx
import { forwardRef } from "react";

function Inner(props, ref) {
  return <input {...props} ref={ref} />;
}

export const Input = forwardRef(Inner);
```

Use cases...

1.  can make passed ref do something completely different

- below, we use it so ref points to alert hi fxn
- in `App` we change onClick to call `inputRef.current.alertHi()`
- so ref doesn't even point to `input` element
- we have changed what it does

`App`

```jsx
import { useRef } from "react";
import { Input } from "./Input";

export default function App() {
  const inputRef = useRef();

  return (
    <>
      // onClick calls inputRef.current.alertHi()
      <button onClick={() => inputRef.current.alertHi()}>Focus</button>
      <Input ref={inputRef} />
    </>
```

`Input`

```jsx
import { forwardRef, useImperativeHandle } from "react";

function Inner(props, ref) {
  useImperativeHandle(ref, () => {
    // return object with function named alertHi
    return { alertHi: () => alert("Hi") };
  });

  // removed ref={ref}
  return <input {...props} />;
}

export const Input = forwardRef(Inner);
```

2.  Use it so that you aren't exposing all of the methods of your `input` ref... can make so only expose `.focus()` etc

- in Child, create a ref - brand new ref / entirely seperate from Parents inputRef
- set in `input` (note, `input` ref is `inputRef=...` (not `ref=`))
- in `useImperitivehandle`, we return an object that defines a fxn `focus`, and it returns `inputRef.current.focus()`

  - so the inputRef in Parent component, can use ONLY the `focus` function defined

  -NOTE: in parent, trying to access other methods (like `inputRef.current.value()`) doesn't work

`App`

```jsx
import { useRef } from "react";
import { Input } from "./Input";

export default function App() {
  const inputRef = useRef();

  return (
    <>
      // the inputRef in Parent component, can use ONLY the `focus` function
      defined
      <button onClick={() => inputRef.current.focus()}>Focus</button>
      <Input ref={inputRef} />
    </>
  );
}
```

`Input`

```jsx
import { forwardRef, useImperativeHandle, useRef } from "react";

function Inner(props, ref) {
  // create a ref (IN CHILD) - brand new ref / entirely
  // seperate from Parents inputRef
  const inputRef = useRef();

  useImperativeHandle(ref, () => {
    // return object with function named alertHi
    return { focus: () => inputRef.current.focus() };
  });

  // changed ref={ref} to ref={inputRef}
  // so input ref (that refers to whole input) in inputRef
  // BUT parent will only have access to inputRef.current.focus()
  return <input {...props} ref={inputRef} />;
}

export const Input = forwardRef(Inner);
```

3.  If you have 2 input (or more) elements in Child, can setup so parent inputRef can put focus on either.

- set up 2 inputRef (inputRef & inputRef2) in child, one on each input element
- use `useImperativeHandle` to assign BOTH to ref returned

`App`

```jsx
import { useRef } from "react";
import { Input } from "./Input";

export default function App() {
  const inputRef = useRef();

  return (
    <>
      <button
        // onClick references input2.focus()
        onClick={() => inputRef.current.input2.focus()}
      >
        Focus
      </button>
      <Input ref={inputRef} />
    </>
  );
}
```

`Input`

```jsx
import { forwardRef, useImperativeHandle, useRef } from "react";

function Inner(props, ref) {
  // define inputRef and inputRef2 for 2 inputs below
  const inputRef = useRef();
  const inputRef2 = useRef();

  useImperativeHandle(ref, () => {
    // when ref is shared with App (via forwardRef) it has an object with input1 and input 2
    return { input1: inputRef.current, input2: inputRef2.current };
  });

  return (
    <>
      <br />

      <input
        {...props}
        // uses inputRef
        ref={inputRef}
      />
      <br />
      <input
        {...props}
        //  uses inputRef2
        ref={inputRef2}
      />
    </>
  );
}

export const Input = forwardRef(Inner);
```

4.  Remember to use DEPENDENCY ARRAY if needed (as shown below)

`Input`

```jsx
import { forwardRef, useImperativeHandle, useState } from "react";

function Inner(props, ref) {
  const [value, setValue] = useState("");

  useImperativeHandle(
    ref,
    () => {
      return { value };
    },
    // use dependency array to rerun each time value changes
    [value]
  );

  return (
    <>
      <input
        type="text"
        value={value}
        onChange={(e) => setValue(e.target.value)}
      />
    </>
  );
}

export const Input = forwardRef(Inner);
```

## `useCallback` as ref (different way to use

`useCallback`)

WITH THIS, KEEP IN MIND DIFFERENCE BETWEEN AN ELEMENT EXISTING AND TRIGGERING useCALLBACK VS BEING IN VIEWPORT

Anytime you want to do something ONCE AN ELEMENT SHOWS UP in the DOM (could be on page), then use `useCallback` AS `useRef`

- We use `useCallback` INSTEAD OF `useRef`!
- If we use `useCallback` (instead of `useRef` + `useEffect`), it will only run when the element that your ref refers to shows up in DOM
- Pretty common to do - most common use is when you only want to do something once an element is rendered to screen (even if outside of viewport)

Note: - when we do this, we can't do anything with inputRef.current... don't get access to features (there is a way around by making second inputRef) - #2 below

1. Explaination

```jsx
const inputRef = useCallback(() => {});
```

- some things can't be done with `useEffect()` - code breaks if it is doing something with inputRef, but inputRef doesn't exist yet on page (see below)

  - there is not a way with `useEffect()` to have it only called if inputRef exists on page
  - we CAN do with `useCallback()`

example

```jsx
import { useCallback, useState } from "react";

export default function App() {
  const [showInput, setShowInput] = useState(false);
  const inputRef = useCallback((input) => {
    if (input == null) return;
    input.focus();
  }, []);

  return (
    <>
      <button onClick={() => setShowInput((s) => !s)}>Toggle</button>
      {showInput && <input type="text" ref={inputRef} />}
    </>
  );
}
```

READ BELOW FOR EXPLAINATION...

Below code works well

```jsx
import { useEffect, useRef } from "react";

export default function App() {
  const inputRef = useRef(null);

  useEffect(() => {
    inputRef.current.focus();
  }, []);

  return (
    <>
      <input type="text" ref={inputRef} />
    </>
  );
}
```

BUT CODE BELOW doesn't work - bc inputRef.current DOESN'T EXIST YET on page

```jsx
import { useEffect, useRef, useState } from "react";

export default function App() {
  const [showInput, setShowInput] = useState(false);
  const inputRef = useRef(null);

  useEffect(() => {
    inputRef.current.focus();
  }, []);

  return (
    <>
      <button onClick={() => setShowInput((s) => !s)}>Toggle</button>
      {showInput && <input type="text" ref={inputRef} />}
    </>
  );
}
```

SOLVE THIS by setting `inputRef = useCallback(() => {}, [])` (instead of using `useRef` and `useEffect`)

```jsx
import { useCallback, useState } from "react";

export default function App() {
  const [showInput, setShowInput] = useState(false);

  // useCallback will run when inputRef shows on page
  const inputRef = useCallback((input) => {
    if (input == null) return;
    input.focus();
  }, []);

  return (
    <>
      <button onClick={() => setShowInput((s) => !s)}>Toggle</button>
      {showInput && <input type="text" ref={inputRef} />}
    </>
  );
}
```

2. - Explaination

As mentioned, there IS a way to access your `.current`, or `.current.value`

- the answer is to use `useCallback` as above so it only runs the code for ... `.focus` when `input` element is on screen
- BUT make a real `useRef` also (by diff name) and set it equal to `value` of `useCallback` which is inputRef
- this is done AT TOP of `useCallback` code before anything else is done

```jsx
import { useCallback, useRef, useState } from "react";

export default function App() {
  const [showInput, setShowInput] = useState(false);

  // create a different ref set to useRef
  const actualInputRef = useRef();

  const inputRef = useCallback((input) => {
    // update new ref to be equal to input value of useCallback
    actualInputRef.current = input;

    // set focus to input value of useCallback, which is out inputRef
    if (input == null) return;
    input.focus();
  }, []);

  console.log(actualInputRef.current);

  return (
    <>
      <button onClick={() => setShowInput((s) => !s)}>Toggle</button>
      {showInput && <input type="text" ref={inputRef} />}
    </>
  );
}
```

## `parseLinkHeader()` (Kyle wrote for project)

```jsx
/*
  This function takes in fetch request's `res.headers.get('Link')` value and converts it to an object with each link type as a key and the url as the value. The only value that we care about for this project is the `next` link, which we will use to fetch the next page of data (if it exists).
*/
export function parseLinkHeader(linkHeader) {
  if (!linkHeader) return {};
  const links = linkHeader.split(",");
  const parsedLinks = {};
  links.forEach((link) => {
    const url = link.match(/<(.*)>/)[1];
    const rel = link.match(/rel="(.*)"/)[1];
    parsedLinks[rel] = url;
  });
  return parsedLinks;
}
```

## `IntersectionObserver` (from Kyles Blog article)

- monitors if an element is in viewport or not

**for an element to change its intersection status it must scroll in/out of the current viewport**

**We are checking the isIntersecting property. This property is true if the element is on the page and it is false if the element is not on the page**

Intersection Observer is one of 3 observer based JavaScript APIs with the other
two being Resize Observer and Mutation Observer. Intersection Observer in my opinion
is the most useful because of how easy it makes things like infinite scrolling,
lazing loading images, and scroll based animations. In this article I will cover
all the basics of Intersection Observer as well as the more complex nuances so you
can start using Intersection Observer to spice up your sites.

Your First Intersection Observer
Creating an Intersection Observer is actually quite simple since all you need to do is pass a function to the IntersectionObserver constructor.

```jsx
const observer = new IntersectionObserver((entries) => {
  console.log(entries);
});
```

In the above example we created a brand new Intersection Observer

- in the function we passed to it we are just logging out the entries parameter. This entries parameter is the only argument that the function accepts
- it just outputs the information related to each element that changes its intersection status. That may sound confusing but let’s take a look at a simple example.

```jsx
const observer = new IntersectionObserver((entries) => {
  entries.forEach((entry) => {
    const intersecting = entry.isIntersecting;
    entry.target.style.backgroundColor = intersecting ? "blue" : "orange";
  });
});

observer.observe(document.getElementById("test"));
```

In the above code...

- we are calling the observe method on our Intersection Observer and telling it to observe intersection changes for the element with the id test.
- for an element to change its intersection status it must scroll in/out of the current viewport.

Below you will see an example that changes the color of our element every time the element intersects our container. In all the examples on this page the solid section with the border is our viewport. You can think of that as the visible portion of the screen. The greyed out sections outside the solid section are considered outside the viewport, but I made them visible to you so you can see what happens as our element intersects the viewport. Normally this area would not be visible.

[See Animation in Blog](https://blog.webdevsimplified.com/2022-01/intersection-observer/)

- As you can see above as soon as our element has 1 single pixel enter the viewport it changes to a blue color. Also, as soon as the entire element is back off the screen it changes back to orange. Now let’s break apart how this all works.

In our code we are

- looping through all the elements in the entries array. This array just lists all the elements we are observing that have had their intersection status change.
- This means that the element has either entered or left the screen.
- We are then looping through those entries and for each one we are checking the isIntersecting property. This property is true if the element is on the page and it is false if the element is not on the page.
- Finally, we are using the target property of our entry to get the current element that is being observed and changing its background to the appropriate color.

Intersection Observer Options

Now the above code covers the most basic use case for the intersection observer, but on its own this isn’t too useful. The different options you can pass to your Intersection Observer when you create it really take this to the next level.

1.  Threshold

- Probably my favorite property is the threshold property. This accepts a value between 0 and 1 and represents the percentage of the element that must be visible before isIntersecting becomes true.
- By default this is set to 0 which means as soon as any part of the element is visible it will be considered intersecting.

```jsx
const observer = new IntersectionObserver(changeColor, { threshold: 1 });

observer.observe(document.getElementById("test"));
```

[See Animation in Blog](https://blog.webdevsimplified.com/2022-01/intersection-observer/)

- In our above example we set our threshold to 1 which means 100% of the element must be visible before it will be considered intersecting so now our color only changes to blue when the entire element is in the viewport.
- You can also pass an array to threshold which means that the Intersection Observer will fire each time your element passes one of the thresholds passed to it.

```jsx
const observer = new IntersectionObserver(
  (entries) => {
    entires.forEach((entry) => {
      entry.target.innerText = `${Math.round(entry.intersectionRatio * 100)}%`;
    });
  },
  { threshold: [0, 0.25, 0.5, 0.75, 1] }
);

observer.observe(document.getElementById("test"));
```

[See Animation in Blog](https://blog.webdevsimplified.com/2022-01/intersection-observer/)

- In the above code you will notice the text in the square will change as you scroll it and it will only update once the square hits the thresholds we passed to our Intersection Observer.
- In order to print this percent value we are getting the intersectionRatio property of our entry which is a number between 0 and 1 which represents the current percentage of the element that is within the viewport.

- You will notice that the text in the box is not always exactly the same as our thresholds. This is because as we scroll we are sometimes scrolling past the exact percentage values so the intersectionRatio will be close to but not exactly the same as our threshold.
- If you scroll slower the numbers will be more accurate while a fast scroll will have less accurate numbers since you are scrolling past more content before the observer can fire.

2.  Root Margin (for infinate scrolling, lay loading, loading animations)

- The next useful option you can pass to an Intersection Observer is rootMargin. - - This property is defined exactly the same as the margin CSS property in that it can take 1 value to apply margin to all sides or multiple values to give individual values to each side.
- The rootMargin will be added to the container viewport so in essence we can shrink/grow our view port with this value.

```jsx
const observer = new IntersectionObserver(changeColor, { rootMargin: "50px" });

observer.observe(document.getElementById("test"));
```

[See Animation in Blog]("https://blog.webdevsimplified.com/2022-01/intersection-observer/")

- With a rootMargin of 50px our viewport is now considered to be 50px larger so once the element is 50px from being within the viewport it will be considered intersecting.
- I added red lines to the above demo to represent where our rootMargin grows the viewport to. Using a positive rootMargin like this is really useful when you need to lazy load images, or do something like infinite scrolling since you can load in all the data before it becomes visible to the user.

- You can also do negative margins to shrink the viewport.

```jsx
const observer = new IntersectionObserver(changeColor, { rootMargin: "-50px" });

observer.observe(document.getElementById("test"));
```

[See Animation in Blog](https://blog.webdevsimplified.com/2022-01/intersection-observer/)

- As you can see from the blue lines our new viewport is 50px smaller.
- This type of rootMargin is perfect for doing things like loading animations that you want to occur after an element is at least a certain distance from the edge of the screen.

3.  Root

- The last option you can pass to an Intersection Observer is the root property which is a property you honestly probably won’t use much.
- This property must be an element that is an ancestor of the elements being observed. This root element is then used as the viewport for intersection.
- This is really only useful when you have a scrolling container inside your page that you want to check observations for since you can make the scrolling container the root element instead of the screen.

- In order to make all the examples on this page work I actually had to use the root property to set the scrolling container as the root element since otherwise the observer would not work correctly. (bc inside blog article)

Advanced Intersection Observer

- This covers all the basic use cases and options for Intersection Observers, but there are a few additional things you should know.

4.  Second Callback Parameter

- The callback you pass to new Intersection Observers actually has two parameters. The first parameter is the entries parameter we have talked a bunch about. The second parameter is simply the observer that is observing the changes.

```jsx
const observer = new IntersectionObserver((entries, o) => {
  console.log(o === observer);
  // True
});
```

-This parameter is useful when you need to do something with the observer from within the callback since you may not always have access to the observer variable from the callback depending on where the callback is defined.

5.  Unobserve and Disconnect

- It is important to stop observing elements when they no longer need to be observed, such as after they are removed from the page or after lazy loading an image in order to avoid memory leaks or performance issues.
- This can be done with the unobserve method or the disconnect method which are both methods on the Intersection Observer.
- The unobserve method takes a single element as its only parameter and it stops observing that single element.
- The disconnect method takes no parameters and will stop observing all elements.

```jsx
new IntersectionObserver((entries, observer) => {
  entries.forEach((entry) => {
    if (entry.isIntersecting) {
      lazyLoadImage(entry);
      observer.unobserve(entry.target);
    }
  });
});
```

## `useEffectEvent` (experimental)

- use this is you have some code inside of a `useEffect`, and you are trying to depend on a variable inside that `useEffect` that you don't want inside of your dependancy array

Experimental: Cannot use in production version of React

In following room, want to have the `useEffect` connect me to a room everytime `url` changes

- so when changes, connect to new room, disconnect from old room
- `logConnection()` is called sending `Connected to ${url}` as `value`, and the `logConnection` logs to console `value` & `url`

```jsx
import { useEffect } from "react";

export function ChatRoom({ url, displayName }) {
  function logConnection(value) {
    console.log(value, displayName);
  }

  useEffect(() => {
    const room = connectToRoom(url);

    room.onConnected(() => {
      logConnection(`Connected to ${url}`);
    });

    return () => room.disconnect();
  }, [url]);
}

function connectToRoom(url) {
  // ...
}
```

BUT the `[url]` in `useEffect` shows error warning saying that `useEffect` has a missing dependency

![dependency warning](/assets/img/dependency_warning.png "dependency_warning")

- because the `logConnection` that is called is logging `displayName` too, so `useEffect` thinks `displayName` should be a dependency
- dont want to pass in `logConnection` to dependency bc function would be recreated everytime component renders
- could wrap in `useCallback` but the `useCallback` would still depend on `value` and `displayName` so everytime either changes it would rerender that function, which would then recall `useEffect`

So, problem with code is we only want to run `useEffect` when `url` changes, but ALSO want to use `value` of `displayName` inside of `useEffect`

- normally if you want to use the value of something inside `useEffect` you need to depend on that value
- so error (warning) is saying we are not depending on everything the `useEffect` is using

Best way to do this is experimental `useEffectEvent` hook

To use, need to be on experimental version of react...

- Kyles package.json...

![experimental React](/assets/img/useEffectEvent.png)

Using experimental version of react, react-dom, and eslint-plugin-react-hooks

Can install with

`npm i react@experimental`

Need to do for all 3 (react, react-dom, and eslint-plugin-react-hooks)

So now can do the following

```jsx
import {
  useEffect,
  experimental_useEffectEvent as useEffectEvent,
} from "react";

export function ChatRoom({ url, displayName }) {
  const logConnection = useEffectEvent((value) => {
    console.log(value, displayName);
  });

  useEffect(() => {
    const room = connectToRoom(url);

    room.onConnected(() => {
      logConnection(`Connected to ${url}`);
    });

    return () => room.disconnect();
  }, [url]);
}

function connectToRoom(url) {
  // ...
}
```

- uses `useEffectEvent`
- wrap that code from the function THAT WE ARE CALLING FROM `useEffect` IN `useEffectEvent`
- we give this `useEffectEvent` the function name

Now there are NO WARNINGS

- why?... `useEffectEvent` is saying we have this code, and it is essentially a side effect of an event I want to trigger inside `useEffect`
- `logConnection(`Connected to ${url}`)` is the EVENT we cant to happen when connected
- saying, we want to `logConnection(`Connected to ${url}`)` to be a side effect of `useEffect`, BUT WE DON'T WANT TO DEPEND ON ANYTHING MY `useEffect` DEPENDS ON - IT IS ENTIRELY SEPERATE
- we also need to pass in whatever we are passing from inside `useEffectEvent` (here it is `value` to recieve `Connected to ${url}`)

  - if we didn't, and just added `url` in the `console.log` there would be a bug in code
