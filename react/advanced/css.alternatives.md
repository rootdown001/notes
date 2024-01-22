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

## Utility-First CSS (Tailwind)

Use these instructions to set up Tailwind in React project in Vite

[Install Tailwind w/ Vite](https://tailwindcss.com/docs/guides/vite)
... then in `main.jsx` do `import "./index.css"`

- creates `tailwind.config.js`

May different libraries - Tailwind is most popular

Also install VS Code Tailwindcss extension for autocomplete

To know...

1.  By default, Tailwind removes MANY of the default styles the browser has.

- When you load, will see default text change. This is because it is removing default browser styles to make everything set back to same starting place for browsers

2.  Notice `<h1></h1>` is not big. Same size as `<h6></h6>`. Again, because tailwind is removing default styles by browser

3.  Broken down into very small individual classes that do specific things

- see Tailwind site...
- just use search bar to find any classes you want
- have a ton of classes for anything you want to do.

4.  Has guardrails - keeps your styles congruent

- Ex, if you choose `pl` for padding-left, you get a dropdown of different padding sizes. By using from dropdown (esp as numbers get bigger) you pick ones to be continuous (ex, has `pl-32` then `pl-36`). This way you don't end up with randome ones (like 33) that you try to match later
- but can customize anything

  - `pl-[35px]` - use brackets to customize

5.  Utility-first, like Tailwind, helps make designs more cohesive.

6.  Can do anything css can do

- Ex for hover, `hover:text-red-500` (now will be red when hovered)

7.  Can also write your own css in `index.css`

8.  The bundle is not big, because it only includes the classes you are using

- can see this by inspecting css in the head in elements. At top is all of the css that is OVERRIDING browser styles. At bottom is css from your code.

9.  Makes easy to make it very componentized

Con...

- difficult to read html sometimes
- can be difficult to override

  - say you use props and bring in a className. Can be unpredictable. Can't just put 2 classes on something and expect the last one to override the first one

`App`

```jsx
import Child from "./Child";

export default function App() {
  return (
    <>
      <h1 className=" text-red-500">App</h1>
      <Child className="text-blue-500" />
    </>
  );
}
```

`Child`

```jsx
export default function Child({ className }) {
  return (
    <>
      <h1 className={`text-red-500 ${className}`}>Child</h1>
    </>
  );
}
```

- in code above, where `className` prop is sent to `Child` and used in template literal, you can't necessarily expect that "Child" will be blue (which you would expect bc the template literal is 2nd)

  - can't put 2 classes on something (like 2 text colors in `Child`) and expect it to be the last one. Does this bc of "css and specificity"

Kyle likes Tailwind bc tons of classes to use, but with guardrails

## CSS Frameworks

Full-fledged framework - Bootstrap is a popular one

Install by following instructions...

[Bootstrap Installation](https://react-bootstrap.netlify.app/docs/getting-started/introduction)

- installs bootstrap & react-bootstrap

  - `npm install react-bootstrap bootstrap`

- then import bootstrap css we need (in `main.jsx`, for example)

```jsx
{
  /* The following line can be included in your src/index.js or App.js file */
}
import "bootstrap/dist/css/bootstrap.min.css";
```

As soon as we do this, can see fonts change (from browser default fonts to bootstrap default)

Bootstrap works by IMPORTING COMPONENTS

- predefined components

```jsx
import Button from "react-bootstrap/Button";

export default function App() {
  return (
    <>
      <h1>App</h1>
      <Button>Button</Button>
    </>
  );
}
```

- this gives bootstrap Button on screen
- can change the props to `Button` component

  - here we add `varient` prop of "danger" to make button danger red

```jsx
import Button from "react-bootstrap/Button";

export default function App() {
  return (
    <>
      <h1>App</h1>
      <Button variant="danger">Button</Button>
    </>
  );
}
```

In Bootsrap docs, entire section on Components

- [Components](https://react-bootstrap.netlify.app/docs/components/accordion)

  - shows everything you can do with Component

Bootstrap framework gives us fully set up components

Pros...

- good if don't like writing much css
- can make design less of an issue

Cons...

- looks like generic Bootstrap (not unique)
- difficult to customize to look different

Other libraries can give components that you style more yourself

- ex: [HeadlessUI](https://headlessui.com/) (built by Tailwind folks)
- Everything is unstyled, and you style
- But get all the components with js done

## Comparison of all

Kyle re-wrote our Posts project 4 diff ways with vanilla, utility (Tailwind), css in js, framework (Bootstrap)

[css 5 diff ways code](https://github.com/WebDevSimplified/React-Simplified-Advanced-Projects/tree/main/23-css-alternative-examples)

Doesn't matter what you use, can all get basically same result

1.  Vanilla CSS

- tons of different classNames

2.  Modules

- with modules, it is typical to put css module in folder with module jsx
- ex, in `components/Card` we see

  - `Card.jsx`
  - `Card.module.css`

- ended up with more components in modules css bc instead of vanilla with all the calss names, was easier to make a few more modules

  - still has `styles.css` but only has a few selectors

    - `*`, `body`, and few styles for handling utilty css like `.mb-4`, `.mb-2`

  - when you look at files (`jsx`) can see there is little to no css in them (just a few utility styles)
  - components handle everything

3.  CSS in JS,

- All of the js in css elements he made start with "Styled...", ex, body is `StyledBody`

- still has `styles.css` but only has a few selectors

  - `*`, `body`, and few styles for handling utilty css like `.mb-4`, `.mb-2`

- jsx looks different bc not seeing `<div></div>` etc, just StyledElements
- don't really see classNames anywhere - all defined outside of component
- some use the `props` variables in the css in js

4.  Utility (Tailwind)

- slight differences in color & font bc only used built in values
- `styles.css`

  - has more css than other ways bc some things in Tailwind are hard to do (like spinner) so he put that in `styles.css`
  - used `@apply` to use some Tailwind in `styles.css` (but they don't recommend)

`styles.css` (segment example)

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

.form-group textarea {
  @apply p-2 rounded-lg border-2 border-slate-900;
}
```

- has a few styles in `index.html`

  - fine to do and pretty normal

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="/vite.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vite + React</title>
  </head>
  <!-- see styles in body below -->
  <body class="bg-slate-100 text-2xl">
    <div id="root"></div>
    <script type="module" src="/src/main.jsx"></script>
  </body>
</html>
```

In components there are a lot of utility classNames setting the style

- as they get more complicated, get really big with utility classnames

```jsx
export function PageHeader({ children, btnSection, subtitle }) {
  return (
    <>
      <h1
        className={`text-5xl flex justify-between items-center gap-8 font-bold ${
          subtitle ? "mb-4" : "mb-8"
        }`}
      >
        {children}
        {btnSection && <div className="text-2xl font-normal">{btnSection}</div>}
      </h1>
      {subtitle && <div className="mb-8 text-3xl">{subtitle}</div>}
    </>
  );
}
```

Can manage it getting too big by breaking out some into className ternery

- below, `colorClasses` is a ternery, and then IT is added as className in Component

example

```js
export function Button({
  className,
  outline = false,
  AsComponent = "button",
  ...props
}) {
  const colorClasses = outline
    ? "border-2 border-sky-800 text-sky-800 hover:bg-sky-100 focus:bg-sky-100 disabled:bg-slate-200 disabled:text-slate-500 disabled:border-slate-500"
    : "bg-sky-800 text-slate-100 hover:bg-sky-700 focus:bg-sky-700 disabled:bg-slate-500";

  return (
    <AsComponent
      className={`no-underline px-4 py-3 rounded-lg cursor-pointer disabled:cursor-not-allowed ${colorClasses} ${className}`}
      {...props}
    />
  );
}
```

5.  Framework (Bootstrap)

- Looks somewhat different bc using default bootstrap
- removed a lot of custom code bc Bootstrap has its own
- `styles.css` has some custom css bc some things you just can't do in bootstrap (like spinner css)
- can see in pages, LOTS of compoonent imports from Bootstrap (they have component for anything you can think of)
