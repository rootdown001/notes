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
