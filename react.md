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
                `https://jsonplaceholder.typicode.com/users/${.memberId}`,
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

4.  `()` hook to get state of `loader`

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

## Basic Blog Project Walkthrough

refer to [Basic Blog Project](https://github.com/rootdown001/React-Simplified-Beginner-Projects-main/tree/master/75-76-basic-blog-project/after)

Important steps:

Setup

1.  Create "after" folder
2.  `cd client` (to client folder)
3.  `npm create vite@latest`
4.  `npm i`
5.  `npm run dev`
6.  Open new terminal to start `json-server`

- `cd api`
- `npm i`
- `npm run dev`

Now we have server on port `3000` and our application on port `5173`

7.  Remove all starting vite stuff that we don't need
8.  import css
9.  Install 2 libraries

- `npm i react-router-dom`
- `npm i axios` (makes fetch easier)

main.jsx

```jsx
import React from "react";
import ReactDOM from "react-dom/client";
import "./styles.css";
import { RouterProvider } from "react-router-dom";
import { router } from "./router";

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    {/* 1) get Router Provider (& import) */}
    {/* 2) Pass it `router`` */}
    <RouterProvider router={router} />
  </React.StrictMode>
);
```

router.jsx

```jsx

// 1. createBrowserRouter
export const router = createBrowserRouter([
  {
    // 2. create root path and <RootLayout/> for
    path: "/",
    element: <RootLayout />,
    // 3. Notice used path for root - can do either way
    // 4. create children
    children: [
      {
        children: [
          // 5. create paths with elements
          // NOTE: don't need "/posts" because "/" in root above
          { path: "posts", element: <PostList /> },
          // .........
```

Starts with doing it simple like this with `element: <PostList />`

- But see below for moving the element into `<PostListRoute />` and PASSING to router with spread operator

Create `<PostList />` (simple at first)

PostList.jsx

```jsx
export function PostList() {
  return <h2>PostList</h2>;
}
```

Create `<RootLayout />` in folder `layouts`

- taking place of previous `<Navbar/>` in simpler coding

```jsx
export function RootLayout() {
  return <Outlet />;
}
```

- ( so instead of having a `<NavLayout/>` that returns `<Navbar/>` & `<Outlet/>`, we are making `<RootLayout/>` that has the jsx code that would be in a `<Navbar/>` then `<Outlet/>`)

Add `<Navigate/>` to router (from `react-router-dom`)

- Now, if on index route ("/") want to route to "/posts"

router.jsx

```jsx
import { Navigate } from "react-router-dom"

export const router = createBrowserRouter([
  {
    path: "/",
    element: <RootLayout />,
    children: [
      {
        children: [
          // Add `<Navigate/>` element at path of `index: true`
          { index: true, element: <Navigate to="/posts" /> },
          { path: "posts", element: <PostList /> },
          // .........
```

Now finish `<RootLayout/>` with our jsx (brought in from sample html)

- `<nav>`, `<div>`, `<ul>`, `<li>`
- `<Link to="...>` (instaead of href)
- `<Outlet>` (note it is in `<div className={'container}>`)
  - by putting `<Outlet/>` in "container" this seperates from "navbar" type jsx about
  - also lets us later add `className` logic for using our "blur" and "spinner"
- `<ScrollRestoration/>`
  - `react-router-dom` component that puts focus back to top when switching between pages

```jsx
import { Link, Outlet, ScrollRestoration } from "react-router-dom";

export function RootLayout() {
  return (
    <>
      <nav className="top-nav">
        <div className="nav-text-large">My App</div>
        <ul className="nav-list">
          <li>
            <Link to="/posts">Posts</Link>
          </li>
          <li>
            <Link to="/users">Users</Link>
          </li>
          <li>
            <Link to="/todos">Todos</Link>
          </li>
        </ul>
      </nav>
      <ScrollRestoration />
      <div className={"container"}>
        <Outlet />
      </div>
    </>
  );
}
```

If we don't use `<ScrollRestoration/>` then when we switch between pages in our nav, the scroller can leave you at random places in the page

Now, back to `router.jsx`

- we know that we will want to be able to navigate to a posts page AND to individual posts (same with users and individual user)
- we will set up `children:` for "posts" route and "users" route

```jsx
export const router = createBrowserRouter([
  {
    path: "/",
    element: <RootLayout />,
    children: [
      {
          children: [
          { index: true, element: <Navigate to="/posts" /> },
          {
            path: "posts",
            children: [
              // go to `<PostList>` if "/posts"
              { index: true, element: <PostList/>},
              // go to another component if "/posts/:postId"
              { path: ":postId", element: <h2>Individual post page</h2> },
            ],
          },
          {
            path: "users",
            children: [
              { index: true, element: <UserList/> },
              { path: ":userId", element: <h2>Individual user page</h2> },
            ],
          },
          // ...
```

Again, we are pre-setting up because we know we need one `element:` for `index` and one for `"postId:`

We will use `axios` to help with out `fetch`. Allows us to do it more easily.

```jsx
import axios from "axios";
```

Now need `loader`

- Could add inline like we've learned, but the `router.jsx` can get very bloated

```jsx
export const router = createBrowserRouter([
  {
    path: "/",
    element: <RootLayout />,
    children: [
      {
          children: [
          { index: true, element: <Navigate to="/posts" /> },
          {
            path: "posts",
            children: [
              { index: true,
                element: <PostList/>,

                // COULD PUT LOADER HERE
                loader: ({ request: { signal } }) => {
                  return axios
                    .get("http://localhost:3000/posts", {signal})
                    .then(res => res.data)
              },
        // ...
```

INSTEAD, we will put `loader` code INSIDE component that is using it - so we will put inside of `<PostList/>`

1.  We add a loader function to `<PostList/>` that we can import to our router

2.  To make even more streamlined, we will MAKE AN EXPORT OBJECT that will fill in `element` AND `loader` items to our route

3.  Call it `postListRoute` and `export` it

4.  object has `loader` function and `element: <PostList/>`

5.  Now we don't need to `export` our components, so remove the `export` from `<PostList/>`

`PostList.jsx`

```jsx
import axios from "axios";

// remove `export`
function PostList() {
  return <h1>PostList</h1>;
}

// create `loader` function just as would be in `router.jsx`
function loader({ request: { signal } }) {
  return axios
    .get("http://localhost:3000/posts", { signal })
    .then((res) => res.data);
}

// export object with `loader` function, and `element`
export const postListRoute = {
  loader,
  element: <PostList />,
};
```

Now we can spread the object (`...postListRoute`) in `router.jsx`

`router.jsx`

```jsx
children: [
          { index: true, element: <Navigate to="/posts" /> },
          {
            path: "posts",
            children: [
              {
                index: true,
                // spread the object - this gives us `loader` function and `element: <PostList/>`
                ...postListRoute,
              },
```

Now we can...

1.  bring in loader data (`useLoaderData`)
2.  set up jsx to map through posts to show each post in a card (bringing in pre-set html and changing `class` to `className`)
3.  Set up button `<Link to={`/posts/${post.id}`}>`

`PostList.jsx`

```jsx
import axios from "axios";
import { useLoaderData, Link } from "react-router-dom";

function PostList() {
  const posts = useLoaderData();

  return (
    // set up jsx
    <>
      <h1 className="page-title">Posts</h1>
      <div className="card-grid">
        {/*map through posts*/}
        {posts.map((post) => {
          return (
            <div key={post.id} className="card">
              <div className="card-header">{post.title}</div>
              <div className="card-body">
                <div className="card-preview-text">{post.body}</div>
              </div>
              <div className="card-footer">
                {/*set up `<Link>`*/}
                <Link to={`/posts/${post.id}`} className="btn">
                  View
                </Link>
              </div>
            </div>
          );
        })}
      </div>
    </>
  );
}

function loader({ request: { signal } }) {
  return axios
    .get("http://localhost:3000/posts", { signal })
    .then((res) => res.data);
}

export const postListRoute = {
  loader,
  element: <PostList />,
};
```

Do same thing with `<UserList>`

`UserList.jsx`

```jsx
import axios from "axios";
import { useLoaderData, Link } from "react-router-dom";

function UserList() {
  const posts = useLoaderData();

  return (
    // set up jsx

  );
}

function loader({ request: { signal } }) {
  return axios
    .get("http://localhost:3000/users", { signal })
    .then((res) => res.data);
}

export const userListRoute = {
  loader,
  element: <UserList />,
};
```

Now correct `router.jsx` to use `userListRoute`

`router.jsx`

```jsx
//.....
import {userListRoute} from "./pages/UserList"
//....

// ...
children: [
          { index: true, element: <Navigate to="/posts" /> },
          {
            path: "posts",
            children: [
              {
                index: true,
                // spread the object - this gives us `loader` function and `element: <PostList/>`
                ...postListRoute,
              },
            ],
          },
          // // spread the object - this gives us `loader` function and `element: <UserList/>`
          { path: "users", ...userListRoute },
          { path: "todos", element: <ToDoList />},
          ],
// ...
```

NOW, notice `loader` in each (`<UserList/>` & `<PostList/>`) look the same except for API address

- Let's make this reusable code
- Make `api` folder & `posts.js`

`posts.js`

```javascript
import axios from "axios";

export function getPosts(options) {
  return axios.get("posts", options).then((res) => res.data);
}
```

Now we need "base" part of API (`http://localhost:3000/`)

- lets make an `axios` `baseApi`

`base.js`

```javascript
import axios from "axios";

export const baseApi = axios.create({ baseURL: "http://localhost:3000/" });
```

Remember, `axios.create` allows us to make a `base` version of axios that works the same as `axios`

Now `posts.js` can be...

```javascript
// using {baseApi}
import { baseApi } from "./base";

export function getPosts(options) {
  // using `baseApi` instead of 'axios
  return baseApi.get("posts", options).then((res) => res.data);
}
```

Now in `PostList` we can call our exported `getPosts(options)` instead of the `return axios.get...`

`PostList.jsx`

```jsx
import { getPosts } from "../api/posts";
//...

function loader({ request: { signal } }) {
  return getPosts({ signal });
}

//...
```

Can use this `getPosts` wherever we want

Notice that our `baseAPI` points to "http://localhost:3000/"

- we want to put in a `.env` so this is used in development, and a real API used in production

Make `.env.development` (not `.local` bc don't care if goes to `git`)

`.env.development`

```jsx
VITE_API_URL=http://localhost:3000
```

Now can make `baseApi` point to `.env.development`

`base.js`

```javascript
import axios from "axios";

export const baseApi = axios.create({ baseURL: import.meta.env.VITE_API_URL });
```

Now do same this with making `users.js`, passing `getUsers`, `UserList.jsx`, etc (won't show here)

But will show, in `<UserList/>` `btn`, we are using a **relative link** of `user.id.toString()`

`UserList.jsx`

```jsx
//...
<div className="card-footer">
  <Link className="btn" to={user.id.toString()}>
    View
  </Link>
</div>

//...
```

Now make `TodoList.jsx`, and `todos.js`

`todos.js`

```javascript
import { baseApi } from "./base";

export function getTodos(options) {
  return baseApi.get("todos", options).then((res) => res.data);
}
```

`TodoList.jsx`

```jsx
import { useLoaderData } from "react-router-dom";
import { getTodos } from "../api/todos";

function TodoList() {
  const todos = useLoaderData();

  return (
    <>
      <h1 className="page-title">Todos</h1>
      <ul>
        {todos.map((todo) => (
          <li className={completed ? "strike-through" : undefined}>{title}</li>
        ))}
      </ul>
    </>
  );
}

function loader({ request: { signal } }) {
  return getTodos({ signal });
}

export const todoListRoute = {
  loader,
  element: <TodoList />,
};
```

OK, let's make `Post.jsx` for individual posts

We set up `router.jsx`

`router.jsx`

```jsx
//...
        children: [
          { index: true, element: <Navigate to="/posts" /> },
          {
            path: "posts",
            children: [
              {
                index: true,
                ...postListRoute,
              },
              // make a `postRoute` spread call
              { path: ":postId", ...postRoute },
            ],
          },
//...
```

Now make `Post.jsx` (with our `postRoute`)

In posts.js, create `getPost` alongside `getPosts`

`posts.js`

```Javascript
import { baseApi } from "./base"

export function getPosts(options) {
  return baseApi.get("posts", options).then(res => res.data)
}

export function getPost(postId, options) {
  return baseApi.get(`posts/${postId}`, options).then(res => res.data)
}
```

Note `getPost()` receives `postId`. In `Post.jsx`, in `loader`, `postId` comes in from out `params`

`Post.jsx`

```jsx
function Post() {
  // useLoaderData() to get post info, including params below
  const post = useLoaderData()
  return ( <>
      <h1 className="page-title">{post.title}</h1>
      <div>{post.body}</div>
    </>)
}

// loader recieves `params` so we can get :postId
function loader(request: {signal}, params) {
  return getPost(params.postId, {signal})
}

export const postRoute = {
  loader,
  element: <Post/>
}
```

Now make `User.jsx`, very similar to `Post.jsx`, update `router.jsx`, update `users.js`
Correct `router.jsx` so that `{path: "users", ...userListRoute}` has children since we will need `users` & `:userId`

`router.jsx`

```jsx
//...
{
  path: "users",
  children: [
    { index: true, ...userListRoute },
    // useRoute will be like postRoute
    { path: ":userId", ...userRoute },
  ],
},
//...
```

And add `getUser()` to `users.js`

```javascript
import { baseApi } from "./base";

export function getUsers(options) {
  return baseApi.get("users", options).then((res) => res.data);
}

export function getUser(userId, options) {
  return baseApi.get(`users/${userId}`, options).then((res) => res.data);
}
```

`User.jsx`

```jsx
function User() {
  const user = useLoaderData();

  return (
    <>
      <h1 className="page-title">{user.name}</h1>
      <div className="page-subtitle">{user.email}</div>
      <div>
        <b>Company:</b> {user.company.name}
      </div>
      <div>
        <b>Website:</b> {user.website}
      </div>
      <div>
        <b>Address:</b> {user.address.street} {user.address.suite}{" "}
        {user.address.city} {user.address.zipcode}
      </div>
    </>
  );
}

function loader({ request: { signal }, params }) {
  return getUser(params.userId, { signal });
}

export const userRoute = {
  loader,
  element: <User />,
};
```

**Loading Spinner**

- We will add loading spinner
- css for it is

```css
.container.loading {
  filter: blur(5px);
  pointer-events: none;
}

.loading-spinner::after {
  content: "";
  z-index: 999;
  width: 200px;
  height: 200px;
  position: fixed;
  top: 50%;
  left: 50%;
  translate: -50% -50%;
  border-radius: 50%;
  border: 20px solid transparent;
  border-bottom-color: hsl(200, 100%, 50%);
  animation: spin infinite 1.25s ease-in;
  mix-blend-mode: multiply;
}

.loading-spinner::before {
  content: "";
  z-index: 999;
  width: 200px;
  height: 200px;
  position: fixed;
  top: 50%;
  left: 50%;
  translate: -50% -50%;
  border-radius: 50%;
  border: 20px solid transparent;
  border-top-color: hsl(200, 100%, 50%);
  animation: spin infinite 2s ease-in-out;
  mix-blend-mode: multiply;
}

@keyframes spin {
  to {
    rotate: 360deg;
  }
}
```

We put in `RootLayout.jsx`

- use `useNavigation()` to get loading state

```jsx
const { state } = useNavigation();
const isLoading = state === "loading";
```

We want the **loading spinner** to be below the nav bar so in `RootLayout.jsx` we insert our logic below `<Nav></Nav>`

- Logic will alter `className` if `isLoading` so it gets loading & spinner css if `isLoading`

`RootLayout.jsx`

```jsx
//...

return (
  <>
    <nav className="top-nav">
      <div className="nav-text-large">My App</div>
      <ul className="nav-list">
        <li>
          <Link to="/posts">Posts</Link>
        </li>
        <li>
          <Link to="/users">Users</Link>
        </li>
        <li>
          <Link to="/todos">Todos</Link>
        </li>
      </ul>
    </nav>
    <ScrollRestoration />
    {isLoading && <div className="loading-spinner" />}
    <div className={`container ${isLoading ? "loading" : ""}`}>
      <Outlet />
    </div>
  </>
);
//...
```

#### **404 Error Page**

To add a **404 Error Page**. all we have to do is add a `path: "*"` in router. Router takes most specific first... so if it can't find a page anywhere, it will THEN choose the wildcard.

`router.jsx`

```jsx
//...
 },
          { path: "todos", ...todoListRoute },
          { path: "*", element: <h1>404 - Page Not Found</h1> },
        ],
      },
    ],
  },
])
//...
```

#### **Error Page** with Generic message in development, and stack trace in production

To add **Error Page** with Generic message in development, and stack trace in production, we can just use `errorElement` in router. Can be as simple as...

`router.jsx`

```jsx
//...
path: "/",
    element: <RootLayout />,
    errorElement: <h1>Error</h1>
    children: [
//...
```

We want this to render inside out `<RootLayout/>` but outside of all of our `[children]` content. So inside of `[children]` we create our `errorElement` and give IT children

```jsx
//...
  {
    path: "/",
    element: <RootLayout />,
    children: [
      {
        // make errorElement at top inside children, then give IT children, and put all of our children inside of it
        errorElement: <h1>Error</h1>,
        children: [
          { index: true, element: <Navigate to="/posts" /> },
          {
            path: "posts",
            children: [
              {
                index: true,
                ...postListRoute,
              },
              { path: ":postId", ...postRoute },
            ],
          },
          {
            path: "users",
            children: [
              { index: true, ...userListRoute },
              { path: ":userId", ...userRoute },
            ],
          },
          { path: "todos", ...todoListRoute },
          { path: "*", element: <h1>404 - Page Not Found</h1> },
        ],
      },
    ],
  },
])
//...
```

SO IN SUMMARY, we are moving all chiuldren 1 level deeper, so we can wrap all of our children inside of `errorElement`. So if there is an error in any of the children, it will render the `errorElement` instead.

Now we want error message to be different depending on if in development or prodection, so lets make a new function we can reference in `errorElement`

At bottom of `router.jsx` we can add...

```jsx
//...

function ErrorPage() {
  const error = useRouteError()

  return (
    <>
      <h1>Error - Something went wrong</h1>
      {import.meta.env.MODE !== "production" && (
        <>
          <pre>{error.message}</pre>
          <pre>{error.stack}</pre>
        </>
      )}
    </>
  )
```

The hook `useRouteError()` gets info from the `errorElement` above.

- so it will give error of `<h1>Error - Something went wrong</h1>`
- Then we use `import.meta.env.MODE` to get our production or development mode. So if `import.meta.env.MODE !== "production"` then we show `{error.message}` & `{error.stack}`.
- Now we just make `<ErrorPage/>` the `element:` for `erroeElement`

`router.jsx`

```jsx

export const router = createBrowserRouter([
  {
    path: "/",
    element: <RootLayout />,
    children: [
      {
        // errorElement now points to function beloe (instead of <h1>...)
        errorElement: <ErrorPage />,
        children: [
          { index: true, element: <Navigate to="/posts" /> },
          {
            path: "posts",
            children: [
              {
                index: true,
                ...postListRoute,
              },
              { path: ":postId", ...postRoute },
            ],
          },
          {
            path: "users",
            children: [
              { index: true, ...userListRoute },
              { path: ":userId", ...userRoute },
            ],
          },
          { path: "todos", ...todoListRoute },
          { path: "*", element: <h1>404 - Page Not Found</h1> },
        ],
      },
    ],
  },
])

// create `ErrorPage` to use as `element` for `errorElement`
function ErrorPage() {
  const error = useRouteError()

  return (
    <>
      <h1>Error - Something went wrong</h1>
      {import.meta.env.MODE !== "production" && (
        <>
          <pre>{error.message}</pre>
          <pre>{error.stack}</pre>
        </>
      )}
    </h1>
  )
}

```

Now we wan to correct our `Post.jsx` so it can load comments and user info. To do this, we need to get info on comments & user, as well as posts.

- right now it is as follows:

`Post.jsx`

```jsx
//...
function Post() {
  const post = useLoaderData()
// ...
function loader(request: {signal}, params) {
  return getPost(params.postId, {signal})
}
//...
```

- we can INSTEAD GET ALL OF OUR INFO (`comments`, `post`, `user`) in loader.
- First lets make a `getComments()` in `comments.js`

`comments.js`

```javascript
import { baseApi } from "./base";

export function getComments(postId, options) {
  return baseApi
    .get(`posts/${postId}/comments`, options)
    .then((res) => res.data);
}
```

- Now we can change `loader` in `Post.jsx`

`Post.jsx`

```jsx
//...
function Post() {
  const { comments, post, user } = useLoaderData();

//...

function loader({ request: { signal }, params: { postId } }) {
  const comments = getComments(postId, { signal });
  const post = getPost(postId, { signal });
  const user = getUser(post.userId, { signal });

  return { comments, post, user };
}
//...
```

- Now it will get all of the infor BUT it won't work
- We need to wait for `post` to finish, before `user` can run because `user` requires `post.userId`
- Also, we need `comments` and `user` to have Promise come back too

- we can use `async`/`await`
- we could write it like this...

```jsx
async function loader({ request: { signal }, params: { postId } }) {
  const comments = await getComments(postId, { signal });
  const post = await getPost(postId, { signal });
  const user = await getUser(post.userId, { signal });

  return { comments, post, user };
```

- BUT if we slow network to 3G and watch, we see in the **waterfall** that each waits for the previous to finish

WHEN USING `async`/`await`, PUT YOUR `await`AS LATE AS POSSIBLE

- so we can leave `const post = await getPost(postId, { signal })`
- but we can put our other `await`s LATER

`Post.jsx`

```jsx
//...
function Post() {
  const { comments, post, user } = useLoaderData();

//...
async function loader({ request: { signal }, params: { postId } }) {
  const comments = getComments(postId, { signal });
  const post = await getPost(postId, { signal });
  const user = getUser(post.userId, { signal });

  return { comments: await comments, post, user: await user };
}
//...
```

THIS WAY, `comments`and `user` can start to get their info EARLIER because they haven't hit `await` until ready to export object

-All thisis saying that when you have a `Promise` you want to `await` for it before you `return` it `loader` or you will be returning `Promise` istead of info you meant to return

Now we just do all of our `jsx`for `Post.jsx` using our new `comments` and `user` info

`Post.jsx`

```jsx
//...
  return (
    <>
      <h1 className="page-title">{post.title}</h1>
      <span className="page-subtitle">
        By: <Link to={`/users/${user.id}`}>{user.name}</Link>
      </span>
      <div>{post.body}</div>

      <h3 className="mt-4 mb-2">Comments</h3>
      <div className="card-stack">
        {comments.map((comment) => (
          <div key={comment.id} className="card">
            <div className="card-body">
              <div className="text-sm mb-1">{comment.email}</div>
              {comment.body}
            </div>
          </div>
        ))}
      </div>
    </>
  );
}

//...
```

- NOTE: in the `<Link to={`/users/${user.id}`}>{user.name}</Link>`, the "/users..." needs to be and absolute link

Now we can finish `User.jsx` in the same way, by making loader also get info on `posts` and `todos`

`User.jsx

```jsx
//...
function User() {
  const { user, posts, todos } = useLoaderData()

//...
async function loader({ request: { signal }, params: { userId } }) {
  const posts = getPosts({ signal, params: { userId } })
  const todos = getTodos({ signal, params: { userId } })
  const user = getUser(userId, { signal })

  return { posts: await posts, todos: await todos, user: await user }
}
//...
```

- NOTE: here, all of our `await`s can be at the end, because NONE OF THE CALLS (`getPosts()`, `getTodos()`, `getUser()`) DEPEND ON EACH OTHER

- Now we do our jsx for `User.jsx`

`User.jsx`

```jsx
//...
  return (
    <>
      <h1 className="page-title">{user.name}</h1>
      <div className="page-subtitle">{user.email}</div>
      <div>
        <b>Company:</b> {user.company.name}
      </div>
      <div>
        <b>Website:</b> {user.website}
      </div>
      <div>
        <b>Address:</b> {user.address.street} {user.address.suite}{" "}
        {user.address.city} {user.address.zipcode}
      </div>

      <h3 className="mt-4 mb-2">Posts</h3>
      <div className="card-grid">
        {posts.map(post => (
          <PostCard key={post.id} {...post} />
        ))}
      </div>
      <h3 className="mt-4 mb-2">Todos</h3>
      <ul>
        {todos.map(todo => (
          <TodoItem key={todo.id} {...todo} />
        ))}
      </ul>
    </>
  )
}
//...
```

- Note that the jsx under `posts.map` in `<div className="card-grid">` is the same as jsx used for other times we create a Post card
- same for jsx for Todo items
- so we will make them their own components
- create a folder `components`, and add `TodoItem.jsx` & `PostCard.jsx`

`TodoItem.jsx`

```jsx
export function TodoItem({ completed, title }) {
  return <li className={completed ? "strike-through" : undefined}>{title}</li>;
}
```

- Now we can call `<TodoItem/>` in our `TodoList.jsx` also

`TodoList.jsx`

```jsx
//...
function TodoList() {
  const todos = useLoaderData();

  return (
    <>
      <h1 className="page-title">Todos</h1>
      <ul>
        {todos.map((todo) => (
          <TodoItem key={todo.id} {...todo} />
        ))}
      </ul>
    </>
  );
}
//...
```

Make our `PostCard.jsx`

`PostCard.jsx`

```jsx
import { Link } from "react-router-dom";

export function PostCard({ id, title, body }) {
  return (
    <div className="card">
      <div className="card-header">{title}</div>
      <div className="card-body">
        <div className="card-preview-text">{body}</div>
      </div>
      <div className="card-footer">
        <Link className="btn" to={`/posts/${id}`}>
          View
        </Link>
      </div>
    </div>
  );
}
```

We can go backand call this in our `PostList.jsx` (instead of having all the jsx written out in each different component)

```jsx
//...
function PostList() {
  const posts = useLoaderData();

  return (
    <>
      <h1 className="page-title">Posts</h1>
      <div className="card-grid">
        {posts.map((post) => (
          <PostCard key={post.id} {...post} />
        ))}
      </div>
    </>
  );
}
//...
```

Now we finish our `User.jsx` jsx with `<PostCard/>` & `<TodoItem/>`

`User.jsx`

```jsx
//...
function User() {
  const { user, posts, todos } = useLoaderData();

  return (
    <>
      <h1 className="page-title">{user.name}</h1>
      <div className="page-subtitle">{user.email}</div>
      <div>
        <b>Company:</b> {user.company.name}
      </div>
      <div>
        <b>Website:</b> {user.website}
      </div>
      <div>
        <b>Address:</b> {user.address.street} {user.address.suite}{" "}
        {user.address.city} {user.address.zipcode}
      </div>

      <h3 className="mt-4 mb-2">Posts</h3>
      <div className="card-grid">
        {posts.map((post) => (
          <PostCard key={post.id} {...post} />
        ))}
      </div>
      <h3 className="mt-4 mb-2">Todos</h3>
      <ul>
        {todos.map((todo) => (
          <TodoItem key={todo.id} {...todo} />
        ))}
      </ul>
    </>
  );
}
//...
```

## React Router - Get Request - Search Bar

- `get` requests (from `<input>`) get sent to `loader`
- `post` requests (from `<input>`) get sent to `action`

Take the following example of `router.jsx` & `TodoList.jsx`

`router.jsx`

```jsx
import { createBrowserRouter } from "react-router-dom";
import { TodoList } from "./TodoList";
import NewTodo from "./NewTodo";

export const router = createBrowserRouter([
  {
    index: true,
    element: <TodoList />,
    loader: ({ request: { signal } }) => {
      return fetch("http://localhost:3000/todos", { signal });
    },
  },
  {
    path: "/new",
    element: <NewTodo />,
  },
]);
```

`TodoList.jsx`

```jsx
import { Link, useLoaderData } from "react-router-dom";
import { TodoItem } from "./TodoItem";

export function TodoList() {
  const todos = useLoaderData();

  return (
    <div className="container">
      <h1 className="page-title mb-2">
        Todos
        <div className="title-btns">
          <Link to="/new" className="btn">
            New
          </Link>
        </div>
      </h1>

      <form className="form">
        <div className="form-row">
          <div className="form-group">
            <label htmlFor="query">Search</label>
            <input type="search" name="query" id="query" />
          </div>
          <button className="btn">Search</button>
        </div>
      </form>

      <ul>
        {todos.map((todo) => (
          <TodoItem key={todo.id} {...todo} />
        ))}
      </ul>
    </div>
  );
}
```

We want to be able to setup a **SEARCH BAR**

- Note that when we hit submit on a search it is just getting data from `loader` for new url with query info
- So we can setup action to get that info to the `loader` in `router`

- `React Router` has it's own `<Form>` so we change `<form>` to `<Form>`

`TodoList.jsx`

```jsx
//...
<Form className="form">
  <div className="form-row">
    <div className="form-group">
      <label htmlFor="query">Search</label>
      <input type="search" name="query" id="query" />
    </div>
    <button className="btn">Search</button>
  </div>
</Form>
//...
```

- cancels default refresh behavior on its own
- when hit Search it updates url with query
- calls the `loader`
- it is passind along all the data for the form

- Note: In `<input name="query">` "query" (OR WHATEVER YOU CALL IT) is what shows up in search bar. Then the `value` is what it "=" from search bar

So in `loader` we can

- RECEIVE URL (add `url` to `loader`s `request` object)
- use `new URL()` object to get url info, specifically `.searchParams`
- get value of out `<input name="query"> from searchbar`

`router.jsx`

```jsx
//...
export const router = createBrowserRouter([
  {
    index: true,
    element: <TodoList />,

    // 1. add url to loader request: object
    loader: ({ request: { signal, url } }) => {
      // 2. create a new variable that receives searchParams from new url object
      const searchParams = new URL(url).searchParams;

      // 3. get value of search bar from doing .get on the name passed in searchParams
      const query = searchParams.get("query");

      // 4. Add `?q=${query}` to end of url
      return fetch(`http://localhost:3000/todos?q=${query}`, { signal });
    },
  },
  {
    path: "/new",
    element: <NewTodo />,
  },
]);
```

The way this is working...

1.  By default, `<form>` (& `router` `<Form>`) have a `method="get"` and `action="/"` (action set to current url)

- So `<Form>` is saying to do a `get` request at `"/"` url
- In `router` this is the same as doing a data request (at `loader`)

2.  So it calls `loader` and passes url (because we put in `loader` `request`)
3.  Gets `.searchParams` and uses key "query" to `get` our value
4.  Calls url with that `q=`

How `loading` state works

1.  use the `useNavigation()` hook to get state

```jsx
const { state } = useNavigation();
```

2.  check to see if `state==="submitting"`. But we aren't doing submission, we are going a `get.` In a `get` request, "submitting" is equal to "loading"

`TodoList.jsx`

```jsx
//...
{
  state === "loading" ? (
    "Loading..."
  ) : (
    <ul>
      {todos.map((todo) => (
        <TodoItem key={todo.id} {...todo} />
      ))}
    </ul>
  );
}
//...
```

Hook up search bar to url

- right now, url dictates what `loader` does, but search bar not showing same thing always
- so we want thatever is in `query` in url to be copied to search bar when page is loading
- do that by doing TWO things...

  - 1. get `query` passed from `loader` (as well as `todos`). And then in search bar `<input>` make `defaultValue={query}`

so for "1)"...

`router.jsx`

```jsx
//...
export const router = createBrowserRouter([
  {
    index: true,
    element: <TodoList />,

    // we are going to return an object... create an async / await
    loader: async ({ request: { signal, url } }) => {
      const searchParams = new URL(url).searchParams;
      // add "" instaead of query if query empty
      const query = searchParams.get("query") || "";

      // return object so we can pass `query`
      return {
        //  return query from searchParams
        searchParams: { query },
        //  our fetch under key `todos
        todos: await fetch(`http://localhost:3000/todos?q=${query}`, {
          signal,
        }).then((res) => res.json()),
      };
    },
  },
//...
```

`TodoList.jsx`

```jsx
//...
export function TodoList() {
  const {
    todos,
    searchParams: { query },
  } = useLoaderData();
  const { state } = useNavigation();
//...
    <input type="search" name="query" id="query" defaultValue={query} />
//...
```

But that is only half of it. If we hit back button, search bar doesn't change. Need to manually set searchbar value each time query changes

- 2. SO SECOND PART is to

  - create a `ref`: `{const queryRef = useRef()}`
  - change `defaultValue={query}` to `ref={queryRef}`
  - create a `useEffect` to update `queryRef` to `query` each time `query` changes

  Therefore, `queryRef` (or query input) is always going to be synched to what is pushed down from `searchParams`

`TodoList.jsx`

```jsx
//...
export function TodoList() {
  const {
    todos,
    searchParams: { query },
  } = useLoaderData();
  const { state } = useNavigation();

  // create queryRef
  const queryRef = useRef();

  // create useEffect
  useEffect(() => {
    queryRef.current.value = query;
  }, [query]);
//...
    // change devaultValue to `ref={queryRef}`
    <input type="search" name="query" id="query" ref={queryRef} />
//...
```

## React Router - Post Request - Action

Use `action` to mutate or change data on server

It calls `action`basically whenever it isn't a `get` request

- `post` requests (from `<Form>`) get sent to `action`
- (`get` requests (from `<Form>`) get sent to `loader`)

When we want to change something in database from our input, we...

1.  use `<Form>` to use react router `Form`
2.  in `<Form>` change `method` to `post`

- `post` will call `action` in router

3.  create an `action` in router at level url calls (here it is "/new")

`NewTodo.jsx`

```jsx
export default function NewTodo() {
  return (
    <div className="container">
      <h1 className="page-title">New Todo</h1>
      {/* use react router form */}
      {/* change method to post */}
      <Form className="form" method="post">
        <div className="form-row">
//...
</div>
</div>
  )
```

NOTE: `<Form className="form" method="post">` also has an `action=` we can use which is set to current url by default

So now we go over to `router.jsx`. Because `<Form></Form>` `method` is "post", it will look for an action on the path sent to in our `router`

1.  Create `action` in loader

- `action` takes in a `{request}`
- call for `request.formData()` to get all data from `Form`

  - `const formData = await request.formData()`
  - this is async response, so `await`

- (if you look back, can see that `request` will be sent a key: of "title" (bc `name="title"`), and value: of input field )
- then `get` the value of key: "title"

  - `const title = formData.get("title")`

2. Now we make our `fetch` request (INSIDE of `action`)

- `fetch("http://...")`
- as second paramenter, pass object with

  - `method: "POST"`, // saying will be a POST
  - `signal: request.signal`,
  - `headers: {"Content-Type": "application/json"}`
  - `body: JSON.stringify({title, completed: false})`, // passing stringified value to post as body. title is shorthand for title: title. we are passing object {title: title, completed: false}

- have the `await fetch()` come back as `todo`
- `parse` the fetch response

  - `.then(res => res.json())`

- `return redirect("/")` // need to return something. redirect back to home page

`router.jsx`

```jsx
{
    path: "/new",
    element: <NewTodo />,
    //  create action
    //  it is an async request
    action: async ({ request }) => {
      // retrieve formData from request
      const formData = await request.formData();
      // retrieve input value (under key of "title")
      const title = formData.get("title");

      // create await fetch
      //pass object with our POST info
      const todo = await fetch("http://localhost:3000/todos", {
        method: "POST",
        signal: request.signal,
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ title }),
        // parse the fetch response
      }).then((res) => res.json());

      // return a redirect
      return redirect("/");
    },
  },
```

Now we should validate our input (`title`)

2 steps:

- 1. put a validation in `action`

  ```jsx
  if (title === "") {
    return "Title is required";
  }
  ```

- 2.  use the info in `NewTodo.jsx` with `useActionData()`

  - `useActionData()` is a hook to get info `return`ed from `action`
  - `const errorMessage = useActionData()`
  - then can print `errorMessage` in jsx render

`router.jsx`

```jsx
{
    path: "/new",
    element: <NewTodo />,
    action: async ({ request }) => {
      const formData = await request.formData();
      const title = formData.get("title");

      // add validation
      if(title === "") {
        return "Title is required"
      }

      const todo = await fetch("http://localhost:3000/todos", {
        method: "POST",
        signal: request.signal,
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ title }),
      }).then((res) => res.json());

      return redirect("/");
    },
  },
```

`NewTodo.jsx`

```jsx
//...
export default function NewTodo() {
  // get message from useActionData()
  const errorMessage = useActionData()

  return (
    <div className="container">
      <h1 className="page-title">New Todo</h1>
      <Form className="form" method="post">

        {/* display errorMessage from useActionData() hook */}
        <div>{errorMessage}</div>
//...
    </div>
  )
```

With `Form` we want to account for slow connection

- use `useNavigation()` hook to get `{state}`. `state` id different for `loader` vs. `action`

  - with `loader`, state is "loading"
  - with `action`, state is "submitting"

- so we canget state

  - `const isSubmitting = state === "submitting" || state === "loading"`
  - written as "submitting" || "loading" because it calls `action` (which uses "submitting" and THEN calls `loader` to render page)

- can make button respond differently if `isSubmitting === true`

`NewTodo.jsx`

```jsx
//...
export default function NewTodo() {

  const errorMessage = useActionData();

  //get state
  const { state } = useNavigation();
  // create isSubmitting to be true if "submitting" or "loading"
  const isSubmitting = state === "submitting" || state === "loading";

  return (
    <div className="container">
      <h1 className="page-title">New Todo</h1>
      <Form className="form" method="post">
        <div>{errorMessage}</div>
//...

          {/* disable button if waiting */}
          {/* button will read "Loading" if waiting */}
          <button disabled={isSubmitting} className="btn">
            {isSubmitting ? "Loading" : "Submit"}
          </button>
        </div>
      </Form>
    </div>
  );
}

```

## Advanced Blog Project Walkthrough

refer to [Basic Blog Project](https://github.com/rootdown001/React-Simplified-Beginner-Projects-main/tree/master/78-79-advanced-blog-project/after)

Create ability to make new posts...

Add new path to router

`router.jsx`

```jsx
//...
    {
    path: "posts",
      children: [
        {
          index: true,
          ...postListRoute,
        },
        { path: ":postId", ...postRoute },
        { path: "new", ...newPostRoute },
      ],
//...
```

Create NewPost.jsx

`NewPost.jsx`

```jsx

```

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

## `useCallback` as ref (different way to use `useCallback`)

Anytime you want to do something ONCE AN ELEMENT SHOWS UP on page, then use `useCallback` AS `useRef`

- We use `useCallback` INSTEAD OF `useRef`!
- If we use `useCallback` (instead of `useRef` + `useEffect`), it will only run when the element that your ref refers to shows up on page
- Pretty common to do - most common use is when you only want to do something once an element is rendered to screen

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

Conclusion
The Intersection Observer is my favorite of the different observer APIs since it has so many use cases from lazy loading images, to scroll based animations. It is also incredibly easy to use which is a huge bonus.

If you want to learn more about the other observer based APIs you can check out my [Resize Observer Ultimate Guide](https://blog.webdevsimplified.com/2022-01/resize-observer/)
