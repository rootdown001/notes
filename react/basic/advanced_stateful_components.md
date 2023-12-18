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
