# Advanced React Course

[React Simplified - Advanced](https://courses.webdevsimplified.com/view/courses/react-simplified-advanced)

## Portals

Portals are a way to take content inside React hierarchy and actually render it somewhere else in the DOM

- We can render into some other `div`, or at `body` or where we want

- Will use `createPortal()` to solve

Say we have `App.jsx` below

- The `AlertMessage` component probably shouldn't be wrapped in `<div>` with everything else.
- Styles in the `div` will effect where `AlertMessage` shows, and we don't want that

```jsx
export default function App() {
  const [isOpen, setIsOpen] = useState(false);

  return (
    <div style={{ position: "relative", marginTop: "100px" }}>
      <h1>App Content</h1>
      <button onClick={() => setIsOpen(true)}>Show Message</button>
      <AlertMessage isOpen={isOpen} onClose={() => setIsOpen(false)}>
        Secret Message
        <br />
        Click To Close
      </AlertMessage>
    </div>
  );
}

function AlertMessage({ children, onClose, isOpen }) {
  if (!isOpen) return null;

  return (
    <div
      onClick={onClose}
      style={{
        cursor: "pointer",
        position: "absolute",
        top: ".5rem",
        left: "50%",
        translate: "-50%",
        background: "#777",
        color: "white",
        borderRadius: ".5rem",
        padding: ".5rem",
      }}
    >
      {children}
    </div>
  );
}
```

To use `createPortal`

1.  import from `react-dom`

- `import { createPortal } from "react-dom";`
- Takes 2 paramenter.

  - first is everything you want to render (ex: `div`)
  - second it WHERE you want it to render

2.  Wrap everything you want to render in `createPortal()` function

```jsx
return createPortal(
  <div
    onClick={onClose}
    style={{
      cursor: "pointer",
      position: "absolute",
      top: ".5rem",
      left: "50%",
      translate: "-50%",
      background: "#777",
      color: "white",
      borderRadius: ".5rem",
      padding: ".5rem",
    }}
  >
    {children}
  </div>,
  document.body
);
```

So now, Secret Message shows at top of `body`, not affected by the `div` that `AlertMessage` is in

If you `inspect` page(when Show Message is clicked), you see that all the code (& styles) for `AlertMessage` is just appended inside of `body`

**`createPortal` allows you to take any dom you want and make it render somewhere else**

Few things to be aware of...

1.  Probably shouldn't be appending things directly to the body

- can cause performance issues, and a lot of libraries append to DOM so can get messy

- INSTEAD can go into `index.html` and make a dedicated `div` with `id="alertmessages"`

  `index.html`

```jsx
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Vite + React</title>
</head>
<body>
  <div id="root"></div>

  // add dedicated div for messages from portal
  <div id="alert-messages"></div>

  <script type="module" src="/src/main.jsx"></script>
</body>
</html>
```

- Now we can just send to `document.querySelector("#alert-messages")`

2.  Event Propogation is different for javascript & React

- (Normally in of javascript, if you click in an object (like inside of div) it bubbles up in the html (here in `index.html`). Here the `createPortal` code is inserted into body when clicked. Normally it would bubble up to alert messages `div`, then `body`, all the way up to `<html>` )

- BUT IN REACT, it will bubble up in the DOM in jsx where it is. So all the events that happen in `AlertMessage` (that returns the `createPortal`) bubble up to the `div` that holds `AlertMessage`

So final `createPortal` Example looks like...

`index.html`

```jsx
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Vite + React</title>
</head>
<body>
  <div id="root"></div>

  // add dedicated div for messages from portal
  <div id="alert-messages"></div>

  <script type="module" src="/src/main.jsx"></script>
</body>
</html>
```

`App.jsx`

```jsx
import React, { useState } from "react";
import { createPortal } from "react-dom";

export default function App() {
  const [isOpen, setIsOpen] = useState(false);

  return (
    <div style={{ position: "relative", marginTop: "100px" }}>
      <h1>App Content</h1>
      <button onClick={() => setIsOpen(true)}>Show Message</button>
      <AlertMessage isOpen={isOpen} onClose={() => setIsOpen(false)}>
        Secret Message
        <br />
        Click To Close
      </AlertMessage>
    </div>
  );
}

function AlertMessage({ children, onClose, isOpen }) {
  if (!isOpen) return null;

  return createPortal(
    <div
      onClick={onClose}
      style={{
        cursor: "pointer",
        position: "absolute",
        top: ".5rem",
        left: "50%",
        translate: "-50%",
        background: "#777",
        color: "white",
        borderRadius: ".5rem",
        padding: ".5rem",
      }}
    >
      {children}
    </div>,
    document.querySelector("#alert-messages")
  );
}
```

#### Note: Question to CodeGPT about "bubbling up"

"In this context, "bubble up" refers to the event propagation mechanism in HTML. When an event, such as a click, occurs on an element within the DOM, it is handled by that specific element's event listener.

However, if the event listener is not present on that element, the event will "bubble up" the DOM tree, triggering event listeners on ancestor elements. Eventually, it reaches the <html> element.

In the given context, it explains how the event bubbling is handled differently in React. Normally, in JavaScript, when an event occurs inside an element, it bubbles up through the DOM tree. But when using React's createPortal, the event does not bubble up beyond the target component. This behavior is different from the traditional event bubbling mechanism in HTML."

## Forwarding Refs

How to pass a `ref` to a custom component

If you are trying pass a `ref` to a custom component, need to wrap entire component in a `forwardRef()`.

For example, the code below works that way it is supposed to. We pass our `ref={inputRef}` in input

```jsx
import { useRef } from "react";

export default function App() {
  const inputRef = useRef();

  function handleSubmit(e) {
    e.preventDefault();
    console.log(inputRef.current.value);
  }

  return (
    <form onSubmit={handleSubmit}>
      <input ref={inputRef} style={{ border: "2px solid green" }} />
      <button type="submit">Submit</button>
    </form>
  );
}
```

But the code breaks if we try to **create a custom component**. Say we create a `CustomInput` and try to pass the `ref={inputRef}`

```jsx
export default function App() {
  const inputRef = useRef();

  function handleSubmit(e) {
    e.preventDefault();
    console.log(inputRef.current.value);
  }

  return (
    <form onSubmit={handleSubmit}>
      <CustomInput ref={inputRef} />

      <button type="submit">Submit</button>
    </form>
  );
}

function CustomInput(props) {
  return <input {...props} style={{ border: "2px solid green" }} />;
}
```

It looks like it should work, but **errors**. Saying many things, inclusing **can't pass ref to this component**.

- If you are trying pass a `ref` to a custom component, need to wrap entire component in a `forwardRef()`.
- Tells React that you are trying to pass a `ref` to this component, make sure it can accept it

There are 2 steps

1.  Need to wrap the custom component (like custom `input` component in `forwardRef`)

    - Usually make a new component, give it any name, then export your custom component name, which is your new component wrapped in `forwardRef`

2.  The wrapped componet receives a 2nd argument (after `props`) that receives `ref`

    - The in custom input, `ref={ref}`

NOTE: ALWAYS PUT `ref` AFTER `props` in arguments

`App.jsx`

```jsx
export default function App() {
  const inputRef = useRef();

  function handleSubmit(e) {
    e.preventDefault();
    console.log(inputRef.current.value);
  }

  return (
    <form onSubmit={handleSubmit}>
      <CustomInput ref={inputRef} />
      <button type="submit">Submit</button>
    </form>
  );
}
```

`CustomInput.jsx`

```jsx
// 1. Import forwardRef
import { forwardRef } from "react";

// 3. Receive 2nd argument of ref
function InnerComponent(props, ref) {
  // 4. in input, ref={ref}
  return <input {...props} ref={ref} style={{ border: "2px solid green" }} />;
}

// 2. export custom component (which is above function wrapped in forwardRef)
export const CustomInput = forwardRef(InnerComponent);
```

AGAIN: ALWAYS PUT `ref` AFTER `props` in arguments

## Modal Project

- More experience working with portals.
- A hard thing about React is implimenting libraries, APIs, etc, not made to work with React
- smaller project than others

#### "What is a modal in this context" - CodeGpt

"A modal is a UI component or element that is displayed on of the main content or page. It is used to present important information, require user input, or display notifications in a way that temporarily interrupts the user's interaction with the rest of the application.

Modal windows typically have a focused and centered design, appear as floating dialogs, and often have a dimmed or blurred overlay behind them to highlight the modal and prevent interaction with the underlying content.

Modal windows are commonly used for actions such as confirming or cancelling an action, displaying additional details or options, or gathering user input through forms or prompts."

In project will make `modal` in 2 ways

- 1. made with `div` to "look like" modal
- 2.  Made with newish `dialog` element

#### Custom modal

There are a lot of ways to make this work.

1. Logic for showing modal. Pass `isOpen` do `<div className={`modal-overlay ${isOpen && "show"}`}>`

- therefore, if `isOpen`, will add `class` of "show"

2.  Pass onClose logic for button

- `onClose={() => setIsCustomModalOpen(false)}`

3.  Set button `onClick={onClose}` in `CustomModal`

4.  Create somewhere in basic html for portal to show

- In `index.html` put `<div id="modal-container" ></div>`

5.  createPortal

- in CustomModal, wrap return in `createPortal` with following argument of destination (`#modal-container`)

`styles.css`

```css
.modal {
  position: absolute;
  top: 50%;
  left: 50%;
  translate: -50% -50%;
  padding: 1rem;
  background: white;
  border: 1px solid black;
  z-index: 1;
}

.modal-overlay.show {
  display: block;
}

.modal-overlay {
  display: none;
  position: absolute;
  inset: 0;
  background: rgba(0, 0, 0, 0.1);
}
```

`App.jsx`

```jsx
export default function App() {
  const [isCustomModalOpen, setIsCustomModalOpen] = useState(false);

  return (
    <div>
      <button onClick={() => setIsCustomModalOpen(true)}>
        Show Custom Modal
      </button>
      <br />
      <button onClick={() => setIsDialogModalOpen(true)}>
        Show Dialog Modal
      </button>
      <CustomModal
        {/* 1. pass isOpen */}
        isOpen={isCustomModalOpen}
        {/* 2. pass onClose with logic */}
        onClose={() => setIsCustomModalOpen(false)}
      />
    </div>
  );
}
```

`CustomModal.jsx`

```jsx
import { useEffect } from "react";
import { createPortal } from "react-dom";

// 1. receive isOpen
// 2. receive onCllose
export function CustomModal({ isOpen, onClose, children }) {
  useEffect(() => {
    function handler(e) {
      if (e.key === "Escape") onClose();
    }

    document.addEventListener("keydown", handler);

    return () => {
      document.removeEventListener("keydown", handler);
    };
  }, [onClose]);

  // wrap in createPortal
  return createPortal(
    {/* 1. add "show" if isOpen*/}
    <div className={`modal-overlay ${isOpen && "show"}`}>
      <div className="modal">
        <p>
          This is a <strong>CUSTOM</strong> modal
        </p>
        {/* add `onClick` to button */}
        <button onClick={onClose}>Close</button>
      </div>
    </div>,
    {/* 5. - add destination*/}
    document.querySelector("#modal-container")
  );
}
```

We are getting errors from ES Lint of no prop-types.

- can disable in `.eslintrc.cjs` with...

`.eslintrc.cjs`

```jsx
  rules: {
    "react/prop-types": "off",
  },
```

Pass `onClose` for logic of closing button

```jsx
export function CustomModal({ isOpen, onClose }) {
  return createPortal(
    <div className={`modal-overlay ${isOpen && "show"}`}>
      <div className="modal">{children}</div>
    </div>,
    document.querySelector("#modal-container")
  );
}
```

A NOTE on why we use createPortal...

- In our code, the `css` determines what the render will look like.
- BUT, say our `App` `div` had a style...

`<div style={{ position: "relative", marginTop: "20px"}}`

- If we did't wrap our `CustomModal` return in a `createPortal`, and it was trying to render in the `App` `div`, then the `div` style would totally mess up the modal - it would override the `css`
- SO, we use `createPortal` to render the `CustomModal` somewhere safe and neutral (like our `<div id="modal-container>` in `index.html`)

Now we need `Escape` key to close...

1.  Hook up `useEffect` in `CustomModal`
2.  ADD event listener
3.  REMOVE event listener for cleanup
4.  Create `function handler()` to ...

- take in event (e)
- call `onClose()` function (to close modal) when `e.key === "Escape"`

5.  Pass `handler` to event listener
6.  Add `[onClose]` to dependency list so run whenever onClose changes, AND everytime rerun, event listener removed and new one started

`CustomModal`

```jsx
export default function CustomModal({ children, isCustomOpen, onClose }) {
  // 1. Hook up useEffect for event listener
  useEffect(() => {
    // 4. create handler function
    function handler(e) {
      if (e.key === "Escape") onClose();
    }

    // 2. Add event listener
    // 5. Pass handler to event listener
    document.addEventListener("keydown", handler);

    return () => {
      // 3. Remove event listener (in `return`) for cleanup
      // 5. Pass handler to event listener
      document.removeEventListener("keydown", handler);
    };
  }, [onClose]);

  return createPortal(

    // .....
}
```

Now we pass `{children}` to make content dynamic

- passes content from `App`

1.  Receive `children` in `CustomModal` as prop
2.  In `CustomModal` use `{children}` in `createPortal` to receive content
3.  In `App` put content to pass as children
4.  Make it so `button` `onClick` just calls `setIsCustomOpen` function to close

`CustomModal`

```jsx
// 1. Receive children as prop
export default function CustomModal({ children, isCustomOpen, onClose }) {
  useEffect(() => {
    function handler(e) {
      if (e.key === "Escape") onClose();
    }

    document.addEventListener("keydown", handler);

    return () => {
      document.removeEventListener("keydown", handler);
    };
  }, [onClose]);

  return createPortal(
    <div className={`modal-overlay ${isCustomOpen ? "show" : ""}`}>
      {/* 2. add {children} */}
      <div className="modal">{children}</div>
    </div>,
    document.querySelector("#modal-container")
  );
}
```

`App.jsx`

```jsx
export default function App() {
  const [isCustomModalOpen, setIsCustomModalOpen] = useState(false);

  return (
    <div>
      <button onClick={() => setIsCustomModalOpen(true)}>
        Show Custom Modal
      </button>
      <br />
      <button onClick={() => setIsDialogModalOpen(true)}>
        Show Dialog Modal
      </button>
      <CustomModal
        isOpen={isCustomModalOpen}
        onClose={() => setIsCustomModalOpen(false)}
      >
        {/* 3. Add content to pass as children*/}
        <p>
          This is a <strong>CUSTOM</strong> modal
        </p>
        {/* 4. make onClick just call set function to close */}
        <button onClick={() => setIsCustomModalOpen(false)}>Close</button>
      </CustomModal>
    </div>
  );
}
```

#### Dialog modal

1.  Add `useState` for `Dialog Modal`
2.  Add button to call Show Dialog Modal
3.  Add `DialogModal` just like `CustomModal`

```jsx
export default function App() {
  const [isCustomModalOpen, setIsCustomModalOpen] = useState(false);
  // 1. Create useState for dialog
  const [isDialogModalOpen, setIsDialogModalOpen] = useState(false);

  return (
    <div>
      <button onClick={() => setIsCustomModalOpen(true)}>
        Show Custom Modal
      </button>
      <br />
      {/* 2. Create button to call Show Dialog Modal */}
      <button onClick={() => setIsDialogModalOpen(true)}>
        Show Dialog Modal
      </button>
      <CustomModal
        isOpen={isCustomModalOpen}
        onClose={() => setIsCustomModalOpen(false)}
      >
        <p>
          This is a <strong>CUSTOM</strong> modal
        </p>
        <button onClick={() => setIsCustomModalOpen(false)}>Close</button>
      </CustomModal>

      {/* 3. Add `DialogModal` just like CustomModal */}
      <DialogModal
        isOpen={isDialogModalOpen}
        onClose={() => setIsDialogModalOpen(false)}
      >
        <p>
          This is a <strong>DIALOG</strong> modal
        </p>
        <button onClick={() => setIsDialogModalOpen(false)}>Close</button>
      </DialogModal>
    </div>
  );
}
```

1. Create `DialogModal.jsx`
2. Pass our props
3. Create `<dialog>` in `return` and receive children
4. Do our createPortal
5. Add argument to pass to out "#modal-container" in `index.html`

`DialogModal`

```jsx
// 1 & 2 - create and pass props
export function DialogModal({ isOpen, onClose, children }) {

//  4. createPortal
  return createPortal(
    {/* 3. Create dialof element and receive children*/}
    <dialog ref={dialogRef}>{children}</dialog>,
    {/* 5. add argument to pass to modal-container div*/}
    document.querySelector("#modal-container")
  )
}
```

Now to make `dialog` work, we DO NOT just add a class attribute like "show"

- we need to somehow call `.showModal` or `.close()`
- `dialog` element is turned on and off this way
- THIS IS NOT A REACT WAY OF DOING THINGS - IT IS MORE IMPERATIVE TO CALL CODE

So...

1.  Create `useEffect` that runs whenever `isOpen` changes
2.  If isOpen is true, then `showModal`, if not, `close`

- Taking state variable (`isOpen`) and hooking up to state of dialog element

3.  We need a `ref` to state of `dialog` - create `useRef` and add to `dialog`
4.  set to `null` by default - means it's unchangable / immutable
5.  Set a variable `dialog` to the current state of out ref

- MY NOTE: by setting it to null, we can safely check if current is null without accidentaly changing it

6. If dialog is `null`, then `return` so we don't do anything yet

`DialogModal`

```jsx
import { useEffect, useRef } from "react";
import { createPortal } from "react-dom";

export function DialogModal({ isOpen, onClose, children }) {
  // 3. create ref dor dialof state
  // 4. set to null
  const dialogRef = useRef(null);

  // 1. Create useEffect that runs whenever `isOpen` changes
  useEffect(() => {
    // 5. Set a variable `dialog` to the current state of out ref
    const dialog = dialogRef.current;

    // 6. If dialog is null, return so we don't do anything yet
    if (dialog == null) return;

    // 2. if isOpen is true, then showModal, if not, close
    if (isOpen) {
      dialog.showModal();
    } else {
      dialog.close();
    }
  }, [isOpen]);

  //....

  return createPortal(
    {/* 3. add ref for dialog state*/}
    <dialog ref={dialogRef}>{children}</dialog>,
    document.querySelector("#modal-container")
  );
}
```

Now, `dialog` element (for modals) will close automatically on `Escape`. But this is not what React expects, so we need to sync up what it expects with what js does by default.

- As it is, if we hit Escape it closes, but can't reopen bc `isOpen` is still true!!!
- To do this takes ANOTHER `useEffect`

With next `useEffect`, we just want to check WHEN DIALOG CLOSES. So when it closes, it CALLS onClose METHOD!! `onClose` will reset `isOpen` to false

So event listener is running, listening for a close. when it sees a close, it calls `onClose` function, which sets `isOpen` to false

- when this happens, it runs the `useEffect` again to cleanup and make another event listener

1.  Add second useEffect
2.  set const dialog to current state of dialog element
3.  Check if null (then don't do anything)
4.  CREATE event listener THAT LISTENS FOR A `close`. If so, runs `onClose`
5.  Set cleanup to remove event listener

`DialogModal`

```jsx
export function DialogModal({ isOpen, onClose, children }) {
  const dialogRef = useRef(null);
  useEffect(() => {
    const dialog = dialogRef.current;
    if (dialog == null) return;

    if (isOpen) {
      dialog.showModal();
    } else {
      dialog.close();
    }
  }, [isOpen]);

  // 1
  useEffect(() => {
    // 2
    const dialog = dialogRef.current;
    // 3
    if (dialog == null) return;

    // 4
    dialog.addEventListener("close", onClose);

    // 5
    return () => {
      dialog.removeEventListener("close", onClose);
    };
  }, [onClose]);

  return createPortal(
    <dialog ref={dialogRef}>{children}</dialog>,
    document.querySelector("#modal-container")
  );
}
```

NOTE on DIALOG element

- the code for this is more confusing and longer
- this is because it is not built with React in mind
- it is not built in DECLARITIVE way - it is built in more IMPERRATIVE way
- some libraries will be difficult like this bc not built for React

  - RECOMMENDATION: wrap in a componment or custom hook to hide ugly logic

- otherwise the custom and dialog modals are identical

## Error Boundries

There will always be errors that happen at some point - these can get pushed up to production.

- Deal with by setting up error boundries
- If you have a error ANYWHERE in your rReact code that has to do with rendering page, whole app will just be a white screen because nothing happens

- say you have `App` & `Child` below
- say we make `Child throw an error`

`App`

```jsx
import Child from "./Child";

export default function App() {
  return (
    <>
      <h1>Parent</h1>
      <Child />
    </>
  );
}
```

`Child`

```jsx
export default function Child() {
  throw new Error("Component");

  return <h2>Child</h2>;
}
```

- This crashes our React code. All of the help in console says do Error Boundries

Error Boundries...

1.  Error Boundry is just a component
2.  It MUST BE A CLASS COMPONENT (the only thing in React you must do as a class component)
3.  ErrorBounary class component can be called whatever you want
4.  WRAP ENTIRE APPLICATION in ErrorBoundry
5.  Give ErrorBoundary a `fallback` that is can actually render

    - Can be a component or just some jsx
    - Here is is `fallback={<h1>Error</h1>}`

6.  In ErrorBoundray class need static method to `static getDerivedStateFromError(error)`

    - the method receives `error`
    - sets `{hasError: true}`
    - (my note... codegpt) "In React class components, static methods are associated with the class itself rather than an instance of the class. They can be called directly on the class without needing to create an instance of the component."
    - (my note... codegpt) "It's important to note that getDerivedStateFromError is a static method and does not have access to this or the component instance. It is intended to be used purely for updating the state based on an error and should not perform any side effects or change other component behavior."

7.  Now if there is an error, it calls ErrorBoundary (b/c `<App/>` is wrapped in the class component)

    - then methos in ErroryBoundary changes `hasError: true`
    - Now does the `fallback` that was passed (like a prop)

`ErrorBoundary`

```jsx
import React from "react";

export class ErrorBoundary extends React.Component {
  state = { hasError: false };

  static getDerivedStateFromError(error) {
    return { hasError: true };
  }

  render() {
    if (this.state.hasError) {
      return this.props.fallback;
    }
    return this.props.children;
  }
}
```

`main`

```javascript
ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <ErrorBoundary fallback={<h1>Error</h1>}>
      <App />
    </ErrorBoundary>
  </React.StrictMode>
);
```

So, what happens is when there is an error in React, it starts going up tree looking at all parent levels until it finds an error boundry

- so Child to App to main and finds error boundary
- passes error to Error Boundry
- runs `static getDerivedStateFromError(error)` method
- sets `hasError: true`
- renders the `fallback` "prop" INSTEAD OF RENDERING THE CHILDREN

Now we can do this wherever we want

- for example, we could have error boundary that just wraps child component

```jsx
export default function App() {
  return (
    <>
      <h1>Parent</h1>
      <ErrorBoundary fallback={<h1>Error in child</h1>}>
        <Child />
      </ErrorBoundary>
    </>
  );
}
```

- here it DOES render `Parent` but then error in `Child` causes `fallback` to render "Error in child"
- This is bc THIS Error Boundry is closer up tree than one in `main`

Best Practices...

- at LEAST wrap entire application in error boundry
- then add error boundarys only in places in App you might actually want them
- For example, in a shopping cart website, maybe wrap the shopping cart in an error boundry. So if someonetriesto open shopping cart and it fails, you just get one error for that

Error Boundry class takes a second method, `componentDidCatch()`

- used for logging errors. Can log to a service or db. Here we just console.log

```jsx
export class ErrorBoundary extends React.Component {
  state = { hasError: false };

  static getDerivedStateFromError(error) {
    return { hasError: true };
  }

  componentDidCatch(error) {
    console.log("Error: ", error.message);
  }

  render() {
    if (this.state.hasError) {
      return this.props.fallback;
    }
    return this.props.children;
  }
}
```

This works great if catching error with way React renders, etc. Like if error is in a `useEffect` as follows, it will catch

`Child`

```jsx
export default function Child() {
  useEffect(() => {
    throw new Error("Component");
  });

  return <h2>Child</h2>;
}
```

BUT, there can be some errors Error Bounries don't catch

1.  Error in promiise / async
2.  error in setTimeout

3.  Watch outfor async code / awaiting promise. The following doesn't catch error

```jsx
export default function Child() {
  useEffect(() => {
    fetch("/").then(() => {
      throw new Error("Component");
    });
  });

  return <h2>Child</h2>;
}
```

- should use `.catch` to do something like this instead

```jsx
export default function Child() {
  useEffect(() => {
    fetch("/")
      .then(() => {
        throw new Error("Component");
      })
      .catch((e) => setError(e));
  });

  return <h2>Child</h2>;
}
```

2.  Watch for things like `setTimeout` - this won't call error boundry

```jsx
export default function Child() {
  useEffect(() => {
    const timeout = setTimeout(() => {
      throw new Error("Timeout");
    }, 1000);

    return () => {
      clearTimeout(timeout);
    };
  }, []);

  return <h2>Child</h2>;
}
```

SUMMARY... Error Boundary's will ONLY CATCH errors that happen bc of the RENDERING STEPS IN REACT. If it's asynchronous or in an event listener, won't catch that (BC THOSE DON'T CAUSE APP TO COMPLETELY FAIL ND GO WHITE SCREEN)

- if you want to handle those type that it won't catch, recommend that you have some way to check for and handle those error messages (like the `.catch` for example)

## Advanced Key Uses

Confusing topic is how React handles state preservations between rerender

Take the following code that lets you switch between Cats and Dog, and EACH CALLS ITS OWN UNIQUE COUNTER

`App`

```jsx
import { useState } from "react";
import Counter from "./Counter";

export default function App() {
  const [changeDogs, setChangeDogs] = useState(false);

  return (
    <div>
      {changeDogs ? (
        <>
          <span># of Dogs:</span>
          <Counter />
        </>
      ) : (
        <>
          <span># of Cats:</span>
          <Counter />
        </>
      )}
      <br />
      <button onClick={() => setChangeDogs((d) => !d)}>Switch</button>
    </div>
  );
}
```

`Counter`

```jsx
import { useState } from "react";

export default function Counter() {
  const [count, setCount] = useState(0);

  return (
    <>
      <button onClick={() => setCount((c) => c - 1)}>-</button>
      {count}
      <button onClick={() => setCount((c) => c + 1)}>+</button>
    </>
  );
}
```

In this example, the render logic in `App` allows swithing between cats & dogs

- BUT even though each has it's own unique `Counter`, the `Counter` state is being shared. WHY??

Demonstrates HOW React knows which `Counter` is which

- React determines which `Counter` state is associated by LOOKING AT WHERE IT IS IN THE TREE
- Thats how React determines state. Not which `Counter` state it is, but WHERE it is in tree
- Both `Counter`s look the same in the tree - both have a `<div>` parent, then `Counter` as next element. So to React, these are the same `Counter`

Need a way to take care of this

- One way is to have the DOM be different for each. Like if you changed the Dogs `<>` to `div` and Cats `<>` to a `section` each `Counter` would be correct. The Dog one would be `div` then `div` then `Counter` component. The cat one would be `div` then `section` then `Counter`.

`App`

```jsx
//...
  return
    <div>
      {changeDogs ? (
        <div>
          <span># of Dogs:</span>
          <Counter />
        </div>
      ) : (
        <section>
          <span># of Cats:</span>
          <Counter />
        </section>
      )}
      <br />
      <button onClick={() => setChangeDogs((d) => !d)}>Switch</button>
    </div>
  );
}
```

(Note though that counter goes back to 0 when switch - need a diff way to keep state between renders)

KEYS are the way to handle this

- We know we can use keys determine which element of an array is corresponding to which element in the DOM
- Can use keys to distinguish different elements if rendered in the same place

`App`

```jsx
export default function App() {
  const [changeDogs, setChangeDogs] = useState(false);

  return (
    <div>
      {changeDogs ? (
        <>
          <span># of Dogs:</span>
          <Counter key="dogs" />
        </>
      ) : (
        <>
          <span># of Cats:</span>
          <Counter key="cats" />
        </>
      )}
      <br />
      <button onClick={() => setChangeDogs((d) => !d)}>Switch</button>
    </div>
  );
}
```

Just by adding different keys to each `Counter`, when we Switch, the state changes to the other animals (note hat it resets to zero bc not ppersisting this anywhere, but this is just example)

- So with the keys, React can now KEEP TRACK of which `Counter` is which

Now let's change code for another way to use keys

`App`

```jsx
export default function App() {
  const [changeDogs, setChangeDogs] = useState(false);

  return (
    <div>
      {changeDogs ? <span># of Dogs:</span> : <span># of Cats:</span>}
      <br />
      <input type="number" />
      <br />
      <button onClick={() => setChangeDogs((d) => !d)}>Switch</button>
    </div>
  );
}
```

- when we enter number on nput, then hit "Switch", the number is being persisted
- This makes sense. The position doesn't change, etc
- WE CAN GIVE INPUT A KEY, DETERMINED CONDITIONALLY

```jsx
export default function App() {
  const [changeDogs, setChangeDogs] = useState(false);

  return (
    <div>
      {changeDogs ? <span># of Dogs:</span> : <span># of Cats:</span>}
      <br />
      <input type="number" key={changeDogs ? "dogs" : "cats"} />
      <br />
      <button onClick={() => setChangeDogs((d) => !d)}>Switch</button>
    </div>
  );
}
```

- The value now switched with "Switch"
- (It still goes back to empty on Switch back bc when switched back seesit as a brand new element
- Again, we use this key here bc without it, since `input` is in the exact same place in tree, it can't differentiate on re-render.
- Can use keys anywhere you want

99% of time you won't need to do this. Only need when elements are showing up in exact same place in tree so need to differentiate them

- if using keys this way, take second and think about if code is written in weird way

## Capture Event Listeners

Event listeners have two phases

- capture
- bubble

When you set an event listener, the default is BUBBLE phase

- capture event listeners run from outtermost to innermost

  - capture runs first

- with bubble phase, it runs from innermost elements up through tree to outermost

  - bubble runs second

So you can choose `bubble` or `capture` (bubble is default when set an event listener)

When you set `onClick`, as in a button, that is an event listener set in bubble phase

You can use `onClickCapture` to set in capture phase

"BY DEFAULT, EVERY EVENT IS A BUBBLE EVENT"

Any event can be a capture. Just add "Capture" at the end

Example:

----- CodeGPT says:
""
Capture and bubble are two different phases of event propagation in JavaScript.

When an event is triggered on a DOM element, it doesn't only affect that specific element but also its parent elements. During the capture phase, the event is first triggered on the highest-level parent element and then propagated down to the target element, following the nesting structure of the DOM tree. After the capture phase, the event enters the target phase and then starts the bubble phase.

Capture event listeners are event handlers that are executed during the capture phase. They are registered using the addEventListener method with the third parameter set to true:

Javascript

`element.addEventListener(eventType, eventHandler, true);`

On the other hand, bubble event listeners are event handlers that are executed during the bubble phase. They are registered using the addEventListener method with the third parameter set to false (this is the default behavior if the parameter is not specified):

Javascript

`element.addEventListener(eventType, eventHandler, false);`

In most cases, when registering event listeners, you would use the bubble phase because it allows you to handle events as they bubble up through the DOM tree. This way, you can handle events on various elements with a single event listener, which is more efficient than adding separate event listeners to each individual element.

Here's a simple example to demonstrate event capturing and bubbling:

```javascript
<div id="outer">
  <div id="inner">Click me</div>
</div>
```

```javascript
const outer = document.getElementById("outer");
const inner = document.getElementById("inner");

outer.addEventListener(
  "click",
  () => {
    console.log("Capture phase - outer element");
  },
  true
);

inner.addEventListener("click", () => {
  console.log("Bubble phase - inner element");
});
```

In this example, if you click on the "inner" element, the console output will be:

```javascript
Capture phase - outer element
Bubble phase - inner element
```

First, the capture event listener on the "outer" element is called, then the bubble event listener on the "inner" element is called.
""

----- END CODEGPT
