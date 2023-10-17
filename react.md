# JSX Code

1. Can't be run in the browser - gets converted to a different syntax of just plain javascript code. For example...

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

| **Topic**                               | **HTML**            | **React**                                                              |
| --------------------------------------- | ------------------- | ---------------------------------------------------------------------- |
| **Attributes**                          | dash                | camelCase                                                              |
| ..._example_                            | _tab-index_         | _tabIndex_                                                             |
| **Custom Data Attributes ( data-... )** | data-...            | data-...                                                               |
| ..._example_                            | _data-date_         | _data-date_                                                            |
| **ARIA Attributes (aria-...)**          | aria-...            | aria-...                                                               |
| ..._example_                            | _aria-label_        | _aria-label_                                                           |
| **class attribute**                     | class               | className                                                              |
| ..._example_                            | _class="project"_   | _className="project"_                                                  |
| **for attribute**                       | for                 | htmlFor                                                                |
| ..._example_                            | _for="id"_          | _htmlFor="id"_                                                         |
| **style attribute**                     | style = "...:...;"  | style={{ : }}                                                          |
|                                         | note:               | In React, the style is passed as object.                               |
| ..._example_                            | style="color:blue;" | style={{ color: "blue" }}                                              |
|                                         | note:               | Also, anything that would be in " " (that isn't a string), are in { }. |

</br>

# Components

1. Components need to start with a capital letter (this is how React knows it is a component)
2. You can change name of what to call a component by using "as". Ex... `import { ToDoList as List } from "./ToDoList"`
3. Can define multiple components in same file

</br>

# Props

1. Pass props without commas... `<NameFunc name="Lance" age={54} />`
2. The component recieves as `props`. Ex... `export function NameFunc(props){...}`
3. Props is actually an obect, so call it as `props.name` or `props.age`
4. Can recieve props destructured... `export function NameFunc({name, age}){...}`
5. Can set a default in the destructuring... `export function NameFunc({name, age = 54}){...}`
6. If you pass a boolean prop without a value, it defaults to `true`
7. If you pass a child, you access with `props.children`

- `children` is a special term in React for this purpose

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

```jsx
 { myVariable } != 1 && `${myVariable} is not equal to 1`
```

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
| Update       | Every re-render after first - only happens when state of a component changes, or its parent is re-rendered     |
| Unmounting   | When component is removed from the page - note: after unmounting then mounting again, state is back to initial |

</br>

# useEffect Hook

- react functions should be PURE functions (no side effects outside of them)
- `useEffect` allows you to do things that change side effects but keeping pure function

### **`useEffect(fx, [dependency array])`**

- `useEffect` takes a function. 2nd parameter is an array of values.

1. Calls that function everytime a value from the array changes
2. If you only want it to run on MOUNT, use empty array [] as dependency array

- To run everytime there is a change to any state, leave off array

3. Use `useEffect` any time want to change DOM, like document title
4. Use `return ()` for cleanup (ex removing event listener) - it runs cleanup THEN the above part of function
5. When component is un-mounted, all cleanup is run
6. REMEMBER, each time rendered, new object (remember ref vs value).
7. Everytime rerendered, React creates a new object that you have made. May have same info, but will have different reference value
8. `useEffect` compares with ===
9. For API do in `useEffect` `.fetch` `.then` `.then` etc `.catch` `.finally`

</br>

# Class component lifecyle

1. `componentDidMount`
2. `componentWillUnmount`
3. `componentDidUpdate`

- only runs on update (unlike `useEffect` which runs on mount & update)
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

1. Create state for data (ex: `const [users, setUsers] = useState()`)
2. Create state for error (ex: `const [error, setError] = useState()`)
3. Create state for loading (ex: `const [loading, setLoading] = useState(true)`)

**Do in `useEffect`**

4. `setLoading(true)` //Set loading to true each time api called
5. `setError(undefined)` // set error to undefined each time api called
6. `setData(undefined)` // set data to undefined each time API called
7. `const controller = new AbortController()` // create new `AbortController` object, used to cancel `fetch` // used for cleanup // it is saying that anytime `fetch` is called, it cancels previous `fetch` from before re-mounting and starts new `fetch`
8. `fetch(url, { signal: controller.signal} )` // fetch the API url // signal links controller to our `fetch` request
9. `.then(res => {...})` // eval result for success or error.

- `if (res.status === 200) { return res.json() }` // is successful, parse json
- `else { return Promise.reject(res) }` // then will get error below in `catch`

9. `.then(data => setUsers(data))` // set returned json to the chosen state. here it is `users`
10. `.catch(e => {...})` // start catch

- `if (e?.name === “AbortError”) return` // if name of error is `AbortReturn`, do nothing bc we already took care of, manually aborted
- `setError(e)` // sets error state to error (if not manually aborted bc that was ONLY TO CANCEL PREVIOUS FETCH

11. `.finally(() => {setLoading(false)})` // indicated we are done loading (so loading jsx can change)

12. `return () => { controller.abort() }` // REMEMBER THAT IN `useEffect` THE RETURN AT THE END IS CLEANUP... this is cancelling previous fetch before calling fetch
13. `,[]` // use empty dependency array for useEffect so only calls on mount

**THEN FOR UI**

1. `let jsx` // define variable

- `if (loading) { jsx = <h2>Loading…</h2> }` // show “Loading…” on screen
- `elseif {error != null) {jsx = <h2>Error</h2>}` // show error if error
- `else <h3>{ jsx = JSON.stringify(data) }</h3>` // if done loading, set variable to stringified JSON

2. `return ( { jsx } )` // render variable to screen

**Note**

- Can go to network in chrome dev to slow down speed to see loading
- “The signal” is a javascript built in that is used to not call `.then` twice (bc `fetch` isnt being cancelled if called then recalled) – USED FOR CLEANUP

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

A way to return multiple elements without having to wrap in a `<div>`

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

</br>

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
- `useMemo` accepts a function, and a dependancy array (like `useEffect`)
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
- `useMemo` makes it so that the function only runs when there is a change from the last `query`.
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

- That way if function is used in a `useEffect` etc, the user doesn't have to worry about doing that themselves

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

From [[freeCodeCamp - "How to Use `localStorage` with React Hooks to Set and Get Items"](https://www.freecodecamp.org/news/how-to-use-localstorage-with-react-hooks-to-set-and-get-items/)]

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

</br>

# Forms

## Forms - Form Basics

Creating forms inside of React

- usually you WANT to use a form for accessibility

Topics:

1.  If your `<input>` is not in a form, then hitting `ENTER` will NOT submit form
2.  Forms have really good accessibility features for screen readers
3.  Default behavior for form is to REFRESH PAGE on enter or submit (this is because it expects it is sending to a server url)

- We want to PREVENT default behavior

  - REMOVE `onClick` from `<button>`
  - ADD `onSubmit={...}` inside of form element
  - In the function passed, ADD `e.preventDefault()`
  - Example

  ```jsx
  function addNewTodo(e) {
    e.preventDefault()
    if (newTodoName) .......
  }

  <form onSubmit={addNewTodo} id="new-todo-form">
  ....
  ```

4.  If you specify a `value={...}` and an `onChange={...}` then it is a **controlled input**

- For **uncontrolled** can specify nothing, or `defaultValue={...}`

5.  If working with checkboxes, use `checked={...}` and `onChange={...}`

- For **uncontrolled** can specify nothing, or `defaultChecked`

Inputs you can have inside of `<form>...</form>`

1.  `<textarea>`

- Work the same as an `<input>`. Can have
  - `value="...."`
  - `onChange={...}`
  - `defaultValue="..."`

2.  `<select>`

- Inside of `<select>` you specify options (`<option value="1">1</option>`, etc)
- Then in `<select>` you can specify `value="1"`
- If you want to make it **uncontrolled**, can use `defaultValue`
- Example...

```jsx
<select value="1" onChange={...}>
  <option value="1">1</option>
  <option value="2">2</option>
  <option value="3">3</option>
</select>
```

</br>

## Forms - One Way Data Flow

Data in Forms (& all of React) flows one way

- Parent to child.......
  - If you want to change state in a component, need to still do it with the setter
  - To do this pass the function in props that can do this
- True in all of React, but comes up a lot in forms b/c forms have so many inputs, etc, together
- So ALL DATA FLOWS IN ONE DIRECTION - if you want to go the other direction, you need to trigger an event to let you interact with the state

</br>

## Forms - useState vs. useRef

1.  If we are using `useState` and a **controlled input**, it will re-render everytime the input changes before hittin submit

- Instead, can use `useRef` and **uncontrolled input**
  - Leave `value` and `onChange` off of input
  - ADD `ref={...}`
- Again, THIS IS COMMON TO DO IN FORMS because we only need access to the value when something happens
- Improves performance a little to use `useRef`
- Example - `useRef` instead of `useState` (which is commented out)

```jsx
import { useRef, useState } from "react";

function App() {
  const nameRef = useRef();
  // const [name, setName] = useState("");

  function handleSubmit(e) {
    e.preventDefault();
    const name = nameRef.current.value;

    if (name === "") return;

    alert(`Name: ${name}`);
  }

  return (
    <form onSubmit={handleSubmit}>
      <label htmlFor="name">Name</label>
      <br />
      <input id="name" ref={nameRef} />
      // <input
      //  type="text"
      //  id="name"
      //  value={name}
      //  onChange={(e) => setName(e.target.value)}
      // />
      <br />
      <br />
      <button>Alert Name</button>
    </form>
  );
}

export default App;
```

2.  Choose whether to use `useState` oor `useRef`

- If you need to use input AS CHANGES ARE MADE, `useState` is probably best to use
- If you only need to use input after submit, `useRef` is probably best to use
  </br>

## Forms - Form Libraries

Creating React Forms can be complicated. Most people use some sort of form library. There are some popular ones

1.  React Hook Form

- Most popular and updated a lot

2.  Formik

- Somewhat popular but hasn't be updated

3.  React Final Form

- Least popular and outdated

### React Hook Form

- He is using v7.43.2
- Makes managing form easier
- Installed with `npm install react-hook-form`

1.  Library has react hook `useForm` that is used for 90% of cases

    - returns an object with lots of information

```jsx
const {
  register,
  handleSubmit,
  watch,
  formState: { errors },
} = useForm();
```

2.  `register` helps you set up all of the event listeners you need

```jsx
<input defaultValue="test" {...register("example")} />
```

- This gives us all of the events that we will need

3.  `handleSubmit` is a function you call, and pass it another function, like `onSubmit`

```jsx
    <form onSubmit={handleSubmit(onSubmit)}>
```

- It takes the function (ex: `onSubmit`) and passes along all of the data from your form

4.  Lets you do data validation from HTML validation. Ex...

```jsx
<input {...register("firstName", { required: true, maxLength: 20 })} />
```

5. Can integrate with external UI component libraries. Use `Controller` for this

```jsx
import { useForm, Controller } from "react-hook-form";
```

- The can wrap in `Controller` what you want to integrate
- Example, can integrate controlled inputs

```jsx
return (
  <form onSubmit={handleSubmit(onSubmit)}>
    <Controller
      name="checkbox"
      control={control}
      rules={{ required: true }}
      render={({ field }) => <Checkbox {...field} />}
    />
  </form>
);
```

6.  Has built in error handling

```jsx
    <input
      {...register("mail", { required: "Email Address is required })}
      aria-invalid={errors.mail ? "true" : "false" }
    />
```

7.  Can do schema-based form validation

- Using `Zod` is best
  </br>

## `react-hook-form`

```jsx
npm install react-hook-form
```

1.  Behind the scenes, `react-hook-form` uses a Ref to not re-render page more than it needs to. This is by default. So by default it is **uncontrolled** input
2.  `<ReactSecect>` is used to show how you can use an external UI library. It gives the nice drop-down select ability
3.  `FormGroup.jsx` component encapsulates some of the error logic

- We pass along `errorMessage` as prop to `FormGroup` component

4. we spread `{...register()}` method in input to be able to use the properties of `register`

- pass to `register` the name of input field. Ex... `{...register("email")}`
- this name is how we now use this instance of `register`

5.  In `<form>` element we use `handleSubmit` feature of `react-hook-form`

- `onSubmit={handleSubmit(onSubmit)}`
- Here we are wrapping `onSubmit` (or any function in our code that we want) in `handleSubmit()` (feature of `react-hook-form`). This is the called by `<form>`'s `onSubmit=` function

6.  If we get to our `function onSubmit(data)` function, that means we have already passed all error tests (when using `react-hook-form`)

7.  The name you use in `register()` becomes the unique key of the `data` property

- Same with `errors` etc from `react-hook-library` - unique key is the name used in `register()`

8.  `register()` has many properties you can use like `required`, `validate`, etc

9.  To refer to `errors` (which we loaded from `useForm`), we are looking at the unique key (like "email") to get the `message`

- We are passing this to or `FormGroup` component with `errorMessage` prop

10. With `useForm` we can set default values.

- Ex... `useForm({defaultValues: { email: "Hi"}})`
- Can also change validation mode, criteria mode, and reValidation mode

11. When doing many validations after {...register} you pass as key :value

- Ex...

```jsx
validate: {
              hasLowerCase: (value) => {
                if (!value.match(/[a-z]/)) {
                  return "Must include at least 1 lowercase letter";
                }
              },
              hasUpperCase: (value) => {
                if (!value.match(/[A-Z]/)) {
                  return "Must include at least 1 uppercase letter";
                }
              }
```

12. To use `<ReactSelect>`, need to change to **controlled** input. Need to add `control` from `useForm`.

- a. Use `useController` hook to allow **controlled** input
- b. Add `control` to `useForm` returned properties
- c. For `useController`, define `field` (will be what is used in `<input>` instead of `register`)
- d. For `useController`, define `name` (same as `id` from `<input>`)
- e. Also, define rules in `useController`

13. Can use `watch` property returned from `useForm` to get the most up to date live value of something

- Ex...

```jsx
const {
  register,
  handleSubmit,
  formState: { errors },
  watch,
} = useForm();

const liveEmail = watch("email");
```

- not used in example below, but can use if wanted

`App.jsx`

```jsx
import { FormGroup } from "./FormGroup";
import ReactSelect from "react-select";
import "./styles.css";
import { useController, useForm } from "react-hook-form";

const COUNTRY_OPTIONS = [
  { label: "United States", value: "US" },
  { label: "India", value: "IN" },
  { label: "Mexico", value: "MX" },
];

function App() {
  // useForm returning register, handleSubmit, errors
  const {
    register,
    handleSubmit,
    formState: { errors },
    // control is added to be able to use useController for the drop down
    control,
  } = useForm();

  // useController hook allows CONTROLLED input for the <ReactSelect> part. Define returned field key with a value (here "countryField" - this is how you call it)
  // In useController, define name (same as id from <input>), and rules
  const { field: countryField } = useController({
    name: "country",
    control,
    rules: { required: { value: true, message: "Required" } },
  });

  //  If we get to our `function onSubmit(data)` function, that means we have already passed all error tests
  // The name you use in `register()` becomes the unique key of the `data` property
  function onSubmit(data) {
    console.log(data);
    alert("Success");
  }

  return (
    {/*Here we are wrapping `onSubmit` (or any function in our code that we want) in `handleSubmit()` (feature of `react-hook-form`).  This is the called by `<form>`'s `onSubmit=` function*/}
    <form onSubmit={handleSubmit(onSubmit)} className="form">
      {/*Below we we are looking at the unique key (like "email") to get the `message`
      - We are passing this to or `FormGroup` component with `errorMessage` prop - This is saying :if there are errors (part of useForm), then if there is email error, get message and pass as errorMessage prop*/}
      <FormGroup errorMessage={errors?.email?.message}>
        <label className="label" htmlFor="email">
          Email
        </label>
        <input
          className="input"
          type="email"
          id="email"
          {/* we spread the properties of register method to be able to use options of the method*/}
          {/* pass to `register` the name of input field. Ex... `{...register("email")}` - just by passing name, register is doing all the behind the scenes things on its own*/}
          {...register("email", {
            {/* if you want required to have a message, need to pass an object as below*/}
            required: { value: true, message: "Required" },
            {/*validate takes the value of input and performs the logic you want*/}
            validate: (value) => {
              if (!value.endsWith("@webdevsimplified.com")) {
                return "Must end with @webdevsimplified.com";
              }
            },
          })}
        />
      </FormGroup>
      <FormGroup errorMessage={errors?.password?.message}>
        <label className="label" htmlFor="password">
          Password
        </label>
        <input
          className="input"
          type="password"
          id="password"
          {...register("password", {
            required: { value: true, message: "Required" },
            minLength: { value: 10, message: "Must be at least 10 characters" },
            {/*with many validations like below, pass a key: value */}
            validate: {
              hasLowerCase: (value) => {
                if (!value.match(/[a-z]/)) {
                  return "Must include at least 1 lowercase letter";
                }
              },
              hasUpperCase: (value) => {
                if (!value.match(/[A-Z]/)) {
                  return "Must include at least 1 uppercase letter";
                }
              },
              hasNumber: (value) => {
                if (!value.match(/[0-9]/)) {
                  return "Must include at least 1 number";
                }
              },
            },
          })}
        />
      </FormGroup>
      <FormGroup errorMessage={errors?.country?.message}>
        <label className="label" htmlFor="country">
          Country
        </label>
        {/*ReactSelect needs controlled input to work, so we use useCntroller hook above.*/}
        <ReactSelect
          isClearable
          classNamePrefix="react-select"
          id="country"
          options={COUNTRY_OPTIONS}
          {/*for controlled input, instead of using register, we use the "field" value returned by useController*/}
          {...countryField}
        />
      </FormGroup>
      <button className="btn" type="submit">
        Submit
      </button>
    </form>
  );
}

export default App;
```

`FormGroup.jsx`

```jsx
export function FormGroup({ errorMessage = "", children }) {
  return (
    <div className={`form-group ${errorMessage.length > 0 ? "error" : ""}`}>
      {children}
      {errorMessage.length > 0 && <div className="msg">{errorMessage}</div>}
    </div>
  );
}
```

</br>

## Managing state without `useState`

## `useReducer` Hook

1.  Another way to manage state. Use for more complex pieces of state
2.  Use if using `useState` would result in different `useState`'s to be linked to each other
3.  Use `useReducer` when there are distinct actions to use on `useState`, or where results are distinct states. (For example, in basic example below, the distinct actions are FETCH_START FETCH_SUCCESS, FETCH_ERROR)

- It is set up in a way where you can do distinct actions

4. Format is...

```jsx
const [state, dispatch] = useReducer(reducer, initialState, initialStateFxn);
```

5. `useReducer` takes `reducer` function, initial value (can be object as in example below), and initialStateFxn, if there is one. initialStateFxn takes as it's parameter whatever initialState is

- Note... so `useState` takes initial state that can be value or function. `useReducer` takes initialValue as second paramenter, and initialStateFxn as third

6. `useReducer` returns `state` (like `useState` does), and `dispatch` method

7. Usually define `reducer` function outside of component

- `reducer` method takes `state` and `action`. Whatever you send in `dispatch()` when it is called is recieved by `reducer` as `action` (it gets `state` automatically)

8.  When using `useReducer`, it is good to break out actions into non-string based (to avoid typo mistakes). If not usig typescript, best way is with a object (see ACTIONS below)

```jsx
function reducer(state, action) {
  ...
}
```

8.  `reducer` function is often a switch statement

- `case`s are based off of `action` `type`
- `action` is often sent with `key : value` pairs for `type` & `payload`

Example of basic `useReducer` hook

```jsx
import { useReducer } from "react";

const ACTIONS = {
  INCREMENT: "INCREMENT",
  DECREMENT: "DECREMENT",
  CHANGE_COUNT: "CHANGE_COUNT",
  RESET: "RESET",
};

function reducer(count, action) {
  switch (action.type) {
    case ACTIONS.DECREMENT:
      return count - 1;
    case ACTIONS.INCREMENT:
      return count + 1;
    case ACTIONS.CHANGE_COUNT:
      return count + action.payload.value;
    case ACTIONS.RESET:
      return 0;
    default:
      return count;
  }
}

export default function Counter({ initialCount = 0 }) {
  const [count, dispatch] = useReducer(reducer, initialCount);

  return (
    <>
      <button onClick={() => dispatch({ type: ACTIONS.DECREMENT })}>-</button>
      {count}
      <button onClick={() => dispatch({ type: ACTIONS.INCREMENT })}>+</button>
      <button onClick={() => dispatch({ type: ACTIONS.RESET })}>Reset</button>
      <button
        onClick={() =>
          dispatch({ type: ACTIONS.CHANGE_COUNT, payload: { value: 5 } })
        }
      >
        +5
      </button>
    </>
  );
}
```

Example of `useFetch` with `useReducer` (instead of `useState`)

- perfect example of how you'd normally use `useReducer`

`useFetch.jsx`

```jsx
import { useEffect, useReducer, useState } from "react";

const ACTIONS = {
  FETCH_START: "FETCH_START",
  FETCH_SUCCESS: "FETCH_SUCCESS",
  FETCH_ERROR: "FETCH_ERROR",
};

function reducer(state, { type, payload }) {
  switch (type) {
    case ACTIONS.FETCH_START:
      return {
        ...state,
        isError: false,
        isLoading: true,
      };
    case ACTIONS.FETCH_SUCCESS:
      return {
        ...state,
        data: payload.data,
        isLoading: false,
        isError: false,
      };
    case ACTIONS.FETCH_ERROR:
      return {
        ...state,
        isError: true,
        isLoading: false,
      };
    default:
      return state;
  }
}

export function useFetch(myUrl, options = {}) {
  const [state, dispatch] = useReducer(reducer, {
    isError: false,
    isLoading: true,
  });
  //  const [data, setData] = useState();
  //  const [isError, setIsError] = useState(undefined);
  //  const [isLoading, setIsLoading] = useState(undefined);

  useEffect(() => {
    dispatch({ type: ACTIONS.FETCH_START });
    //    setData(undefined);
    //    setIsError(undefined);
    //    setIsLoading(true);

    const controller = new AbortController();

    fetch(myUrl, options, { signal: controller.signal })
      .then((res) => {
        if (res.status === 200 || res.status === 201) {
          return res.json();
        } else {
          console.log("res: ", res);
          return Promise.reject(res);
        }
      })
      .then((data) => {
        dispatch({ type: ACTIONS.FETCH_SUCCESS, payload: { data } });
        // setData(data);
      })
      .catch((e) => {
        if (e?.name === "AbortError") return;
        dispatch({ type: ACTIONS.FETCH_ERROR });
        // setIsError(e);
      });
    //  .finally(() => {
    //  if (controller.signal.aborted) return;
    //  setIsLoading(false);
    //  });

    return () => {
      controller.abort();
    };
  }, [myUrl]);

  return state;
}
```

## `useContext` Hook

1.  Makes sharing state between multiple components much easier
2.  `useContext` allows you to pass props to any components down the line without passing props though each intermediate component
3.  In the following code, `useState` is used for `isDarkMode`, And `toggleTheme` is a function in `App`

- Since the logic for controlling dark mode is in the root of the component tree, you would need to "prop-drill" through components to use this in nested components, etc

```jsx
import { useEffect, useState } from "react";

function App() {
  const [isDarkMode, setIsDarkMode] = useState(false);

  function toggleTheme() {
    setIsDarkMode((d) => !d);
  }

  useEffect(() => {
    document.body.style.background = isDarkMode ? "#333" : "white";
    document.body.style.color = isDarkMode ? "white" : "#333";
  }, [isDarkMode]);

  return (
    <>
      <button
        style={{
          background: isDarkMode ? "white" : "#333",
          color: isDarkMode ? "#333" : "white",
          border: "none",
          padding: ".5em",
          borderRadius: ".25em",
          cursor: "pointer",
        }}
        onClick={toggleTheme}
      >
        Toggle Theme
      </button>
      <p>Lorem ipsum</p>
    </>
  );
}

export default App;
```

- Say the `<button>` is in a `Grandchild.jsx` component, which is called from `Child.jsx` component. We would need to prop-drill `isDarkMode` and `toggleTheme` as props though `Child.jsx` and `Grandchild.jsx`
- If `Child.jsx` does not have a `<button>` for dark mode, the component doesn't even need the props, but you'd have to pass props though anyway

3.  To use `useContext`...

    a. import `createContext`

    ```jsx
    import { createContext } from "react";
    ```

    b. Define `createContext` OUTSIDE of component. `useContext` makes state global so needs to be outside of component

    c. Capitalize variable name for `createContext` instance (because it will be used as a component)

    - We `export` because will be imported into components like importing a component

    ```jsx
    export const ThemeContext = createContext();
    ```

    d. Wrap all of the code (tree of coponents) that will use anything that we are going to put in the `useContext`

    - Wrap it with the defined name of our `createContext` (ex... `ThemeContext`)
    - Add `.Provider` (would be `.Consumer` if being consumed in Class Component)

    e. Add a prop to this of `value=` and pass the variables that will be used. We are passing as object below

    - I removed `<button>` logic which will be in Child or nested for `useContext`
    - I added <Child /> to call. So `<Child>` is wrapped in tree of coponents (so `App` calls `Child`, and `Child`calls `Grandchild`. Now `Grandchild` can use variables by using `useContext` )

    ```jsx
    return (
      <ThemeContext.Provider value={{ isDarkMode, toggleTheme }}>
        <Child />
        <p>Lorem ipsum</p>
      </ThemeContext.Provider>
    );
    ```

    f. Then to use...

    - `import` from where it was defined
    - call `useContext` by name at top of component if calling from a class component, need `.Consumer` instead
    - destructure variables out to use them

    ```jsx
    import { ThemeContext } from "./App";

    export function Grandchild() {
      const [isDarkMode, toggleTheme] = useContext(ThemeContext);
      {
        /* ........ */
      }
    }
    ```

    - Now anywhere we want to use our context we can use it, as long as a child of our context Provider, and parent component is wrapped in context Provider

    - Note... `App` is still using `useState` to manage the variables. We are ALSO adding `useContext` to pass variables somewhere down component tree without prop-drilling

4.  Caveats - Don't use for all of the state. Use if...

    a. You have state that will be global to your entire application

    b. Or state global to a subset of application

    - Example - If you have a section of state just dealing with comments, you might want to wrap that in a context to use anywhere that deals with comments

5.  Full example...

`App.jsx`

```jsx
import { useEffect, useState, createContext } from "react";
import Child from "./Child";

export const ThemeContext = createContext();

function App() {
  const [isDarkMode, setIsDarkMode] = useState(false);

  function toggleTheme() {
    setIsDarkMode((d) => !d);
  }

  useEffect(() => {
    document.body.style.background = isDarkMode ? "#333" : "white";
    document.body.style.color = isDarkMode ? "white" : "#333";
  }, [isDarkMode]);

  return (
    <ThemeContext.Provider value={{ isDarkMode, toggleTheme }}>
      <Child />
      <p>Lorem ipsum</p>
    </ThemeContext.Provider>
  );
}

export default App;
```

`Child.jsx`

```jsx
import Grandchild from "./Grandchild";

export default function Child() {
  return (
    <>
      <Grandchild />
    </>
  );
}
```

`Grandchild.jsx`

```jsx
import { useContext } from "react";
import { ThemeContext } from "./App";

export default function Grandchild() {
  const { isDarkMode, toggleTheme } = useContext(ThemeContext);

  return (
    <>
      <button
        style={{
          background: isDarkMode ? "white" : "#333",
          color: isDarkMode ? "#333" : "white",
          border: "none",
          padding: ".5em",
          borderRadius: ".25em",
          cursor: "pointer",
        }}
        onClick={toggleTheme}
      >
        Toggle Theme
      </button>
    </>
  );
}
```

## Future of Redux

With `useContext` and `useReducer` don't see a need for Redux to manage state

- Redux did two things... gave `reducer` like capabilities, and allowed ability to easily share state with different levels of nesting

## Local is Best

Problem: People share too much state globally, and don't put state locally enough

1.  Put state as locally as possible. Store state in component as close as possible to where it is needed
    - Example, in a `Counter` component, state can be kept in that component. BUT if another component, such as `CounterReset`, needs to access the state, need to put state up a level in component.
    - This way the `Counter` and `CounterReset` components can both access state.
2.  Becomes difficult with larger and larger apps. You may not even realize the state can be more local than it is
3.  Keep asking, "Can I make this more local than it is?"
4.  Applies to making state too global. `useContext` makes state global. `useContext` should be used where it is prohibitive to pass down props, or needs to be used all over app
5.  In examples we've often been putting state in `App`, but irl put it as locally as possible

## Never Store Derived State

1.  Never store something in state that is derived from other things in state
2.  Never store something in state that is taking part of other things in state

Example of WHAT TO NOT DO...

```jsx
// WRONG WAY TO DEAL WITH DERIVED STATE

import { useState } from "react";

export default function App() {
  const [items, setItems] = useState([1, 2, 3, 4, 5]);
  const [filteredItems, setFilteredItems] = useState(items);

  function updateFilteredItems(e) {
    if (e.target.value === "") {
      setFilteredItems(items);
    } else {
      setFilteredItems(items.filter((item) => item < e.target.valueAsNumber));
    }
  }

  return (
    <>
      <label htmlFor="lessThan">Show Less Than</label>
      <input id="lessThan" type="number" onChange={updateFilteredItems} />
      <br />
      <br />
      <div>{filteredItems.join(", ")}</div>
      <br />
      {/*2.5 ADD ISN'T WORKING BC filteredItems USES DERIVED STATE*/}
      <button onClick={() => setItems((i) => [...i, 2.5])}>
        Add 2.5 to list
      </button>
    </>
  );
}
```

- Code doesn't work well because `filteredItems` is a subset of `items` and both are stored in state.
- So the way it is written, if you update `items` you have to also update `filteredItems`. So the adding 2.5 to list isn't working.

INSTEAD, DO NOT USE DERIVED STATES. (Below we don't use `filteredItems` in state)

```jsx
import { useState, useMemo } from "react";

export default function App() {
  const [items, setItems] = useState([1, 2, 3, 4, 5]);
  const [inputValue, setInputValue] = useState("");

  // filteredItems is now just a variable, not saved in state
  // is calculated each time re-rendered(can useMemo if want)
  const filteredItems = useMemo(() => {
    return inputValue === ""
      ? items
      : items.filter((item) => item < inputValue);
  }, [items, inputValue]);

  return (
    <>
      <label htmlFor="lessThan">Show Less Than</label>
      {/* we setInputValue below (instead of calling derived state funtion */}
      <input
        id="lessThan"
        type="number"
        onChange={(e) => setInputValue(e.target.valueAsNumber)}
        value={inputValue}
      />
      <br />
      <br />
      <div>{filteredItems.join(", ")}</div>
      <br />
      <button onClick={() => setItems((i) => [...i, 2.5])}>
        Add 2.5 to list
      </button>
    </>
  );
}
```

- This is changed so we don't use `filteredItems` as a derived state. Instead `items` and `inputValue` are saved as state. `filterdItems` is just a variable calculated when re-renders
- Works now bc when click "Add 2.5", we add 2.5 to end of list (`setItems`), then rerun component and `filteredItems` function is re-run
- We optimized by adding `useMemo`

3.  TEST... If you update one piece of state and it requires you to update another piece of state that is depending on that, then you are storing derived state
4.  Instead of storing derived state, just calculate on each re-render

- If code is running slow bc of this new calculation on every re-render, can optimize with `useMemo`

5.  Another piece of code where this is a problem is if you have a state for `user` and state for `selectedUser`

```jsx
const [users, setUsers] = useState([
  { id: 1, name: "Kyle" },
  { id: 2, name: "John" },
]);

const [selectedUser, setSelectedUser] = useState();
```

- This can give unwanted responses if `user` state changes bc of value referneces
- INSTEAD reference by id

```jsx
const [users, setUsers] = useState([
  { id: 1, name: "Kyle" },
  { id: 2, name: "John" },
]);

const [selectedUserId, setSelectedUserId] = useState();

const selectedUser = users.find((user) => user.id === selectedUserId);
```

- Here we are storing `selectedUserId`, which is then used to derive our value of `selectedUser` (instead of making `selectedUser` a state)

## Environment Variables

How to deal with state that is dependent on which environment you are working in - environment variable

1.  Sometimes you want different environment variables depending on which environment you are in (local, development, production)

- Example, in development you want to call your development API; in production, you want to call your production API.

2.  This works diffent in VITE vs CREATE_REACT_APP

3.  VITE

    a. In VITE, any environment variable you start with `VITE_` will be exposed. Everything else is not exposed - Say you have environment variable in VITE in folder `.env`

    `.env`

    ```jsx
    VITE_URL = www.dev.com;
    PASSWORD = password123;
    ```

    - `PASSWORD` will not be exposed bc doesn't start with `VITE_`
      - You call this in VITE with `{import.meta.env.VITE_URL}`
      - If you called that with PASSWORD, it would NOT expose it

    `App.jsx`

    ```jsx
    export default function App() {
      return <div>{import.meta.env.VITE_URL}</div>;
    }
    ```

    b. Be careful about what gets checked into github repository

    - In `gitignore` we don't check in files ending inl `.local`. So can use `.local` to keep environmental variables from github repo
    - We can have 3 `.env` files, and VITE will use them as specified

    ```jsx
    .env.local
    .env.development.local
    .env.production.local
    ```

    - VITE will now use `.env.development.local` in development environment, use `.env.production.local` in production environment, and `.env.local` any other time
    - Ex... if `npm run build` you are in production environment. Run `npm run prieview` and VITE will be using `.env.production.local`

4.  CREATE_REACT_APP

- This is similar but differences are...

  a. All the naming works the same

  b. `gitignore` still ignores `.local`

  c. To have CREATE*REACT_APP use a variable (exposing it) start with `REACT_APP*`

  `.env.development.local`

  ```jsx
  REACT_APP_URL = www.dev.com;
  PASSWORD = password123;
  ```

  d. Call the variable with {process.env.REACT_APP_URL}

  - Note that we refered to it as `.env.REACT_APP_URL`, and React / CREATE_REACT_APP know to use `.env.development.local` in the development mode

  e. In CREATE_REACT_APP you HAVE TO RESTART APPLICATION when you change environment variables for it to work

## Routing without a Library

Generally use a `switch` statement in `App.jsx` to find URL we are at

1.  Routing allows you to navigate between multiple pages. ( Most famous library is React Router Library )
2.  Say we have the following `pages` folder

`Pages` folder

```jsx
Home.jsx;
Store.jsx;
About.jsx;
```

- We want to render appropriate page based on what URL we are on

  - `/`
  - `/store`
  - `/about`

- Without a library, we want to setect page based on URL
- Get URL with

  - `window.location.href` (for full URL)

  - `window.location.pathname` (for just `/...`)

3.  In React, it doesn't matter the URL, it ALWAYS reders `index.html`, `main.jsx`, and `App.jsx`

4.  Generally use a `switch` statement in `App.jsx` for pathname of URL we are on

`App.jsx`

```jsx
import About from "./pages/About";
import Home from "./pages/Home";
import Store from "./pages/Store";

export default function App() {
  let component;
  switch (window.location.pathname) {
    case "/":
      component = <Home />;
      break;
    case "/about":
      component = <About />;
      break;
    case "/store":
      component = <Store />;
      break;
  }

  return (
    <>
      <nav>
        <ul>
          <li>
            <a href="/">Home</a>
          </li>
          <li>
            <a href="/about">About</a>
          </li>
          <li>
            <a href="/store">Store</a>
          </li>
        </ul>
      </nav>
      {component}
    </>
  );
}
```

`Home.jsx` (`About` & `Store` similar)

```jsx
export default function Home() {
  return <div>Home</div>;
}
```

- Note that Nav Bar always shows up bc `App` is always rendered

5.  Quite SLOW bc it re-runs everything (downloading all of the html, all of the js - really a full page navigation)

- BUT in React all info is run on the client, so don't need a full refresh. `React Router` will help with this bc it won't re-run the whole page

  - `React Router` never re-calls the whole page - it does the state managment and page navigation inside of application

</br>

## React Router

1.  React Router has methods to make routing much easier and more powerful

```jsx
npm import react-router-dom
```

2.  Will work similar to our `switch` statement (in last section) but much more powerful

- Usually create `router` in it's own file `router.jsx`
- Use `createBrowserRouter` (best for 99% of cases)

  - see `createHashRouter` and `createMemoryRouter below`

- `import { createBrowserRouter } from "react-router-dom"`

- create our `router` instance

  ```jsx
  export const router = createBrowserRouter([{}]);
  ```

- This takes an array of routes

- Array has different objects to set. Two main ones are `path` and `element`

  - `path` is the path after url

  - `element` is where to go with path

```jsx
export const router = createBrowserRouter([
  { path: "/", element: <Home /> },
  { path: "/store", element: <Store /> },
  { path: "/about", element: <About /> },
]);
```

- Now we impliment `router`. Usually done in `main.jsx` because we want to wrap ENTIRE APPLICATION in this (could do in `App.jsx`)

- We just create a `RouterProvider` (imported from `react-router-dom`)

- Pass it `router` that we import from `router.jsx`. Basically we are passing the object we made in `router` to the `RouterProvider` in `main.jsx`

`main.jsx`

```jsx
import React from "react";
import ReactDOM from "react-dom/client";
import { RouterProvider } from "react-router-dom";
import { router } from "./router";

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <RouterProvider router={router} />
  </React.StrictMode>
);
```

- This renders our react Component depending on our path

- Now we need to call our `<Navbar>` component

```jsx
export default function Navbar() {
  return (
    <nav>
      <ul>
        <li>
          <a href="/">Home</a>
        </li>
        <li>
          <a href="/about">About</a>
        </li>
        <li>
          <a href="/store">Store</a>
        </li>
      </ul>
    </nav>
  );
}
```

- BUT there are 2 rules about how to use our `<Navbar>`.

  - Instead of anchor tags (`<a></a>`) we use a custom component called `<Link>` (a `react-router` specific component). This will keep page from doing full page refresh. With `<Link>` we use `to` instead of `href`

  ```jsx
  <li>
    <Link to="/">Home</Link>
  </li>
  ```

  - we can't put `<Navbar>` above `<RouterProvider>` because `router` (via `<RouterProvider>` has to wrap everything). If we try it we get error because `<Link>` is part of `react-router`. So anything that calls `<Navbar>` (which includes `<Link>`) needs to be wrapped in `<RouterProvider>`. Therefore we can put `<Navbar>` at top of each component

  ```jsx
  export default function Home() {
    return (
      <>
        <Navbar />
        <h1>Home</h1>
      </>
    );
  }
  ```

  - NOW, we can see by slowing network that it is updating URL, but not refreshing page. It is only swapping out content. So much faster

4.  If you don't want to use routing objects, can do old way by making routing components. To do this, use `createRoutesFromElements`

```jsx
import {
  Route,
  createBrowserRouter,
  createRoutesFromElements,
} from "react-router-dom";
import Home from "./pages/Home";
import Store from "./pages/Store";
import About from "./pages/About";

export const router = createBrowserRouter(
  createRoutesFromElements([
    <>
      <Route path="/" element={<Home />} />
      <Route path="/store" element={<Store />} />
      <Route path="/about" element={<About />} />
    </>,
  ])
);
```

- Here we import `Route` and `createRoutesFromElements`
- Instead of OBJECT of `path` & `element`, we are making `Route` components, and using COMPONENTS with `path` & `element` inside
- BUT using objects is cleaner and more current

5.  The `createBrowserRouter` is the default (& best 99% of the time), but there are other `create...Router` you can use in different situations

a. `createHashRouter`

    - will do `.../#/`, `.../#/store`, etc
    - URL comes after `#` symbol
    - only use if you can't change URL of your site

```jsx
export const router = createHashRouter([
  { path: "/", element: <Home /> },
  { path: "/store", element: <Store /> },
  { path: "/about", element: <About /> },
]);
```

b. `createMemoryRouter`

- Stores everything related to routing in MEMORY, not in url bar
- Only good for testing, like if don't have access to browser
- will NOT see URL bar change bc all in memory

```jsx
export const router = createMemoryRouter([
  { path: "/", element: <Home /> },
  { path: "/store", element: <Store /> },
  { path: "/about", element: <About /> },
]);
```

</br>

## React Router - Nesting Routes

- Expands upon what you can do with React Router.
- Before this, note how we had to put `<Navbar>` inside of each component (`Home`, `Store`, `About`)
- To solve this we can **nest routes**

1.  First we can wrap all of our components in another component. Here, we use `element` to wrap our components in component called `<NavLayout>`

2.  We wrap our objects of `{ path: ..., element: ...}` in an array and declare this as `children` of your new element

- My Note: These objects in `router` can have pairs in the objects like `{ path: ... , element: ...}` or `{ element: ... , children: [...] }`

3.  We create component `<NavLayout>` (here we just do as function below). `<NavLayout>` renders out `<Navbar>` AND the current component

4.  We use `<Outlet />` to render the correct child component. `<Outlet />` is a `React Router` component that allows us to render the child component (`<Home />`, `<Store />`, etc) of our element (`<NavLayout>`)

```jsx
export const router = createBrowserRouter([
  {
    element: <NavLayout />,
    children: [
      { path: "/", element: <Home /> },
      { path: "/store", element: <Store /> },
      { path: "/about", element: <About /> },
    ],
  },
]);

function NavLayout() {
  return (
    <>
      <Navbar />
      <Outlet />
    </>
  );
}
```

- `React Router` always renders from the top down

  - It defaults to `element: <NavLayout />`
  - Starts to render `<NavLayout>` component: So it renders `<Navbar />`
  - Then sees there is an `<Outlet />` so it looks at children of `element: <NavLayout />` to see which one matches (the URL)
  - Ex: if it sees "/store" matches the url, then it renders `<Store />`

5.  We can nest as deep as we want

- Say we add another component `<Team />`
- And want to be able to go to specific team members. We make a component `<TeamMember />`
- We pass `<TeamMember />` a prop of the name based on url (`<TeamMember name="joe" />`)
- Do same with other team members
- Add new components to our `<Navbar />` `<li>` list

`TeamMember.jsx`

```jsx
export default function TeamMember({ name }) {
  return <h1>TeamMember - {name}</h1>;
}
```

`router.jsx`

```jsx
export const router = createBrowserRouter([
  {
    element: <NavLayout />,
    children: [
      { path: "/", element: <Home /> },
      { path: "/store", element: <Store /> },
      { path: "/about", element: <About /> },
      { path: "/team", element: <Team /> },
      { path: "/team/joe", element: <TeamMember name="joe" /> },
      { path: "/team/sally", element: <TeamMember name="sally" /> },
    ],
  },
]);

function NavLayout() {
  return (
    <>
      <Navbar />
      <Outlet />
    </>
  );
}
```

6.  This works well but there is repeated code. We can make make the "/team", "/team/joe", etc childrem of a `{ path: "/team", ... }` by wrapping them in a children array

- And we can remove "/team" from the children beacuse "/team" is parent element (Note: children are "joe", NOT "/joe")

7.  When you want to directly match the path of the parent, use `index: true` (instead of `path: "/team"`)

```jsx
export const router = createBrowserRouter([
  {
    element: <NavLayout />,
    children: [
      { path: "/", element: <Home /> },
      { path: "/store", element: <Store /> },
      { path: "/about", element: <About /> },
      {
        path: "/team",
        children: [
          { index: true, element: <Team /> },
          { path: "joe", element: <TeamMember name="joe" /> },
          { path: "sally", element: <TeamMember name="sally" /> },
        ],
      },
    ],
  },
]);
```

8.  Now say in `<Navbar />` we only want to show `<Team />` (not team members until we are in `<Team />`)

- Make new component `<TeamNav />`
- Break `<Navbar />` up into `<Navbar />` and `<TeamNav />`
- Add `element: <TeamNavLayout />` to object with `path: "/team"`
- Add `<TeamNavLayout />` function. Here it is just below in our `router.jsx`. This directs to `<TeamNav />` when we are in `"/team..."`

`Navbar.jsx`

```jsx
import { Link } from "react-router-dom";

export default function Navbar() {
  return (
    <nav>
      <ul>
        <li>
          <Link to="/">Home</Link>
        </li>
        <li>
          <Link to="/about">About</Link>
        </li>
        <li>
          <Link to="/store">Store</Link>
        </li>
        <li>
          <Link to="/team">Team</Link>
        </li>
      </ul>
    </nav>
  );
}
```

`TeamNav.jsx`

```jsx
import { Link } from "react-router-dom";

export default function TeamNav() {
  return (
    <nav>
      <ul>
        <li>
          <Link to="/team/joe">Team - Joe</Link>
        </li>
        <li>
          <Link to="/team/sally">Team - Sally</Link>
        </li>
      </ul>
    </nav>
  );
}
```

`router.jsx`

```jsx
export const router = createBrowserRouter([
  {
    element: <NavLayout />,
    children: [
      { path: "/", element: <Home /> },
      { path: "/store", element: <Store /> },
      { path: "/about", element: <About /> },
      {
        element: <TeamNavLayout />,
        path: "/team",
        children: [
          { index: true, element: <Team /> },
          { path: "joe", element: <TeamMember name="joe" /> },
          { path: "sally", element: <TeamMember name="sally" /> },
        ],
      },
    ],
  },
]);

function NavLayout() {
  return (
    <>
      <Navbar />
      <Outlet />
    </>
  );
}

function TeamNavLayout() {
  return (
    <>
      <TeamNav />
      <Outlet />
    </>
  );
}
```

- Remember `React Router` goes top down.
- Say we are on "/store" page

  - start at top of router. It calls default element component `<NavLayout/>`. This renders `<Navbar/>` (which renders the list of Home, Store, About, and Team)
  - it goes down looking for a match to "/store", finds it, and renders `<Store/>` component.
  - Therefore, `<TeamNavLayout/>` never gets called, so we don't see Team Members

- Say we are on "/team/joe" page
  - start at top of router. It calls default element component `<NavLayout/>`. This renders `<Navbar/>` (which renders the list of Home, Store, About, and Team)
  - it goes down looking for match first to "/team"
  - it finds it and calls `<TeamNavLayout/>`. This renders `<TeamNav/>` (which renders the list Team - Joe, and Team - Sally)
  - it keeps going down list to find match for "joe". It finds "joe" and renders `<TeamMember name="joe" />`

9.  You can also pass along a **context**. `context` in `React Router` can be very powerful

- In `<Outlet />` you pass a `context`. This is available to all children of the element

```jsx
function TeamNavLayout() {
  return (
    <>
      <TeamNav />
      <Outlet context="Hi from TeamNavLayout Outlet component" />
    </>
  );
}
```

- Now any of the children of `<TeamNavLayout />` can have asscess to this `context`
- Access with the `useOutletContext` hook from `React Router`.

  - Import `{useOutletContext}` into any of the children
  - Assign value to your desired variable and use in in your component

```jsx
import { useOutletContext } from "react-router-dom";

export default function Team() {
  const value = useOutletContext();

  return <h1>Team - {value}</h1>;
}
```

- Here the `<Team />` component can use the context because it is a child of the `<TeamNavLayout />` element in `router.jsx`

```jsx
export const router = createBrowserRouter([
  {
    element: <NavLayout />,
    children: [
      { path: "/", element: <Home /> },
      { path: "/store", element: <Store /> },
      { path: "/about", element: <About /> },
      {
        element: <TeamNavLayout />,
        path: "/team",
        children: [
          { index: true, element: <Team /> },
          { path: "joe", element: <TeamMember name="joe" /> },
          { path: "sally", element: <TeamMember name="sally" /> },
        ],
      },
    ],
  },
]);
```

10. How **relative links** work

- So far we have used **absolute links**, as shown below. They just go right on end of url

```jsx
<li>
  <Link to="/team/joe">Team - Joe</Link>
</li>
```

- We can change this to a **relative link** as below. Now "joe" is REALTIVE to where it is rendered inside of `React Router`

```jsx
<li>
  <Link to="joe">Team - Joe</Link>
</li>
```

- BY DEFAULT when we move around with relative links, all of the links are RELATIVE TO YOUR ROUTES (ie, whatever route element you are inside of). Sometimes that is the same as relative to our url, sometimes not

  - This can be seen by adding a couple of liks to our `<TeamNav />`
  - Look at the `<TeamNav />` component below and the `router.jsx`

```jsx
import { Link } from "react-router-dom";

export default function TeamNav() {
  return (
    <nav>
      <ul>
        <li>
          <Link to="joe">Team - Joe</Link>
        </li>
        <li>
          <Link to="/team/sally">Team - Sally</Link>
        </li>
        <li>
          <Link to="..">.. Relative to actual route</Link>
        </li>
        <li>
          <Link to=".." relative="path">
            .. Relative to path
          </Link>
        </li>
      </ul>
    </nav>
  );
}
```

- Assuming we are in `"/team/sally"`, when we click `<Link to="..">.. Relative to actual route</Link>`, we go up to 1st (default) element in router b/c we are moving relative to route we are in (we are a child of `element: <TeamNavLayout />, path: "/team"`)

  - Therefore we move up to `element: <NavLayout />` (`path: "/"`) route
  - We move from `"/team/sally"` to `"/"`

- BUT if we click `<Link to=".." relative="path">.. Relative to path</Link>` this move us to `path: "/team"`.

  - This is because the `relative="path"` attribute means that it moves relative to the path
  - This means we go from `"/team/sally"` to `"/team"`

- We can see more of how router works by setting `path: "."` . The single dot with take you to the current url relative to route

```jsx
import { Link } from "react-router-dom";

export default function TeamNav() {
  return (
    <nav>
      <ul>
        <li>
          <Link to="joe">Team - Joe</Link>
        </li>
        <li>
          <Link to="/team/sally">Team - Sally</Link>
        </li>
        <li>
          <Link to=".">. Current url relative to route</Link>
        </li>
        <li>
          <Link to="." relative="path">
            . Current url relative to route
          </Link>
        </li>
      </ul>
    </nav>
  );
}
```

- Assuming we are in `"/team/sally"`, when we click `<Link to=".">. Current url relative to route</Link>`, we go `"/team"` . That is the current url relative to our route

- BUT if we click `<Link to="." relative="path">. Current url relative to route</Link>` this keeps us at the same url because it is going to current url relative to path we are on

- SO IN SUMMARRY

  - If you want your routes to be relative to whatever `path` you are in, the set `relative="path`
  - Otherwise it will be relative to where you are rendering inside of router

</br>

11. To have class of active added to your links, use `<NavLink>` instead of `<Link>`

- Now when our `css` has `.active` that will apply to active link

`styles.css`

```jsx
.active {
  color: red;
}
```

`Navbar.jsx`

```jsx
import { NavLink } from "react-router-dom";

export default function Navbar() {
  return (
    <nav>
      <ul>
        <li>
          <NavLink to="/">Home</NavLink>
        </li>
        <li>
          <NavLink to="/about">About</NavLink>
        </li>
        <li>
          <NavLink to="/store">Store</NavLink>
        </li>
        <li>
          <NavLink to="/team">Team</NavLink>
        </li>
      </ul>
    </nav>
  );
}
```

- NOTE: By default, active links apply to the route you are in. In our example above, if we click on `"/team/sally"` our `"/team"` link will be red

  - This is because by default it makes the link active for the route you're in

```jsx
// ...
    {
      element: <TeamNavLayout />,
      path: "/team",
      children: [
        { index: true, element: <Team /> },
        { path: "joe", element: <TeamMember name="joe" /> },
        { path: "sally", element: <TeamMember name="sally" /> },
      ],
// ...
```

- To make it so it DOESN'T MAKE WHOLE ROUTE ACTIVE you add `end` in your `<NavLink>`

```jsx
<li>
  <NavLink to="/team" end>
    Team
  </NavLink>
</li>
```

- `<NavLink>` also changes how we set `className` attribute.

  - `className` can TAKE A FUNCTION in `<NavLink>`
  - can take in different props like `isActive` and `isPending`
  - Therefore `className` can change depending on if `isActive`, etc

```jsx
<li>
  <NavLink className={({ isActive }) => ....} to="/">Home</NavLink>
</li>
```

- Same is true for `style` property
- Can also pass function as CHILD of `<NavLink>` so that you can have different text depending on if `isActive`

```jsx
<li>
  <NavLink to="/">{({ isActive }) => ....}</NavLink>
</li>
```

12. Handling errors in `React Router`

- Can put `errorElement` anywhere in router. It will display error if any of children of route have an error
- You can nest it down and put on particular children so it only applies to them

```jsx
export const router = createBrowserRouter([
  {
    element: <NavLayout />,
    errorElement: <h1>Error</h1>,
    children: [
      { path: "/", element: <Home /> },
      { path: "/store", element: <Store /> },
      { path: "/about", element: <About /> },
      {.........
```

## React Router - Dynamic Routes

1.  Route is dynamically figured out from ID passed in

- like ID from TeamMember.json file

```jsx
[
  { "id": "..-......-....-...-...1", "name": "Joe"},
  { "id": "..-......-....-...-...4", "name": "Sally"},
  {...}
]
```

2.  So in `router` instead of hard-coding routes, you prefix `path` with a colon

- `{path: ": memberId"}`
- the colon is followed by the ID from json
- remove hardcoded routes and the `prop` that was passed to `<TeamMember>`

`router.jsx` (partial)

```jsx
      // ....
      {
        element: <TeamNavLayout />,
        path: "/team",
        children: [
          { index: true, element: <Team /> },
          // CHANGES BELOW
          { path: ":memberId", element: <TeamMember /> }
        ],
        // .....
```

3.  Then in `<TeamNav>` you loop through members with `map` function

- Will create `<li>` for each member in json list.

`TeamNav.jsx`

```jsx
import { Link } from "react-router-dom";
import { teamMembers } from "./teamMembers.json";

export default function TeamNav() {
  return (
    <nav>
      <ul>
        {teamMembers.map((member) => (
          <li key={member.id}>
            <Link to={member.id}>Team - {member.name}</Link>
          </li>
        ))}
      </ul>
    </nav>
  );
}
```

- So far this works to render an `<ul>` of the `<li>` "Team - {member.name}"

4.  Use `useParams()` to pass our member routing ID from `router.jsx`. In the router, whatever is after the colon (ex: in ":memberId", the `memberId` is passed in the `useParams()` return object)

- So we can then use `teamMembers.find()` to find member where id === `memberId`
- We can then reference info from this (ex: `member.name` in the rendering)

`TeamMember.jsx`

```jsx
import { useParams } from "react-router-dom";
import teamMembers from "../teamMembers.json";

export default function TeamMember() {
  const { memberId } = useParams();

  const member = teamMembers.find((m) => m.id === memberId);

  return <h1>TeamMember - {member.name}</h1>;
}
```

5.  What if we want a hardcoded route AND a dynamic route

- How does router know which one to use? It will always use most specific route

- So if we update `router.jsx`, `TeamNav.jsx` and add `NewTeamMember.jsx` as below...

`router.jsx`

```jsx
// ...
{
        element: <TeamNavLayout />,
        path: "/team",
        children: [
          { index: true, element: <Team /> },
          { path: ":memberId", element: <TeamMember /> },
          { path: "new", element: <NewTeamMember /> },
        ],
// ...
```

`TeamNav.jsx`

```jsx
import { Link } from "react-router-dom";
import teamMembers from "./teamMembers.json";

export default function TeamNav() {
  // console.log(teamMembers);

  return (
    <nav>
      <ul>
        {teamMembers.map((member) => (
          <li key={member.id}>
            <Link to={member.id}>Team - {member.name}</Link>
          </li>
        ))}
        <li>
          <Link to="new">New Member</Link>
        </li>
      </ul>
    </nav>
  );
}
```

`NewTeamMember.jsx`

```jsx
export default function NewTeamMember() {
  return <h1>New Team Member</h1>;
}
```

- With this setup, we have "New Member" in our `<li>` list.
- How does `router` know which route to use if we click "New Member"?

  - `router` WILL ALWAYS USE THE MOST SPECIFIC ROUTE (URL). It doesn't matter the order they are in `router.jsx`. Because "new" (hardcoded) is more specific than ":memberId" it will use "new"

  - If we remove "new" route, we get error b/c then it tries to use ":memberId". Get error bc in `TeamMember` the `member.name`fails bc there is not a corresponding member in json. It is trying to access `teamMember` with id of "new" (which doesn't exist)

```jsx
import { useParams } from "react-router-dom";
import teamMembers from "../teamMembers.json";

export default function TeamMember() {
  const { memberId } = useParams();

  const member = teamMembers.find((m) => m.id === memberId);

  return <h1>TeamMember - {member.name}</h1>;
}
```

    - (If we change to `member?.name` it doesn't fail bc of our "if member")

6.  Wild Card Routing - We use "\*" to signify a wildcard route.

- Ex: in `router` below, we added route for "/test/\*"

```jsx
export const router = createBrowserRouter([
  {
    element: <NavLayout />,
    children: [
      { path: "/", element: <Home /> },
      // WILDCARD
      { path: "/test/*", element: <h1>Test</h1> },
      { path: "/store", element: <Store /> },
      { path: "/about", element: <About /> },
      {
        element: <TeamNavLayout />,
        path: "/team",
        children: [
          { index: true, element: <Team /> },
          { path: ":memberId", element: <TeamMember /> },
          { path: "new", element: <NewTeamMember /> },
        ],
      },
    ],
  },
]);
```

- Now if url is "/text/ANYTHING", we go to this route

- I think order is

  - hardcoded (most specific)
  - dynamic
  - wildcard (least specific)

7.  Can use wildcard to set an error page. We just make a `path` of "\*", and render to `element` (like `<h1> 404 </h1>`)

```jsx
export const router = createBrowserRouter([
  {
    element: <NavLayout />,
    children: [
      // wildcard error path
      { path: "*", element: <h1>404</h1> },
      { path: "/", element: <Home /> },
      { path: "/store", element: <Store /> },
      { path: "/about", element: <About /> },
      {
        element: <TeamNavLayout />,
        path: "/team",
        children: [
          { index: true, element: <Team /> },
          { path: ":memberId", element: <TeamMember /> },
          { path: "new", element: <NewTeamMember /> },
        ],
      },
    ],
  },
]);
```

- Now `router` will look at all paths and if doesn't match any, it uses LEAST SPECIFIC path of "\*"

8.  Can use wildcard to go back to home page. If any url doesn't exist, will just go to home page

- use `<Navigate to="/" />`
- This is a router component that just navigates to a specific location

```jsx
export const router = createBrowserRouter([
  {
    element: <NavLayout />,
    children: [
      // wildcard error path TO HOME PAGE
      { path: "*", element: <Navigate to="/" /> },
      { path: "/", element: <Home /> },
      { path: "/store", element: <Store /> },
      { path: "/about", element: <About /> },
      {
        element: <TeamNavLayout />,
        path: "/team",
        children: [
          { index: true, element: <Team /> },
          { path: ":memberId", element: <TeamMember /> },
          { path: "new", element: <NewTeamMember /> },
        ],
      },
    ],
  },
]);
```

9.  `useNavigate` hook is available that works similar to router's `<Navigate to="..." />` component

- say we wanted the `<About />` page to navigate BACK TO HOME after 1 second of being on about page
- We define variable with `useNavigate` hook.

  - Ex: `const navigate = useNavigate()`

- We define where to go, like home page, with `navigate("/")`
- Can use useEffect to `setTimeout` for going to `navigate("/")` after 1 second of being on `<About />`

```jsx
import React, { useEffect } from "react";
import { useNavigate } from "react-router-dom";

export default function About() {
  const navigate = useNavigate();

  useEffect(() => {
    const timeout = setTimeout(() => {
      navigate("/");
    }, [1000]);

    return () => {
      clearTimeout(timeout);
    };
  }, []);

  return (
    <>
      <h1>About</h1>
    </>
  );
}
```

## React Router - Loaders

1.  Using `loader` to load data for us

- Above example but taking away json file... using API for teamMembers
- Able to load data without `useEffect`
- Cleans up our code

- `loader`

  - takes in a function
  - returns a promise with all data that we need: (`return fetch(url)`)
  - pass ability to abort: (`, {signal}`)
  - `loader` passes along some properties: (`{ request: { signal }}`). Used to cancel request. Now if we navigate to page that gives an error, it will cancel our request
  - DON'T need to convert to json - `loader` does that for us

  `router.jsx` (partial)

```jsx
// ...
{
      element: <TeamNavLayout />,
      loader: ({ request: { signal } }) => {
        return fetch("https://jsonplaceholder.typicode.com/users", {
          signal,
        });
      },
      path: "/team",
// ...
```

- `useLoaderData()` is a `react-router` hook that will use loader data where it is called
- now `teamMembers` comes from `loader` data via `useLoaderData()`

`TeamNav.jsx`

```jsx
import { Link, useLoaderData } from "react-router-dom";
export default function TeamNav() {
  const teamMembers = useLoaderData();

  return (
    <nav>
      <ul>
        {teamMembers.map((member) => (
          <li key={member.id}>
            <Link to={member.id}>Team - {member.name}</Link>
          </li>
        ))}
        <li>
          <Link to="new">New Member</Link>
        </li>
      </ul>
    </nav>
  );
}
```

`TeamMember.jsx`

```jsx

```

2.  Breakdown of sequesnce

- in router we are at "/team" path
- render out `<TeamNavLayout>`
- to render this we need to load data. `loader` is called

  - Everything in a loading state while it waits

- when it loads data, it automatically converts to json
- because loader is associated with element `<TeamNavLayout>` in `router`, `<TeamNavLayout>` and children can access data with `useLoaderData()`
- now we get out `teamMembers` from url, and proceed as before
- then in `<TeamMember>` we can get `member` (from `map` in `<TeamNav>`)from `useLoaderData()` (not from json)

  - not getting `memberId` from `useParams()` anymore. Don't need `teamMembers.find(...)` anymore

- Notice in `router`, the route to ":memberId" that calls `<TeamMember>` needs member info for the component
- WE ADD A LOADER to that child route...

  - same as other loader, but we are adding the user id at the end
    to get it we are able to pass `params` here too as a paramenter. Allows us to add user as `params.memberId`
  - Also add the `signal`

`router.jsx` (Partial)

```jsx
// ...
      {
        element: <TeamNavLayout />,
        loader: ({ request: { signal } }) => {
          return fetch("https://jsonplaceholder.typicode.com/users", {
            signal,
          });
        },
        path: "/team",
        children: [
          { index: true, element: <Team /> },
          {
            path: ":memberId",
            loader: ({ params, request: { signal } }) => {
              return fetch(
                `https://jsonplaceholder.typicode.com/users/${params.memberId}`,
                {
                  signal,
                }
              );
            },
            element: <TeamMember />,
          },
          { path: "new", element: <NewTeamMember /> },
        ],
      },
// ...
```

- ISSUE WITH ID & `react-router`:

  - In our `<Link to={member.id}>Team - {member.name}</Link>` there is an issue. Loader is passing ID from api as a number. `react-router` expects a string.
  - So we need to change to string `<Link to={member.id.toString}>Team - {member.name}</Link>`

`TeamNav.jsx`

```jsx
import { Link, useLoaderData } from "react-router-dom";
export default function TeamNav() {
  const teamMembers = useLoaderData();

  return (
    <nav>
      <ul>
        {teamMembers.map((member) => (
          <li key={member.id}>
            <Link to={member.id.toString()}>Team - {member.name}</Link>
          </li>
        ))}
        <li>
          <Link to="new">New Member</Link>
        </li>
      </ul>
    </nav>
  );
}
```

3.  How to `redirect` to somewhere else from inside `loader`

- `redirect` is a function from `react-router`
- can use for error. Ex: `throw redirect("/team")`
- call from inside loader

  - check to see if `res.status === 200`. If not, `redirect` to desired page

- So if ever in a `loader` and you want to redirect somewhere else, use `redirect()`
- Code looks complicated inside of `router` BUT this cleans it up everywhere else

4.  `useNavigation()` hook to get state of `loader`

- can use to get state for loading. Can put anywhere. We put in `<NavLayout>`
- returns lots of different things, inluding STATE OF LOADERS

- `const { state } = useNavigaion()`
- `state` has different properties, including `"loading"`

- can use conditional render to render `<h1>Loading</h1>` if `state === "loading"`

router.jsx

```jsx
// ...
      {
        element: <TeamNavLayout />,
        loader: ({ request: { signal } }) => {
          return fetch("https://jsonplaceholder.typicode.com/users", {
            signal,
          });
        },
        path: "/team",
        children: [
          { index: true, element: <Team /> },
          {
            path: ":memberId",
            loader: ({ params, request: { signal } }) => {
              return fetch(
                `https://jsonplaceholder.typicode.com/users/${params.memberId}`,
                {signal}
              ).then(res => {
                if (res.status === 200) return res.json;
                throw redirect("/team")
              })
            },
            element: <TeamMember />,
          },
          { path: "new", element: <NewTeamMember /> },
        ],
      },
// ...
```

NavLayout.jsx

```jsx
function NavLayout() {
  const { state } = useNavigation();
  return (
    <>
      <Navbar />
      {state === "loading" ? <h1>Loading</h1> : <Outlet />}
    </>
  );
}
```

## `process.env.NODE_ENV` variable

(from research for Basic Routing Project)

The `process.env.NODE_ENV` variable can be used to see if you are in production or development environment

- Allows you to set different messages for each

Example

```jsx
if (process.env.NODE_ENV === "production" && (isPostsError || isTodosError)) {
  return <h2>Error fetching data...</h2>;
}

if (process.env.NODE_ENV === "development" && (isPostsError || isTodosError)) {
  return (
    <div>
      <h2>Error fetching data...</h2>
      <pre>{`${isPostsError && "Posts Error: " + isPostsError}\n${
        isTodosError && "Todos Error: " + isTodosError
      }`}</pre>
    </div>
  );
}
```

1.  Here we use the `process.env.NODE_ENV` variable to see if we are in development or production

- In "production" we return a generic error message

  - "Error fetching data..."

- In "development" we return a full stack error message with `<pre>` by showing the actual error

  - we use short circuiting so that if there is an `isPostsError` (ie, true) then we render `"Posts Error: " + isPostsError`
  - same for isTodosError

```jsx
<pre>
  {`${isPostsError && "Posts Error: " + isPostsError}\n${
    isTodosError && "Todos Error: " + isTodosError
  }`}
</pre>
```
