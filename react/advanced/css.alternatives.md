## CSS Modules

When you import `css` like this, is is global...

`import "./App.css"`

And when we have the following, the later takes precedent over the former (for anything conflicting)...

```jsx
import "./styles.css";
import "./App.css";
```

There are times we may want to componetize our css. Say we have the following, and we want the `h1` in each to have the `className="header"`. If we set it up this way, it doesn't work bc both jsx are getting color from second css called.

`App.css`

```css
.header {
  color: red;
}
```

`Child.css`

```css
.header {
  color: blue;
}
```

`App`

```jsx
import Child from "./Child";
import "./App.css";

export default function App() {
  return (
    <>
      <h1 className="header">App</h1>
      <Child />
    </>
  );
}
```

`Child`

```jsx
import "./Child.css";

export default function Child() {
  return (
    <>
      <h1 className="header">Child</h1>
    </>
  );
}
```

If you want to have styles and classes unique to each component, the best way is with css modules.

- not React specific
- can use with many frameworks

1.  To use modules, take any css file you want, and change to `.module.css` (for example, `Child.module.css`).

2.  Then import `styles` as in `import styles from "./Child.module.css"`

    - `styles` is an object that contains all of the classes
    - each class is followed by a random string. This is how it is differentiates that class from the ones in other modules (ex, how it differentiates between `.header` in `App.module.css` and `Child.module.css`)
    - everytime you import, it's new random strings following class

    ![css module names](/assets/img/css_modules.png)

3.  Call each `className` by object.class

    - `className={styles.header}` (where header, for example, is just the class name)
    - can use string interpolation...

    ```jsx
    className={`${styles.header} ${styles["header-lighter"]}`}
    ```

    - Note the brackets b/c a class like `header-lighter` needs "" in bracket
    - some people use camelCase (instead of -) to make it easier when nameing classs, even though not convention

    ```jsx
    className={`${styles.header} ${styles.headerLighter}`}
    ```

4.  Benefit: Allows you to work in a more componentized way. Example, if you want to make buttons and have none of the styles in that component leak out anywhere else, this is great.

5.  Now in our corrected (with css modules) example below, there is NO global css. Only modules

    - BUT can use with global too (modules + normal css)

`App.module.css`

```css
.header {
  color: red;
}
```

`Child.module.css`

```css
.header {
  color: blue;
}
```

`App`

```jsx
import Child from "./Child";
import styles from "./App.module.css";

export default function App() {
  return (
    <>
      <h1 className={styles.header}>App</h1>
      <Child />
    </>
  );
}
```

`Child`

```jsx
import styles from "./Child.module.css";

export default function Child() {
  return (
    <>
      <h1 className={styles.header}>Child</h1>
    </>
  );
}
```

## CSS in JS

Here we will look at using CSS in JS to fix issue from last section. Remember, code to fix global css issue with is...

`App`

```jsx
import Child from "./Child";
import "./App.css";

export default function App() {
  return (
    <>
      <h1 className="header">App</h1>
      <Child />
    </>
  );
}
```

CSS in JS is...

- a general term for writing your css in js file. Takes all of css out of css files, and write it in our javascript
- many different libraries - we will use one called "styled-components" (most populare, been around long time)
- to use, intall...

  - `npm i styled-components`

- then import to your code

  - `import styled from styled-components`
  - ("styled" is convention to name it)

- How it works

1.  call `styled.` whatever element we want. Ex, `styled.div` or `styled.button`
2.  uses Tagged Template Literals (instead of calling like a normal function or setting to a value). Use backtics, and write all css inside

    - it is confusing. Can read more in Kyle's blog article [Tagged Template Literals](https://blog.webdevsimplified.com/2020-03/tagged-template-literals/)

3.  Then name it, and it RETURNS A COMPONENT (so instead of having `<h1>` in your return, you make a `styled.h1`, name it, then call that component)
4.  Don't need the className (ie `className="header"`)

- we are NOT using classes for css styling

5.  Don't get good syntax highlighting - can use VS Code styled-components extension to make easier to read

6.  When you define your css in js, do it OUTSIDE of your component (to limit the number of times it re-renders)

`App`

```jsx
import Child from "./Child";
import styled from "styled-components";

const RedHOne = styled.h1`
  color: red;
`;

export default function App() {
  return (
    <>
      <RedHOne>App</RedHOne>
      <Child />
    </>
  );
}
```

`Child`

```jsx
import styled from "styled-components";

const BlueHOne = styled.h1`
  color: blue;
`;

export default function Child() {
  return (
    <>
      <BlueHOne>Child</BlueHOne>
    </>
  );
}
```

Pros & Cons

Pro...

1.  Makes it easy to use variables. In following, we can change color variable and it will change the color

```jsx
import styled from "styled-components";

const color = "blue";
const BlueHOne = styled.h1`
  color: ${color};
`;

export default function Child() {
  return (
    <>
      <BlueHOne>Child</BlueHOne>
    </>
  );
}
```

2.  Can use props. Anyplace we use Tagged Template Literal, we can pass a function (ie, `${function}`)

- below we pass function `${(props) => props.color}`, and set the color in the component. So it gets prop from component, and passes in function

```jsx
import styled from "styled-components";

const BlueHOne = styled.h1`
  color: ${(props) => props.color};
`;

export default function Child() {
  return (
    <>
      <BlueHOne color="blue">Child</BlueHOne>
    </>
  );
}
```

3.  Like CSS Modules, it's much easier to make things component based bc all styles are local. No global css to deal with

4.  Can do everything you can do in css in css in js

- ex: `&:hover` lets you do hover animation

Cons...

1.  Some syntax highlighting bugs - not the best authoring experience
2.  Co-mingling js and css - some people don't like the "code smell" of this :)
3.  For some libraries it requires more work to be done on client bc some libraries arent pre-compiled - code is being pushed to client to generate all of the styles

- Some of the libraries pre-compile

## Utility CSS
