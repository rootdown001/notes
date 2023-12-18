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
