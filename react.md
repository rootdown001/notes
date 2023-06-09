# JSX Code

1. Can't be run in the browser - gets converted to a different syntax of just plain javascript code. for example...

```jsx
import React from "react";

function App() {
  return <h1 id="test">Hello World</h1>>;
}

export default App;
```

GETS CONVERTED TO....

```jsx
import React from "react";

function App() {
  return React.createElement("h1", { id: "test" }, "Hello World");
}

export default App;
```

</br>

# Difference between HTML & JSX

- only use dash in jsx with data- or aria-

| **Topic**                               | **HTML**                | **React**                                                              |
| --------------------------------------- | ----------------------- | ---------------------------------------------------------------------- |
| **Attributes**                          | dash                    | camelCase                                                              |
| - _example_ -                           | - _tab-index_ -         | - _tabIndex_ -                                                         |
| **Custom Data Attributes ( data-... )** | data-...                | data-...                                                               |
| - _example_ -                           | - _data-date_ -         | - _data-date_ -                                                        |
| **ARIA Attributes (aria-...)**          | aria-...                | aria-...                                                               |
| - _example_ -                           | - _aria-label_ -        | - _aria-label_ -                                                       |
| **class attribute**                     | class                   | className                                                              |
| - _example_ -                           | - _class="project"_ -   | - _className="project"_ -                                              |
| **for attribute**                       | for                     | htmlFor                                                                |
| - _example_ -                           | - _for="id"_ -          | - _htmlFor="id"_ -                                                     |
| **style attribute**                     | style = "...:...;"      | style={{ : }}                                                          |
|                                         | note:                   | In React, the style is passed as object.                               |
| - _example_ -                           | - style="color:blue;" - | - style={{ color: "blue" }} -                                          |
|                                         | note:                   | Also, anything that would be in " " (that isn't a string), are in { }. |

</br>

# Components

1. Components need to start with a capital letter (this is how React knows it is a component)
2. You can change name of what to call a component by using "as". Ex... `import { ToDoList as List } from "./ToDoList"`
3. Can define multiple components in same file

</br>

# Props

1. Pass props without commas... `<NameFunc name="Lance" age={54} />`
2. The component recieves as `props`... `export function NameFunc(props){...}`
3. Props is actually an obect, so call it as `props.name` or `props.age`
4. Can recieve props destructured... `export function NameFunc({name, age}){...}`
5. Can set a default in the destructuring... `export function NameFunc({name, age = 54}){...}`
6. If you pass a boolean prop without a value, it defaults to `true`
7. If you pass a child, you access with `props.children`. `children` is a special term in React for this purpose

Example, if you pass a child like this...

```jsx
function App() {
  return (
    <div>
      <NameFunc>
        <span>Child Name</span>
      </NameFunc>
    </div>
  );
}
```

Then you can receive and refer to as

```jsx
export function NameFunc(props) {
  return <div>{props.children}</div>;
}
```

OR

```jsx
export function NameFunc({ children }) {
  return <div>{children}</div>;
}
```

</br>

# Declarative vs. Imparative (High Level Concept)

1. React is declarative

- You declare what your code output **should look like**
- You DO NOT do a step by step guide in React
- Things only change in React if data / state changes

2. Javascript, for example, is imparative

- Foucused on the step-by-step

</br>

# Importing Non-JS files

- Just import into `main.jsx` or `App.jsx`. For example,

```jsx
import "./styles.css";
```

- Can also import json and use as the json object. For example,

```jsx
import user from "./user.json";

function App() {
  return <h1>{JSON.stringify(user)}</h1>;
}
```

- Can import images and use as source

```jsx
import img from "./Code.png";

function App() {
  return <img src={img} />;
}
```

</br>

# Conditional Rendering

**Remember: Ternary Operator**

Good to use, but ONLY IF returning your result (not if doing functions, etc, in result)

- Do not nest ternary operators

**Remember: Short Circuiting**

1. For && or ||, they only evaluate second part depending on beginning

- True || … never evaluates … bc true || is already true.
  This is bc true || true = true AND true || false = true
- False && … never runs the … This is bc false && true = false AND false && false = false

2. Short circuting used in rendering in React.

- Example…
  {favoriteNumber != null && `My Favorite number is ${favoriteNumber}}
- If number is not null, beginning evaluates to true, so you end up with 2nd part being evaluated.
- Therefore, if true &&, you get the second part. If false&&, you get nothing
- You can also inline ternary in react rendering

</br>

# Rendering Lists

- React cant render objects. Get an error
- Can work with arrays [1,2,3], **BUT** not if objects in it
- With an array, react tries to render element ex... 123
- Need to have objects in array, and use map on array to render out objects
- Problem: in react it tries to figure out which element altering by index.
- NEED TO PASS KEY PROP to the first element you return.. - Behind the scenes, react is hooking up unique keys with elements of array. Example

```jsx
{
  items.map((item) => {
    return (
      <div key={item.id}>
        {item.name}
        <input type="text" />
      </div>
    );
  });
}
```

- Here the `<div>` gets the key prop.
- This way react can keep track of what corresponds to returned element.
- In this example, react knows that return `<div>` … `</div>` corresponds to `{item.id}`
- Use `key={...}` in the element
- Key ONLY HAS TO BE UNIQUE FOR EACH INSTANCE. Therefore, can do the following and it works bc each `.map` loop is single instance

```jsx
{
  items.map((item) => {
    return (
      <div key={item.id}>
        {item.name}
        <input type="text" />
      </div>
    );
  });
}

{
  items.map((item) => {
    return (
      <div key={item.id}>
        {item.name}
        <input type="text" />
      </div>
    );
  });
}
```

- if you are passing to a child component, `key=` goes in the passing of child component

```jsx
{
  items.map((item) => {
    return <Item key={item.id} />;
  });
}
```

</br>

# Some React / JSX rules

1. You can set jsx to a valiable.

- `const myVariable = <h2>Hi there</h2>`
- This is referenced in the render as `{myVariable}`

2. See "Render Raw HTML" below for **inserting HTML as a string**
3. Cannot pass multiple elements in React render. You can if you wrap them all as nested elements in an outer element (or fragment)
4. Can render out string, number, array, null, undefined, boolean

- **But** `null`, `undefined`, and `false` don't render anything
  - We use this feature a lot in rendering

5. Components need to start with a capital letter (this is how React knows it is a component)
6. You can change name of what to call a component by using "as". Ex... `import { ToDoList as List } from "./ToDoList"`
7. Can define multiple components in same file

```jsx
 { myVariable } != 1 && `${myVariable} is not equal to 1`
```

</br>

# Diffence between React function and class

| **Class**               | **Function**            |
| ----------------------- | ----------------------- |
| export class {...}      | export function() {...} |
| extends React.Component | --                      |
| render() { return () }  | return ( )              |

</br>

# Virtual DOM

| **3 steps on render**                                            |
| ---------------------------------------------------------------- |
| 1. runs component & children (that is has stored in memory)      |
| 2. Compare virtual DOM to DOM                                    |
| 3. Just make changes in DOM that were different from virtual dom |

</br>

# Lifecycle

| **Lifecyle** | **When runs...**                                                                                               |
| ------------ | -------------------------------------------------------------------------------------------------------------- |
| Mounting     | First time component is rendered to page                                                                       |
| Update       | Every re-render after forst - only happend when state of a component changes, or its parent is re-rendered     |
| Unmounting   | When component is removed from the page - note: after unmounting then mounting again, state is back to initial |

</br>

# useEffect Hook

- react functions should be PURE functions (no side effects outdide of them)
- useEffect allows you to do things that change side effects but keeping pure function

### **useEffect(fx, [dependency array])**

- useEffect takes a function. 2nd parameter is an array of values.

1. Calls that function everytime a value from the array changes
2. If you only want it to run on MOUNT, use empty array [] as dependency array
3. Use useEffect any time want to change dom, like document title
4. Use return () for cleanup (ex removing event listener) - it runs cleanup THEN the above part of function
5. When component is un-mounted, all cleanup is run
6. REMEMBER, each time rendered, new object - remember ref vs value. 7. Everytime rerendered creates now object that you have made. May have same infor, but will have diff reference value
7. useEffect compares with ===
8. For API do in useEffect. fetch .then .then etc .catch .finally

</br>

# Class component lifecyle

1. componentDidMount
2. componentWillUnmount
3. componentDidUpdate

- only runs on update (unlike useEffect which runs on mount & update)
- pass prevProps and prevState for comparison
- No dedicated cleanup function - need to add, ex, remove event listener AND event listener so u run cleanup first
- If you are going to use eventListener, etc that get passed a fxn, keep fxn the same and put up in constructor

</br>

# Strict Mode

1. On mount, it does mount, unmount, mount very fast to look for code
2. Helps you look for bugs.
3. For example, with event listeners, by doing mount - unmount - mount it will show if NOT CLEANEDUP by console.loging twice
4. Double executes many things to look for bugs, using not pure functions
5. Checks for depricated code
6. StrictMode ONLY RUNS IN DEVELOPMENT (not in build)

</br>

# APIs

1. Create state for data (ex: const [users, setUsers] = useState()
2. Create state for error (ex: const [error, setError] = useState()
3. Create state for loading (ex: const [loading, setLoading] = useState(true)

**Do in useEffect**

4. setLoading(true) //Set loading to true each time api called
5. setError(undefined) // set error to undefined each time api called
6. setData(undefined) // set data to undefined each time api called
7. Const controller = new AbortController() // create new AbortController object, used to cancel fetch // used for cleanup // it is saying that anytime fetch is called, it cancels previous fetch from before re-mounting and starts new fetch
8. fetch(url, { signal: controller.signal} ) // fetch the api url // signal links controller to our fetch request
9. .then(res => { // eval result for successful or error…

- if (res.status === 200) { return res.json() } // is successful, parse json
- Else { return Promise.reject(res) } // then will get error below in catch

9. .then(data => setUsers(data)) // set returned json to the chosen state… here it is users
10. .catch(e => { // start catch

- If (e?.name === “AbortError”) return // if name of error is AbortReturn, do nothing bc we already took care of, manually aborted
- setError(e) // sets error state to error (if not manually aborted bc that was ONLY TO CANCEL PREVIOUS FETCH

11. .finally(() => {setLoading(false)}) // indicated we are done loading (so loading jsx can change)

12. Return () => { controller.abort() } // REMEMBER THAT IN USEEFFECT THE RETURN AT THE END IS CLEANUP… this is cancelling previous fetch before calling fetch
13. ,[] // use empty dependency array for useEffect so only calls on mount

**THEN FOR UI**

1. let jsx; // define variable

- If (loading) { jsx = <h2>Loading…</h2> } // show “Loading…” on screen
- Else if {error != null) {jsx = <h2>Error</h2> // show error if error
- Else <h3>{ jsx = JSON.stringify(data) }</h3> // if done loading, set variable to stringified JSON

2. Return ( { jsx } ) // render variable to screen

**Note**

- Can go to network in chrome dev to slow down speed to see loading
- “The signal” is a javascript built in that is used to not call .then twice (bc fetch isnt being cancelled if called then recalled) – USED FOR CLEANUP

## API code block

```jsx
import { useState, useEffect } from "react";

function App() {
  const [data, setData] = useState();
  const [error, setError] = useState(undefined);
  const [loading, setLoading] = useState(undefined);

  useEffect(() => {
    //Set loading to true each time api called
    setLoading(true);
    // set error to undefined each time api called
    setError(undefined);
    // set data to undefined each time api called
    setData(undefined);

    // create new AbortController object, used to cancel fetch.
    // used for cleanup. it is saying that anytime fetch is called,
    // it cancels previous fetch from before re-mounting and starts new fetch
    const controller = new AbortController();

    // fetch the api url
    // signal links controller to our fetch request
    fetch("https://jsonplaceholder.typicode.com/users", {
      signal: controller.signal,
    })
      .then((res) => {
        // returnparsed json if 200
        // elsereturn promise.reject
        if (res.status === 200) {
          return res.json();
        } else {
          return Promise.reject(res);
        }
      })
      // setData to return
      .then((d) => {
        setData(d);
      })
      .catch((e) => {
        // if name of error is AbortReturn, do nothing bc we already took care of, manually aborted
        if (e?.name === "AbortError") return;
        setError(e);
      })
      // indicated we are done loading (so loading jsx can change)
      .finally(() => {
        setLoading(false);
      });

    // cleanup. Abort previous fetch
    return () => {
      controller.abort();
    };
  }, []);

  let jsx; // define variable

  // show “Loading…” on screen
  if (loading) {
    jsx = <h2>Loading…</h2>;
  }
  // show error if error
  else if (error != null) {
    jsx = <h2>Error</h2>;
  }
  // if done loading, set variable to stringified JSON
  else {
    jsx = <h3>{JSON.stringify(data)}</h3>;
  }

  return (
    <>
      <h1>User List</h1>
      {/* show jsx */}
      <div>{jsx}</div>
    </>
  );
}

export default App;
```

</br>

# Fragments

A way to return multiple elements without haing to wrap in a `<div>`

1. They are just an empty element

```jsx
<>// jsx inside</>
```

2. If you need to define a `key=` in a fragment, you need to define the fragment with React.Fragment

- need to import React, or import { Fragment }

```jsx
import React from "react";
// OR.....
import { Fragment } from "react";

{
  items.map((item) => {
    return <React.Fragment key={item.id}>{item.name}</React.Fragment>;
  });
}
```

- Above we used `<> </>` instead of `<div> </div>`
- Beacause we put `key=` instde, we defined fragment with `<React.Fragment>`

</br>

# Spread Props

To pass properties of an object in React, you can pass EACH property like this (where you name each property)...

```jsx
{
  items.map((item) => {
    return (
      <>
        <User
          key={item.id}
          name={item.name}
          phone={item.phone}
          address={item.address}
        />
      </>
    );
  });
}
```

**OR** use the **Spread Prop** where you pass the properties with the spread operator...

```jsx
{
  items.map((item) => {
    return (
      <>
        <User key={item.id} {...item} />
      </>
    );
  });
}
```

- This passes **EACH** property in the object
- Useful IF THEY MAP ONE TO ONE
- Then in the receiving component, you receive what props you want with property name in object

```jsx
export function User({ name }, { phone }, { address }) {
  return (
    <li>
      {name}, {phone}, {address}
    </li>
  );
}
```

</br>

# Render Raw HTML

## **dangerouslySetInnerHTML**

This is used to set cutom HTML into a React component

1. **Never use if HTML allows input** because someone could insert a malicious script into your HTML
2. Use to inject a custom HTML
3. It is made difficult & convoluted because it's something you almost never want to do
4. But sometimes you only have your HTML in **string format** and you need to be able to render
5. Format is `dangerouslySetInnerHTML={{ __html: ... }}`

- It goes inside of an element (like an attribue in a div)
- It accepts an object where key is `__html` (2 underscores)
- Note: there are double `{{ }}` because the outer says it's javascript, and the inner says it's an object
- The HTML it accepts is a string (backticks around custom html)
- It cannot accept children (no text or spaces in between `<div></div>`)

```jsx
import React from "react";

const CUSTOM_HTML = `  
<h1>Hi</h1>
<div>My name is Lance</div> `;

function App() {
  return <div dangerouslySetInnerHTML={{ __html: CUSTOM_HTML }}></div>;
}

export default App;
```

# Input Event Listeners

1. Inside element tag, use "on..." for action you want

- `onClick`, `onMouseDown`, etc.
- Pass a function (not call function)

```jsx
<h1 onClick={passFunction}>Click Me</h1>
```

2. For input, use `onChange` event

- Anything you do in input field fires `onChange` with EACH change
- With `onChange` you pass an event
- then change state (ex: `setName`) by setting equal to `e.target.value`
- set value equal to state. Ex: `value={name}`. You can't change this by typing in field bc declarative
- can use `defaultValue` is want. Can change this by typing, but should actually do as below

```jsx
<input type="text" value={name} onChange={(e) => setName(e.target.value)} />
```

- the way to read this is it sets the value to `{name}`. Typing in input field fires `onChange`, which calls `setName` to make `name` equal to target value. Then field shows value of `name` because of `value={name}`
- This is considered a **controlled input**, bc react has control but not browser
- If you use `defaultValue={name}` then react does not have control (browser does). This would be **uncontrolled input**.

</br>

# Arrays & State Notes

```jsx
const INITIAL_VALUE = ["A", "B", "C"];
const [array, setArray] = useState(INITIAL_VALUE);
```

1. When using arrays and state, you need ability to have access to **current array**
2. Because state is **immutable** need to NOT do any state mutation
3. Can use `slice` (NOT `splice`) because `slice` doesn't change array

### Remove First Element

- Pass function to `onClick`. Ex: `onClick={removeFirstElement}`

```jsx
function removeFirstElement() {
  setArray((currentArray) => {
    return currentArray.slice(1);
  });
}
```

### Remove Specific Letter

- Pass function to `onClick`. Ex: `onClick={removeSpecificLetter("B")}`

```jsx
function removeSpecificLetter(letter) {
  setArray((currentArray) => {
    return currentArray.filter((element) => element !== letter);
  });
}
```

### Add Letter to Start

- Pass function to `onClick`. Ex: `onClick={addLetterToStart("B")}`
- Use spread opperator becuse we DON"T EDIT `array`
- Also we don't use `push()` for same reason

```jsx
function addLetterToStart(letter) {
  setArray((currentArray) => {
    return [letter, ...currentArray];
  });
}
```

### Add Letter to End

- Pass function to `onClick`. Ex: `onClick={addLetterToEnd("B")}`

```jsx
function addLetterToEnd(letter) {
  setArray((currentArray) => {
    return [...currentArray, letter];
  });
}
```

### Clear Array

- Pass function to `onClick`. Ex: `onClick={clear}`

```jsx
function clear() {
  setArray([]);
}
```

### Reset Array

- Pass function to `onClick`. Ex: `onClick={reset}`

```jsx
function reset() {
  setArray(INITIAL_VALUE);
}
```

### Update All of One Letter to Another

- Pass function to `onClick`. Ex: `onClick={updateAtoH}`
- using `map()` on `currentArray`. Returns each element as is to new array unless `element === "A"`

```jsx
function updateAtoH() {
  setArray((currentArray) => {
    return currentArray.map(element => {
      if (element === "A") return "H"
      return element
    }
  });
}
```

### Add Value to Array from Input

- need to make sure `value` in state does not start as `undefined` or it becomes **uncontrolled input**
- Therefore `value` starts as ""

```jsx
import { useState } from "react";

const INITIAL_VALUE = ["A", "B", "C"];

function App() {
  const [array, setArray] = useState(INITIAL_VALUE);
  const [value, setValue] = useState("");

  function addLetterToStart(letter) {
    setArray((currentArray) => {
      return [letter, ...currentArray];
    });
  }

  return (
    <div>
      <input value={value} onChange={(e) => setValue(e.target.value)} />
      <br />
      <button onClick={() => addLetterToStart(value)}>
        Add Value To Array
      </button>
      <br />
      {array.join(", ")}
    </div>
  );
}

export default App;
```

### Add Letter at any index

- Pass function to `onClick`. Ex: `onClick={addLetterAtIndex}`
- takes letter and index as arguments
- Using `slice()` because doesn't change `currentArray`

```jsx
function addLetterAtIndex(letter, index) {
  setArray((currentArray) => {
    return [
      ...currentArray.slice(0, index),
      letter,
      ...currentArray.slice(index),
    ];
  });
}
```

</br>

### Update 1 Array Element by ID

- Loop thru array and call functions by ID
- Very common syntax in React
- In the `input` element in the `Item` component below, the `onChange` calls to the `toggleTodo` function, using `id` as an argument. This `id` argument is followed by any other arguments to pass (`completed` in this example).
- Here we also use a technique is **Object Spread syntax** and it is a convenient way to create a new object with some of the properties updated without mutating the original object.

```jsx
function toggleTodo(id, completed) {
  setItems((currentItems) => {
    return currentItems.map((item) => {
      if (item.id === id) {
        return { ...item, completed };
      } else {
        return item;
      }
    });
  });
}
```

```jsx
export default function Item({ id, completed, toggleTodo }) {
  return (
    <>
      <input
        type="checkbox"
        checked={completed}
        onChange={(e) => toggleTodo(id, e.target.checked)}
      />
    </>
  );
}
```

# CSS Tricks

1. Create List that does the following

- Turn mouse arrow into a finger pointer when a label element (with className `.list-item-label`) or element (with attribute `[data-list-item-checkbox]`) is hovered over
- Change color to dark gray & put line through text when a label element (className `.list-item-label`) with a child element (with attribute `[data-list-item-checkbox]`) is hovered over. Child indicated by `>`.
- Change color to light grey and put line through text of sibling element (with attribute `[data-list-item-checkbox]`) when element (with attribute `[data-list-item-checkbox]`) is checked. Sibling indicated by `~`.

```css
.list-item-label:hover,
[data-list-item-checkbox]:hover {
  cursor: pointer;
}

.list-item-label:hover > [data-list-item-text] {
  color: #333;
  text-decoration: line-through;
}

[data-list-item-checkbox]:checked ~ [data-list-item-text] {
  text-decoration: line-through;
  color: #aaa;
}
```

</br>

```jsx
export default function Item({ entry }) {
  return (
    <>
      <li className="list-item">
        <label className="list-item-label">
          <input type="checkbox" data-list-item-checkbox />
          <span data-list-item-text>{entry}</span>
        </label>
        <button data-button-delete>Delete</button>
      </li>
    </>
  );
}
```

2.

# React Hooks

### Rules for Hooks

1. Hooks can never be called **conditionally** or in **any other block than the function**.

- Why? Because React remembers hooks **by the order they were first called**
- Example...

```jsx
function App() {
  const [value, setValue] = useState("");

  useEffect(() => {
    document.title = value;
  }, [value]);

  return <>{Value}</>;
}
```

- In example, React remembers 1st hook is `useState` `value`, 2nd hook is `useEffect`.
- So you can't call in other blocks, `for` loops, `if` satements, etc. These could end up changing the number hooks rendered in the component.
- This means you ALWAYS HAVE THE SAME NUMBER OF HOOKS RENDERED EVERYTIME YOU RENDER COMPONENT.
- Hooks need to go at top of file in your function.
  - If you had an `if` statement with a return above `useEffect` the `useEffect` might not get called each time. This would change the number of hooks rendered on component render
- _Instead_ you can put `if` statements of `for` loops INSIDE of `useEffect`

2. Hooks can only be used inside of a) function components
   b) custom hooks

</br>

## useRef Hook

1. `useRef` allows saving data between renders. Unlike `state` and `prop`, saving data in `useRef` does not call rerender

- Assign a value, as in `useRef("Lance")`

  - It takes a value or a function
  - value gets saved in an object

```jsx
const nameRef = useRef("Lance");
```

- `useRef` is taking the value `"Lance"` and wrapping it in an object (`nameRef`)
- the object (`nameRef`) has 1 property called `.current`

- To save a new value, `nameRef.object = "Sally"`
- Editing value **does not** cause re-render. `useRef` is tied to the component (but not the re-rendering).

```jsx
const nameRef = useRef("Lance");
```

- Can have the variable now of `nameRef` in different components, and each component can change `nameRef` to a different value than other components have
  - We use `useRef` instead of a variable outside of function because that would be global to whole React app

2. THERE IS A 2ND USE CASE OF `useRef`: can also be used to get a reference to an element in HTML

```jsx
const inputRef = useRef();
```

- Every element in HTML has a `ref` attribute
- We can set, for example...

```jsx
<input ref={inputRef} value={name} onChange={(e) => setName(e.target.value)} />
```

- Now if we `console.log(inputRef.current)`  
  we get a REFERENCE TO THE VALUE OF `input` ELEMENT
- `inputRef` **is not available** until AFTER component renders
  - This allows things like **putting focus** on an input value with cursor. Done in a `useEffect` because `useEffect` is run AFTER the initial rendering. That way we don't get an error (because the `inputRef` is not available until AFTER component renders)
  - Example...

```jsx
useEffect(() => {
  inputRef.current.focus();
}, []);
```

- This will put cursor focus in `input` element afterit is first rendered

- Full example...

```jsx
import { useState, useRef, useEffect } from "react";

function App() {
  const [name, setName] = useState("");
  const inputRef = useRef();

  useEffect(() => {
    inputRef.current.focus();
  }, []);

  return (
    <>
      <label>
        Name:
        <input
          ref={inputRef}
          value={name}
          onChange={(e) => setName({ name: e.target.value })}
        />
      </label>
    </>
  );
}

export default App;
```

</br>

### Refs in Class Components

1. Use refs in class components to reference an element in HTML. Setup in constructor with `React.createRef()`. Example...

```jsx
this.inputRef = React.createRef();
```

- can then use `inputRef` in your element. Ex..

```jsx
<input
  ref={this.inputRef}
  value={name}
  onChange={(e) => setName(e.target.value)}
/>
```

- Finally, you can use `componentDidMount()` to give cursor focus to input

```jsx
componentDidMount() {
  this.inputRef.current.focus()
}
```

- Full example...

```jsx
import React from "react";

class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      name: "",
    };
    this.inputRef = React.createRef();
  }

  componentDidMount() {
    this.inputRef.current.focus();
  }

  render() {
    return (
      <>
        <label>
          Name:
          <input
            ref={this.inputRef}
            value={this.state.name}
            onChange={(e) => this.setState({ name: e.target.value })}
          />
        </label>
      </>
    );
  }
}
```

2. Don't even need Refs to save value that doesn't cause re-render in class components

- just put `this.value =` in the constructor
- can then change this outside of constructor to save a different value to the `this` property

</br>

## `useMemo`

1. Memorization for performance

- Use when you have a piece of code that is slow to run, but you don't need it to run on every render
- `useMemo` accepts as function, and a dependancy array (like `useEffect`)
- `useMemo` works by checking to see if the dependancy has changed, and if so, it runs the function
- If the dependency has not changed, it does not re-run the function
- `useMemo` does NOT cache values like other memorization in coding.
  - It ONLY stores the last value
- Example...

```jsx
const filteredList = useMemo(() => {
  return LIST.filter((n) => n.toString().includes(query));
}, [query]);
```

- In example, `console.log(filteredList.length)` makes the call to `filteredList`. This equation takes a relatively long time
- `useMemo` makes it so that the function only run when there is a change from the last `query`.
- Full example...

```jsx
import { useState, useMemo } from "react";

const LIST = Array(1_000_000)
  .fill()
  .map((_, i) => i + 1);

function App() {
  const [query, setQuery] = useState("");
  const [isDarkMode, setIsDarkMode] = useState(false);

  const filteredList = useMemo(() => {
    return LIST.filter((n) => n.toString().includes(query));
  }, [query]);

  console.log(filteredList.length);

  return (
    <div
      style={{
        backgroundColor: isDarkMode ? "#333" : "white",
        color: isDarkMode ? "white" : "#333",
      }}
    >
      <label>
        Query:
        <input value={query} onChange={(e) => setQuery(e.target.value)} />
      </label>
      <br />
      <label>
        <input
          type="checkbox"
          onChange={(e) => setIsDarkMode(e.target.checked)}
          checked={isDarkMode}
        />
        Dark Mode
      </label>
    </div>
  );
}

export default App;
```

## `useCallback`

1. `useCallback` works like `useMemo` but for memorization of **FUNCTIONS**
2. `useCallback` memorizes the function

- `useMemo` memorizes the RETURN of `useMemo`

3. Mostly used for if a **function is a dependency** for a `useEffect`, `useMemo`, or other hooks.

- Because if a `useEffect` (or other hook) has a **function** as a dependency, it will always be run on re-render because functions are always re-created if put inside component
- So if the function is always re-created in component (ie, reference to function), then the hook that has the function as a dependency will always re-run
- Big difference between `useMemo` and `useCallback` is that...
  - `useMemo` is for _performance_
  - `useCallback` is for wrapping functions so they can be used in a dependancy array

4. Note: Best to use `useCallback` to wrap function so function can be in `useEffect` INSTEAD OF referencing the values in the function in `useEffect` as an alternative to putting function in dependency array. This is because your functions can be stored somewhere else, or you may want to change function
5. Example... we DON'T want to do this below because `printName` is recreated each time component is re-run, which means the `useEffect` will always be re-run

```jsx
function printName() {
  console.log(`Name: $(name)`);
}

useEffect(() => {
  console.log("in effect");
  printName();
}, [printName]);
```

- Instead we use `useCallback` as below...

```jsx
const printName = useCallback(() => {
  console.log(`Name: $(name)`);
}, [name]);

useEffect(() => {
  console.log("in effect");
  printName();
}, [printName]);
```

5. Full example...

```jsx
import { useState, useEffect, useCallback } from "react";

function App() {
  const [name, setName] = useState("");
  const [age, setAge] = useState(0);

  const printName = useCallback(() => {
    console.log(`Name: $(name)`);
  }, [name]);

  useEffect(() => {
    console.log("in effect");
    printName();
  }, [printName]);

  return (
    <>
      <label>
        Name:
        <input value={name} onChange={(e) => setName(e.target.value)} />
      </label>
      <br />
      <label>
        Age:
        <input
          type="number"
          value={age}
          onChange={(e) => setAge(e.target.valueAsNumber)}
        />
      </label>
    </>
  );
}

export default App;
```

## Custom Hooks

1. Function that contains other hooks inside of it to do your logic
2. Use when you see logic you are reusing
3. React knows it's a custom hook because it starts with `use`
4. Include one or more hooks in custom hook
5. Can return as an _array_ or an _object_
6. Generally we include it in a NEW file as `.js` (because doesn't have jsx). Then we import like other hooks... `import { useToggle } from "./useToggle"`
7. Anytime we put a `function` inside of a custom hook, it's a good idea to wrap function in a `useCallback`

- That way if function is used in a `useEffect` ets, the user doesn't have to worry about doing that themselves

8. Example... We use this toggle logic a lot...

```jsx
function App() {
  const [name, setName] = useState("");
  const [isDarkMode, setIsDarkMode] = useState(false);

  // logic....

  return (
    <div>
      {/* // jsx.... */}
      <button onClick={() => setIsDarkMode((d) => !d)}>Toggle Dark Mode</button>
      {/* // jsx.... */}
    </div>
  );
}
```

- We can make custom hook as below

```jsx
function useToggle(initialValue) {
  const [value, setValue] = useState(initialValue);

  function toggle() {
    setValue((currentValue) => !currentValue);
  }

  return [value, toggle];
}
```

- we then set our `useToggle` as `const [isDarkMode, toggleDarkMode] = useToggle(false)`
- In jsx, we just call `onClick={toggleDarkMode}` instead of other code

9. Another example... we use this setName logic a lot

```jsx
function App() {
  const [name, setName] = useState("");

  // logic....

  return (
    <div>
      {/* // jsx.... */}
      <label>
        Name:
        <input name={value} onChange={(e) => setName(e.target.value)} />
      </label>
      {/* // jsx.... */}
    </div>
  );
}
```

- We can make custom hook as below

```jsx
function useInputValue(initialValue) {
  const [value, setValue] = useState(initialValue);

  return {
    value,
    onChange: (e) => setValue(e.target.value),
  };
}
```

- Note that this returns an object. So now we can just spread in out `<input>` as <input {...nameInput} />

9. Full example...

```jsx
import { useState } from "react";

function App() {
  const nameInput = useInputValue("");
  // const [name, setName] = useState("");
  const [isDarkMode, toggleDarkMode] = useToggle(false);
  // const [isDarkMode, setIsDarkMode] = useState(false);

  return (
    <div
      style={{
        background: isDarkMode ? "#333" : "white",
        color: isDarkMode ? "white" : "#333",
      }}
    >
      <label>
        Name:
        {/* <input name={value} onChange={e => setName(e.target.value)} />*/}
        <input {...nameInput} />
      </label>
      <br />
      <br />
      {/* <button onClick={() => setIsDarkMode((d) => !d)}>Toggle Dark Mode</button> */}
      <button onClick={toggleDarkMode}>Toggle Dark Mode</button>
    </div>
  );
}

function useInputValue(initialValue) {
  const [value, setValue] = useState(initialValue);

  return {
    value,
    onChange: (e) => setValue(e.target.value),
  };
}

function useToggle(initialValue) {
  const [value, setValue] = useState(initialValue);

  function toggle() {
    setValue((currentValue) => !currentValue);
  }

  return [value, toggle];
}

export default App;
```

</br>

# Local Storage in React

[[freeCodeCamp - "How to Use `localStorage` with React Hooks to Set and Get Items"](https://www.freecodecamp.org/news/how-to-use-localstorage-with-react-hooks-to-set-and-get-items/)]

`localStorage` is a **web storage object** allowing JS sites & apps to keep key-value pairs in a web browser.

- Data survives page refreshes and browser restarts
- Enables developers to store and retrieve data in the browser
- NOT to be used as a database
- It includes 5 methods...

1. `setItem()`: Add a key & value to `localStorage`

- Can store value of any datatype (text, integer, object array...)
- Must FIRST stringify data with `JSON.stringify()`
- Example...

```jsx
const [items, setItems] = useState([]);

useEffect(() => {
  localStorage.setItem("items", JSON.stringify(items));
}, [items]);
```

- This sets state (`items`) to localStorage by...
  - names key 'items'
  - assigns `items` value after stringifying it
  - `[items]` array set as dependency to re-run `useEffect`

2. `getItem()`: Get item from `localStorage` using key

- When we stored data, it was converted to a JSON string. We have to parse it back to JSON (convert JSON string back to JSON object). This is done with `JSON.parse()`

```jsx
const [items, setItems] = useState([]);

useEffect(() => {
  const items = JSON.parse(localStorage.getItem("items"));
  if (items) {
    setItems(items);
  }
}, []);
```

- This gets JSON string from `localStorage` using key ('items')
- It converts to JSON object with `JSON.parse(...)`
- It runs `setItems` function from `useState` if returned value exists (ie, is in `localStorage`)
- It only runs on first mount because of `[]`, empty dependency array

3. `removeItem()`: Delete item from `localStorage` using key
4. `clear()`: Delete all instances of `localStorage`
5. `key()`: When you supply a number, it aids in retrieval of `localStorage` key

Example from WebDev (create `useLocalStorage` that emulates `useState` and be able to pass value or function)

```jsx
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
