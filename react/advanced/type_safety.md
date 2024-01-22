## PropTypes

Important to manage prop types

Can do this 2 ways

1.  Reacts `PropTypes`
2.  A library like TypeScript

Here we look at React's Proptypes

- BUT if you have option to use Typescript, use it (not Proptypes)

  - Proptypes is a RUNTIME CHECK. Runs at runtime and can slow your code
  - Typescript checks code before

To use Proptypes, install the library

```bash
npm i prop-types
```

To use,

- import PropTypes library

```jsx
import PropTypes from "prop-types";
```

- take component and add `propTypes` property to it

  - now we have dropdown of all type options

- inside your `Component.propTypes` object, you define your types

  - as you do this, you see red warning lines disapear bc there is type checking

- for `{children}`, there are differen `PropTypes` you can use

  - `PropTypes.node` (what we want to do)
  - `PropTypes.element` (specific for a React Component)

Now if we save the following (which should give error bc passing string for age). But it doesn't BC THIS CHECKS AT RUNTIME

`App`

```jsx
import Child from "./Child";

export default function App() {
  return (
    <>
      <Child name="Lance" age="55" />
    </>
  );
}
```

`Child`

```jsx
import PropTypes from "prop-types";

export default function Child({ name, age, children }) {
  return (
    <>
      <div>
        <strong>Name:</strong> {name}
        <br />
        <strong>Age:</strong> {age}
        <br />
        <strong>Age (in 10 years):</strong> {age + 10}
        <p>{children}</p>
      </div>
    </>
  );
}

Child.propTypes = {
  name: PropTypes.string,
  age: PropTypes.number,
  children: PropTypes.node,
};
```

When we do run, it gives PropType error IN THE CONSOLE (not in VS Code as we write)

- By default, props are optional in PropTypes.

  - To make it required, add `.isRequired`

```jsx
Child.propTypes = {
  name: PropTypes.string.isRequired,
  age: PropTypes.number.isRequired,
  children: PropTypes.node,
};
```

Some different types you can check for

- `.any` (can be anything at all)
- `.string`
- `.number`
- `.boolean`
- `.array` (accept ANYTHING) that is array at all
- `.arrayOf(PropTypes.string)` array of strings
- `.arrayOf(PropTypes.oneOfType([PropTypes.string, PropTypes.number]))` array of strings OR numbers
- `.object` (accept ANYTHING) that is object at all
- `.oneOf(["LOADING", "ERROR"])` (to make an enum)

        [Note (from google): An enum type is a special data type that enables for a variable to be a set of predefined constants. The variable must be equal to one of the values that have been predefined for it. Common examples include compass directions (values of NORTH, SOUTH, EAST, and WEST) and the days of the week.]

- `.shape({ define how object should look})` as below, but shape can accept other properties too

```jsx
// Below we are saying the object is required, and will have street & city as strings, and both of those are required
Child.propTypes = {
  address: PropTypes.shape({
    street: PropTypes.string.isRequired,
    city: PropTypes.string.isRequired,
  }).isRequired,
};
```

- `.exact({ name: PropTypes.string, age: PropTypes.number })` object that can ONLY have these keys

A nice feature is that after you define your types, you get autocomplete for different options

If you are using class (instead of functions), it works the same

## TypeScript Setup & Props

To setup Typescript...

1.  Create project like normal

    - `npm create vite@latest`
    - Choose `TypeScript + SWC`

    Files generated are slightly different

    - `tsconfig.json` created
    - `tsconfig.node.json` created
    - `package.json` has additional dependencies for typescript
    - `eslintrc.cjs` has additional rules for typescripts
    - `vite-env.d.ts` in `src` hooks up vite with typescript features for metadata
    - files end in `.tsx` instead of `.jsx`

Now we get great type safety, autocomplete, checks for you in VS Code and you get error as you write

Best way to do type setting for props is with `:` write after props in your arguments.

- can do inline (which gets unweildy)
- or with a `type` object definition (can also use `interface`)

Inline

```tsx
export function Child({ name }: { name: string }) {
  return <h1>{name}</h1>;
}
```

`type` object definition

```tsx
type ChildProps = {
  name: string;
};

export function Child({ name }: ChildProps) {
  return <h1>{name}</h1>;
```

Another way to write functional component is with `React.FC` (react functional component)

- set `Child` with `const Child: React.FC = ({name}) => { return <h1>{name}</h1> }`
- then do type setting with `<ChildProps>` after `React.FC`
- add `: ChildProps` in arguments

```tsx
type ChildProps = {
  name: string;
};

export const Child: React.FC<ChildProps> = ({ name }: ChildProps) => {
  return <h1>{name}</h1>;
};
```

- Kind of confusing way to do it

Now to pass children...

- can use `ReactNode` as type for receiving children
- `import type { ReactNode } from "react";`
- notice the `import type` - this ensures that when you do to production it doesn't import any unnecesssary code
- beacuse this is purely for development only - don't need to push to production

```tsx
import type { ReactNode } from "react";

type ChildProps = {
  children: ReactNode;
};

export function Child({ children }: ChildProps) {
  return <h1>{children}</h1>;
}
```

- this will give error if you don't pass any children
- if you want children to be optional, write `children?`...

```tsx
type ChildProps = {
  children?: ReactNode;
};
```

How to deal with HTML props

- here we've made a Button component, and pass to it all of the normal button props `{...props}`
- also pass an `outline` boolean and ternary our style on it
- in order to `type` all of the props that come with an HTML component / element, we use...

  - `import type {ComponentProps} from "react"`

- then at end of our `type ButtonProps{}` we add `& ComponentProps<"button">`

  - the `<"button">` could be any HTML element

- Now when we pass props from `App` in `<Button>` component (like `<Button disabled >`) we get a dropdown of all options of everything that can be passed with button

`App`

```tsx
import Button from "./Button";

export default function App() {
  return (
    <Button outline disabled>
      I am a button
    </Button>
  );
}
```

`Button`

```tsx
import type { ComponentProps } from "react";

type ButtonProps = {
  outline?: boolean;
} & ComponentProps<"button">;

export default function Button({ outline, ...props }: ButtonProps) {
  return (
    <button
      style={{ border: outline ? "1px solid red" : undefined }}
      {...props}
    ></button>
  );
}
```

Now you can also use `ComponentProps` with CUSTOM COMPONENTS

```tsx
type ButtonProps = {
  outline?: boolean;
} & ComponentProps<typeof Child>;
```

- this is handy in case the type of a Component are not exported

## TypeScript useState

If you pass `useState` a value, it INFERS type based on value

- here it infers type is `string` (bc passed ""). If you try to pass number you get error

```tsx
import { useState } from "react";

export default function App() {
  const [value, setValue] = useState("");

  return (
    <input
      type="text"
      value={value}
      onChange={(e) => {
        setValue(e.target.value);
      }}
    />
  );
}
```

If you don't pass anything, it INFERS type is `undefined`

- so then if you try to pass string, you get error

If you want to pass nothing, need to use generic `useState`

- in <> state `string`
- now expects string | undefined (string bc stated, undefined bc passed nothing)
- could also sate <string | undefined> - same thing

```tsx
import { useState } from "react";

export default function App() {
  const [value, setValue] = useState<string>();

  return (
    <input
      type="text"
      value={value}
      onChange={(e) => {
        setValue(e.target.value);
      }}
    />
  );
}
```

To pass in an empty array, need to still state what type of array

- if you don't the type is `never[]`
- so pass `[]` and define as type `<number[]>`

```tsx
import { useEffect, useState } from "react";

export default function App() {
  const [value, setValue] = useState<number[]>([]);

  useEffect(() => {
    console.log("value: ", value);
  }, [value]);

  return (
    <button
      onClick={() => {
        setValue([1, 2, 3]);
      }}
    >
      Set 1, 2, 3
    </button>
  );
}
```

If you DO pass something simple, l;ike string, can just leave off type bc it's smart enough to infer type string

- Kyle tries to let TS infer as much as possible

## TypeScript useRef

- By far most annoying to work with in TS

Couple errors with code below...

- `value.current` & `ref=` have type errors

```tsx
import { useRef } from "react";

export default function App() {
  const inputRef = useRef();
  const value = useRef();

  value.current = 10;

  return <input ref={inputRef} />;
}
```

Ref expects to be passed RefObject | undefined (CANNOT BE NULL)

- but if you pass nothing, it sets to `undefined` and you get error
- So, for using html element with useRef (like input), YOU NEED TO PASS NULL
- BUT you also need to type it. If you don't can only be `null`

  - For `input` you pass `<HTMLInputElement>`
  - its just "HTML" then shows autocomplete dropdown
  - Doing it this way gives you ReadOnly (immutibale) - you are saying you want TS to handle it for you
  - So below is how you do it for ref with elements

```tsx
import { useRef } from "react";

export default function App() {
  const inputRef = useRef<HTMLInputElement>(null);

  return <input ref={inputRef} />;
}
```

BUT say you want to be able to mutate it, like the `value` below

```tsx
import { useRef } from "react";

export default function App() {
  const inputRef = useRef<HTMLInputElement>(null);
  const value = useRef();

  value.current = 10;

  return <input ref={inputRef} />;
}
```

- want to pass a type, or set with your starting value or undefined (NEVER PASS NULL)

```tsx
import { useRef } from "react";

export default function App() {
  const inputRef = useRef<HTMLInputElement>(null);
  const value = useRef<number>();

  value.current = 10;

  return <input ref={inputRef} />;
}
```

In summary, 2 rules

1.  For `useRef` with an HTML element, pass element type (`<HTMLInputElement`) AND pass `null`
2.  For `useRef` NOT used with html element, pass your type (like `<number>` or `<number[]>`) or set with a number (like `useRef(0)`) or pass undefined (BUT never pass null)

## TypeScript useReducer

Not really anything specifically about react, but is a TS trick

Start with following code...

```tsx
import { useEffect, useReducer } from "react";

function reducer(state, action) {
  switch (action.type) {
    case "INCREMENT":
      return { ...state, count: state.count + action.increaseBy };
    case "DECREMENT":
      return { ...state, count: state.count - action.decreaseBy };
  }
}

export default function App() {
  const [state, dispatch] = useReducer(reducer, { count: 0 });

  return (
    <div>
      <button onClick={() => dispatch({ type: "DECREMENT", decreaseBy: 1 })}>
        -
      </button>
      {state.count}
      <button onClick={() => dispatch({ type: "INCREMENT", increaseBy: 1 })}>
        +
      </button>
    </div>
  );
}
```

If we give `state` a type, as below, we get an error...

```tsx
import { useEffect, useReducer } from "react";

type State = {
  count: number
}

function reducer(state: State, action) {
  switch (action.type) {
    case "INCREMENT":
      return { ...state, count: state.count + action.increaseBy };
    case "DECREMENT":
      return { ...state, count: state.count - action.decreaseBy };
  }
}

export default function App() {
  //.........
```

- bc our `reducer` doen't always return `state`. Hover over `reducer` and see it returns `state` | `undefined`
- bc we don't have a default case set up
- can set up default to return `state` or throw an error

Now `state` is properly typed below

- can see if you hover over `reducer` its not saying " | undefined"

```tsx
import { useEffect, useReducer } from "react";

type State = {
  count: number;
};

function reducer(state: State, action) {
  switch (action.type) {
    case "INCREMENT":
      return { ...state, count: state.count + action.increaseBy };
    case "DECREMENT":
      return { ...state, count: state.count - action.decreaseBy };
    default:
      throw new Error("Got to default");
  }
}
//........
```

Now we have to type our `action`

- will show some ways that seem to make sense, then see actual way

- can do as follows with `type`, `increaseBy`, `decreaseBy`,

```tsx
type Action = {
  type: string;
  increaseBy: number;
  decreaseBy: number;
};
```

BUT get error. If hover over button using `decreaseBy` can see it wants `increaseBy` also. So need to make each optional

```tsx
type Action = {
  type: string;
  increaseBy?: number;
  decreaseBy?: number;
};
```

BUT still get error, now on the `action.increaseBy` & `action.decreaseBy` saying its possibly undefined

- Kyle says it's bc its saying don't know if have `increasedBy` when type is "INCREMENT"

SO ANSWER IS to break out the type into individual different actions in `type Action`

- it is saying the `action` can be type `"INCREMENT"` with `increasedBy` OR type `"DECREMENT"` with `decreasedBy`

```tsx
import { useEffect, useReducer } from "react";

type State = {
  count: number;
};

type Action =
  | {
      type: "INCREMENT";
      increaseBy: number;
    }
  | { type: "DECREMENT"; decreaseBy: number };

function reducer(state: State, action: Action) {
  switch (action.type) {
    case "INCREMENT":
      return { ...state, count: state.count + action.increaseBy };
    case "DECREMENT":
      return { ...state, count: state.count - action.decreaseBy };
    default:
      throw new Error("Got to default");
  }
}
```

So with `useReducer`, break out `type Action` into the different types for different cases

- so now since we did all the typing in the `reducer` function, the types in the code in our `useReducer` hook is typed correctly. If we hover over the following we see that `state` have `count` as number, and `dispatch` takes an Action type

```tsx
//...
export default function App() {
  const [state, dispatch] = useReducer(reducer, { count: 0 });

//...
```

## TypeScript useContext

`useContext` requires quite a bit of setup bc of way `useContext` works

Look at following code...

`App`

```tsx
import { createContext, useEffect, useState } from "react";
import { Child } from "./Child";

type User = {
  id: string;
  name: string;
  age: number;
};

export const Context = createContext();

export default function App() {
  const [users, setUsers] = useState<User[]>([]);

  useEffect(() => {
    getUsers().then(setUsers);
  }, []);

  function addUser({ name, age }: { name: string; age: number }) {
    setUsers((prevUsers) => {
      return [...prevUsers, { id: crypto.randomUUID(), name, age }];
    });
  }

  return (
    <Context.Provider value={{ users, addUser }}>
      <Child />
    </Context.Provider>
  );
}

function getUsers() {
  return Promise.resolve([
    { id: crypto.randomUUID(), name: "Lance", age: 55 },
    { id: crypto.randomUUID(), name: "Liz", age: 50 },
  ]);
}
```

Error at `createContext` bc it always expects a default value

- our context is getting an array of users and `addUser` function. If was just array of users, could pass `createContext({users: []})`.
- but what we donn't want to do that bc context involves more data - we also had `addUser`
- ANSWER is to pass `null` then everything is defined in your component function

```tsx
export const Context = createContext(null);
```

Now we give `createContext` a type, `ContextType`

```tsx
//...
type User = {
  id: string;
  name: string;
  age: number;
};

type ContextType = {
  users: User[];
  addUser: ({ name, age }: { name: string; age: number }) => void;
  // we have => bc addUser returns nothing - just does setUsers
};

export const Context = createContext<ContextType | null>(null);
// ...
```

`ContextType` includes `users` & `addUser` (the same things passed as props to our `Context.Provider`)

BUT this doesn't solve all errors - in component where we `useContext`, get error at `const { users } = useContext(Context)` - says `users` is not type `ContextType | null` - THIS IS BECAUSE in `useContext(Context)` COULD BE NULL. See below

- "CODEGPT says - The type error you are encountering in your useContext is likely because the context value you are providing is null. The useContext hook requires a non-null context value to function correctly."

`Child`

```tsx
import { useContext } from "react";
import { Context } from "./App";

export function Child() {
  const { users } = useContext(Context);

  return (
    <ul>
      {users.map((user) => {
        return <li>{user.name}</li>;
      })}
    </ul>
  );
}
```

We fix this by creating custom hook below our `createContext` statement.

- don't need to export our `Context` - remove that Bc our hook will handle
- now we make hook that returns `usersContext`, and CHECKS FOR NULL

```tsx
export const Context = createContext<ContextType | null>(null);

export function useUsers() {
  const usersContext = useContext(Context);
  if (usersContext == null) {
    throw new Error("Must use within Provider");
  }
  return usersContext;
}
```

- then import our `useUsers` in `Child`
- use this hook in Child instead of original `useContext(Context)`
- again, works bc our hook (setup in `App`) has `useContext` and checks so null - so `{users}` can't be null

```tsx
import { useUsers } from "./App";

export function Child() {
  const { users } = useUsers();

  return (
    <ul>
      {users.map((user) => {
        return <li>{user.name}</li>;
      })}
    </ul>
  );
}
```

Rules:

- `createContext` always expects a default value - use `null`
- create type for Context (here `ContextType`). Pass `<contextType | null>` (it's `| null` bc we pass null as starting value)
- `useContext(Context)` doesn't work properly if one type possibility is null -

  - so way to handle is to create custom hook (near `createContext`) - a hook that does a `useContext(Context)` and then checks for null. So the returned value is not null. Now the type is right
  - This works as long as `Child` is wrapped in `Context.Provider`. (Inside `Context.Provider` will never be null bc of this hook)

Full Correct Code

`App`

```tsx
import { createContext, useContext, useEffect, useState } from "react";
import { Child } from "./Child";

type User = {
  id: string;
  name: string;
  age: number;
};

type ContextType = {
  users: User[];
  addUser: ({ name, age }: { name: string; age: number }) => void;
};

export const Context = createContext<ContextType | null>(null);

// custom hook to deal with useContext and check for null
export function useUsers() {
  const usersContext = useContext(Context);
  if (usersContext == null) {
    throw new Error("Must use within Provider");
  }
  return usersContext;
}

export default function App() {
  const [users, setUsers] = useState<User[]>([]);

  useEffect(() => {
    getUsers().then(setUsers);
  }, []);

  function addUser({ name, age }: { name: string; age: number }) {
    setUsers((prevUsers) => {
      return [...prevUsers, { id: crypto.randomUUID(), name, age }];
    });
  }

  return (
    <Context.Provider value={{ users, addUser }}>
      <Child />
    </Context.Provider>
  );
}

function getUsers() {
  return Promise.resolve([
    { id: crypto.randomUUID(), name: "Lance", age: 55 },
    { id: crypto.randomUUID(), name: "Liz", age: 50 },
  ]);
}
```

`Child`

```tsx
import { useUsers } from "./App";

export function Child() {
  const { users } = useUsers();

  return (
    <ul>
      {users.map((user) => {
        return <li>{user.name}</li>;
      })}
    </ul>
  );
}
```

## TypeScript Generics

Sometimes we don't know what he are passing in an array

- Generics come into play when we want the code to be smart enough know what we are passing
- We handle by creating a generic type

In code below, in `List` component, everything is working when we use `type Item`

- but this doesn't allow us to put anything unknown in `items` array (other than id & name)

`App`

```tsx
import { List } from "./List";

export default function App() {
  return (
    <List
      items={[
        { id: 1, name: "Lance" },
        { id: 2, name: "Liz" },
      ]}
      getKey={(item) => item.id}
      renderItem={(item) => <div>{item.name}</div>}
    />
  );
}
```

`List`

```tsx
type Item = {
  id: number;
  name: string;
};

type ListProps = {
  items: Item[];
  getKey: (item: Item) => React.Key;
  renderItem: (item: Item) => React.ReactNode;
};

export function List({ items, getKey, renderItem }: ListProps) {
  return (
    <div>
      {items.map((item) => {
        return <div key={getKey(item)}>{renderItem(item)}</div>;
      })}
    </div>
  );
}
```

- But when we type our `item` like this (`type Item = {id: number name: string}`) we can't have anything else in array
- we want our items array to be able to accept other properties that we don't necessarily know about (like age)

So we use TypeScript Generics

- will make code be able to look at what it's receiving and figure it out
  -So below we...

  - get rid of `type Item`
  - put a generic type on our `ListProps` -> `ListProps<Item>` - common syntax is `<T>` but can be anything - below we call it `<Item>` (not to be confused withour other `type Item`)
  - Then we add the generic to 1) our function so it can take in generic type and 2) to `ListProps` in our arguments type - `export function List<Item>(....: ListProps<Item>)`

`List`

```tsx
type ListProps<Item> = {
  items: Item[];
  getKey: (item: Item) => React.Key;
  renderItem: (item: Item) => React.ReactNode;
};

export function List<Item>({ items, getKey, renderItem }: ListProps<Item>) {
  return (
    <div>
      {items.map((item) => {
        return <div key={getKey(item)}>{renderItem(item)}</div>;
      })}
    </div>
  );
}
```

So this is saying

- we have an array of items, don't know what type is
- `getKey` is taking in some item, returning a key
- `renderItem` is taking in an item and returning a React element

- in our `App ` it's automatically infering... that we know our `items` is of type `Item

  - very cool bc if you hover over `item` in `App` return, it shows the type BASED ON WHAT IS ACTUALLY IN THER ARRAY.
  - If array has `{id: 1, name: "Lance", age 55}`, then it shows type is `{id: number, name: string, age: number}`
  - If array has `{id: 1, name: "Lance"}`, then it shows type is `{id: number, name: string}`

- Sometimes you need to manually type your generic if React can't figure it out for you.

  - just do a manual type behind your component name that you are rendering - `List` here
  - then manually type... so is `<List<{id: number, name: string, age?: number}>......` - here we made `age` optional
  - when you do this, now if you hover over item, it shows type of `{id: number, name: string, age?: number | undefined}`

```tsx
import { List } from "./List";

export default function App() {
  return (
    <List<{ id: number; name: string; age?: number }>
      items={[
        { id: 1, name: "Lance" },
        { id: 2, name: "Liz" },
      ]}
      getKey={(item) => item.id}
      renderItem={(item) => <div>{item.name}</div>}
    />
  );
}
```

Recap...

1.  Specify your function as a generic function - `export function List<Item>()`
2.  Specify your prop Type as a generic prop type - `type ListProps<Item> = {...}` (in definition and function argument typing)
3.  Whatever you have in your generic, just pass along as generic, never hardcoding anything - `Item`
4.  In other component (function that renders your Component - ie `App` renders `List`) it will infer the type based on what is in the generic array / object
5.  If it can't infer it, you can manualy type when calling `List`
