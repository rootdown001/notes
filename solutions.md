# Solutions

## Simple Dark Mode

```jsx
import { useState } from "react";

function App() {
  const [isDarkMode, setIsDarkMode] = useState(false);

  return (
    <div
      style={{
        backgroundColor: isDarkMode ? "#333" : "white",
        color: isDarkMode ? "white" : "#333",
      }}
    >
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

</br>

## `useFetch` Custom Hook

```jsx
import { useEffect, useState } from "react";

export function useFetch(myUrl, options = {}) {
  const [data, setData] = useState();
  const [isError, setIsError] = useState(undefined);
  const [isLoading, setIsLoading] = useState(undefined);

  useEffect(() => {
    setData(undefined);
    setIsError(undefined);
    setIsLoading(true);

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
      .then((d) => {
        console.log("d: ", d);
        setData(d);
      })
      .catch((e) => {
        if (e?.name === "AbortError") return;
        setIsError(e);
      })
      .finally(() => {
        if (controller.signal.aborted) return;
        setIsLoading(false);
      });

    return () => {
      controller.abort();
    };
  }, [myUrl]);

  return { data, isError, isLoading };
}
```

...sample call of `useFetch`...

- Allows toggle between 3 endpoints

```jsx
import { useState, useEffect } from "react";
import { useFetch } from "./useFetch";

const URLS = {
  USERS: "https://jsonplaceholder.typicode.com/users",
  POSTS: "https://jsonplaceholder.typicode.com/posts",
  COMMENTS: "https://jsonplaceholder.typicode.com/comments",
};

const OPTIONS = {
  method: "POST",
  body: JSON.stringify({ name: "Lance" }),
  headers: { "Content-type": "application/json" },
};

function App() {
  const [url, setUrl] = useState(URLS.USERS);

  const { data, isLoading, isError } = useFetch(url);

  return (
    <>
      <div>
        <label>
          <input
            type="radio"
            checked={url === URLS.USERS}
            onChange={() => setUrl(URLS.USERS)}
          />
          Users
        </label>
        <label>
          <input
            type="radio"
            checked={url === URLS.POSTS}
            onChange={() => setUrl(URLS.POSTS)}
          />
          Posts
        </label>
        <label>
          <input
            type="radio"
            checked={url === URLS.COMMENTS}
            onChange={() => setUrl(URLS.COMMENTS)}
          />
          Comments
        </label>
      </div>
      {isLoading ? (
        <h1>Loading...</h1>
      ) : isError ? (
        <h1>Error</h1>
      ) : (
        <pre>{JSON.stringify(data, null, 2)}</pre>
      )}
    </>
  );
}

export default App;
```

</br>

## `useArray` Custom Hook

```jsx
import { useState, useEffect, useCallback } from "react";

export function useArray(arr) {
  const [array, setArray] = useState(arr);

  // wrapped in useCallback so can pass functions without them being recreated each time
  const push = useCallback((num) => {
    setArray((currentArray) => {
      return [...currentArray, num];
    });
  }, []); // no dependencies in array b/c we never want function to be recreated

  const replace = useCallback((index, num) => {
    setArray((currentArray) => {
      return [
        ...currentArray.slice(0, index),
        num,
        ...currentArray.slice(index + 1),
      ];
    });
  }, []);

  const filter = useCallback((func) => {
    setArray((currentArray) => {
      return currentArray.filter(func);
    });
  }, []);

  const remove = useCallback((index) => {
    setArray((currentArray) => {
      return [
        ...currentArray.slice(0, index),
        ...currentArray.slice(index + 1),
      ];
    });
  }, []);

  const clear = useCallback(() => {
    return setArray([]);
  }, []);

  const reset = useCallback(() => {
    return setArray(arr);
  }, [arr]); // here we put a dependency of arr bc we are passing it and need to rerun if new value

  const addNumAtIndex = useCallback((num, index) => {
    setArray((currentArray) => {
      return [
        ...currentArray.slice(0, index),
        num,
        ...currentArray.slice(index),
      ];
    });
  }, []);

  return {
    array,
    set: setArray,
    push,
    replace,
    filter,
    remove,
    clear,
    reset,
    addNumAtIndex,
  };
}
```

...sample call of `useArray`...

- Allows call of custom array functions

```jsx
import { useArray } from "./useArray.js";

const INITIAL_ARRAY = [1, 2, 3];
// const INITIAL_ARRAY = () => [1, 2, 3];

function App() {
  const {
    array,
    set,
    push,
    replace,
    filter,
    remove,
    clear,
    reset,
    addNumAtIndex,
  } = useArray(INITIAL_ARRAY);

  return (
    <>
      <div>{array.join(", ")}</div>
      <div
        style={{
          display: "flex",
          flexDirection: "column",
          gap: ".5rem",
          alignItems: "flex-start",
          marginTop: "1rem",
        }}
      >
        <button onClick={() => set([4, 5, 6])}>Set to [4, 5, 6]</button>
        <button onClick={() => push(4)}>Push 4</button>
        <button onClick={() => replace(1, 9)}>
          Replace the second element with 9
        </button>
        <button onClick={() => filter((n) => n < 3)}>
          Keep numbers less than 3
        </button>
        <button onClick={() => remove(1)}>Remove second element</button>
        <button onClick={clear}>Clear</button>
        <button onClick={reset}>Reset</button>
        <button onClick={() => addNumAtIndex(1, 2)}>Add 1 at index 2</button>
      </div>
    </>
  );
}

export default App;
```

</br>

## `useLocalStorage` Custom Hook

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

...sample call of `useLocalStorage`...

```jsx
import { useEffect } from "react";
import { useLocalStorage } from "./useLocalStorage";

function App() {
  const [firstName, setFirstName] = useLocalStorage("FIRST_NAME", "");

  // Bonus:
  const [lastName, setLastName] = useLocalStorage("LAST_NAME", () => {
    return "Default";
  });

  // Bonus:
  const [hobbies, setHobbies] = useLocalStorage("HOBBIES", [
    "Programming",
    "Weight Lifting",
  ]);

  return (
    <>
      <div
        style={{
          display: "flex",
          flexDirection: "column",
          alignItems: "flex-start",
          marginBottom: "1rem",
        }}
      >
        <label>First Name</label>
        <input
          type="text"
          value={firstName}
          onChange={(e) => setFirstName(e.target.value)}
        />
      </div>

      {/* Bonus: */}
      <div
        style={{
          display: "flex",
          flexDirection: "column",
          alignItems: "flex-start",
          marginBottom: "1rem",
        }}
      >
        <label>Last Name</label>
        <input
          type="text"
          value={lastName}
          onChange={(e) => setLastName(e.target.value)}
        />
      </div>

      {/* Bonus: */}
      <div>{hobbies.join(", ")}</div>
      <button
        onClick={() =>
          setHobbies((currentHobbies) => [...currentHobbies, "New Hobby"])
        }
      >
        Add Hobby
      </button>
    </>
  );
}

export default App;
```

</br>

## Simple Todo List

`App.jsx`...

```jsx
import { useState } from "react";
import Item from "./Item";
import "./styles.css";

function App() {
  const [items, setItems] = useState([
    { id: "1", entry: "Test1", completed: false },
    { id: "2", entry: "Test2", completed: false },
  ]);
  const [name, setName] = useState("");
  // console.log("🚀 ~ file: App.jsx:10 ~ App ~ name:", name);

  function addTodo() {
    if (name === "") return;
    setItems((currentItems) => {
      return [
        ...currentItems,
        { id: crypto.randomUUID(), entry: name, completed: false },
      ];
    });
    setName("");
  }
  // console.log("🚀 ~ file: App.jsx:14 ~ App ~ items:", items);

  function deleteItem(id) {
    console.log("id: ", id);
    return setItems(items.filter((item) => item.id !== id));
  }

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

  return (
    <>
      <ul id="list">
        {items.map((item) => {
          return (
            <Item
              key={item.id}
              id={item.id}
              entry={item.entry}
              completed={item.completed}
              deleteItem={deleteItem}
              toggleTodo={toggleTodo}
            />
          );
        })}
      </ul>

      <div id="new-todo-form">
        <label htmlFor="todo-input">New Todo</label>
        <input
          type="text"
          id="todo-input"
          value={name}
          onChange={(e) => setName(e.target.value)}
        />
        <button onClick={addTodo}>Add Todo</button>
      </div>
    </>
  );
}

export default App;
```

`Item.jsx`...

```jsx
import React from "react";

export default function Item({ id, entry, completed, deleteItem, toggleTodo }) {
  return (
    <>
      <li className="list-item">
        <label className="list-item-label">
          <input
            type="checkbox"
            checked={completed}
            data-list-item-checkbox
            onChange={(e) => toggleTodo(id, e.target.checked)}
          />
          <span data-list-item-text>{entry}</span>
        </label>
        <button data-button-delete onClick={() => deleteItem(id)}>
          Delete
        </button>
      </li>
    </>
  );
}
```

</br>

## Form Input with `useState`

```jsx
import { useState, useMemo } from "react";
import { checkEmail, checkPassword } from "./validators";

export function StateForm() {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");
  const [isAfterFirstSubmit, setIsAfterFirstSubmit] = useState(false);

  const emailErrors = useMemo(() => {
    return isAfterFirstSubmit ? checkEmail(email) : [];
  }, [isAfterFirstSubmit, email]);

  const passwordErrors = useMemo(() => {
    return isAfterFirstSubmit ? checkPassword(password) : [];
  }, [isAfterFirstSubmit, password]);

  function onSubmit(e) {
    e.preventDefault();
    setIsAfterFirstSubmit(true);

    const emailResults = checkEmail(email);
    const passwordResults = checkPassword(password);

    if (emailResults.length === 0 && passwordResults.length === 0) {
      alert("Success");
    }
  }

  return (
    <form onSubmit={onSubmit} className="form">
      <div className={`form-group ${emailErrors.length > 0 ? "error" : ""}`}>
        <label className="label" htmlFor="email">
          Email
        </label>
        <input
          className="input"
          type="email"
          id="email"
          value={email}
          onChange={(e) => setEmail(e.target.value)}
        />
        {emailErrors.length > 0 && (
          <div className="msg">{emailErrors.join(", ")}</div>
        )}
      </div>
      <div className={`form-group ${passwordErrors.length > 0 ? "error" : ""}`}>
        <label className="label" htmlFor="password">
          Password
        </label>
        <input
          className="input"
          type="password"
          id="password"
          value={password}
          onChange={(e) => setPassword(e.target.value)}
        />
        {passwordErrors.length > 0 && (
          <div className="msg">{passwordErrors.join(", ")}</div>
        )}
      </div>
      <button className="btn" type="submit">
        Submit
      </button>
    </form>
  );
}
```

</br>

## Form Input with `useRef`

```jsx
import { useRef, useState } from "react";
import { checkEmail, checkPassword } from "./validators";

export function RefForm() {
  const emailRef = useRef();
  const passwordRef = useRef();

  const [emailErrors, setEmailErrors] = useState([]);
  const [passwordErrors, setPasswordErrors] = useState([]);
  const [isAfterFirstSubmit, setIsAfterFirstSubmit] = useState(false);

  function onSubmit(e) {
    e.preventDefault();
    setIsAfterFirstSubmit(true);

    const emailResults = checkEmail(emailRef.current.value);
    const passwordResults = checkPassword(passwordRef.current.value);

    setEmailErrors(emailResults);
    setPasswordErrors(passwordResults);

    if (emailResults.length === 0 && passwordResults.length === 0) {
      alert("Success");
    }
  }

  return (
    <form onSubmit={onSubmit} className="form">
      <div className={`form-group ${emailErrors.length > 0 ? "error" : ""}`}>
        <label className="label" htmlFor="email">
          Email
        </label>
        <input
          onChange={
            isAfterFirstSubmit
              ? (e) => setEmailErrors(checkEmail(e.target.value))
              : undefined
          }
          className="input"
          type="email"
          id="email"
          ref={emailRef}
        />
        {emailErrors.length > 0 && (
          <div className="msg">{emailErrors.join(", ")}</div>
        )}
      </div>
      <div className={`form-group ${passwordErrors.length > 0 ? "error" : ""}`}>
        <label className="label" htmlFor="password">
          Password
        </label>
        <input
          className="input"
          type="password"
          id="password"
          ref={passwordRef}
          onChange={
            isAfterFirstSubmit
              ? (e) => setPasswordErrors(checkPassword(e.target.value))
              : undefined
          }
        />
        {passwordErrors.length > 0 && (
          <div className="msg">{passwordErrors.join(", ")}</div>
        )}
      </div>
      <button className="btn" type="submit">
        Submit
      </button>
    </form>
  );
}
```

</br>

## react-hook-form

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
  const {
    register,
    handleSubmit,
    formState: { errors },
    control,
  } = useForm();

  const { field: countryField } = useController({
    name: "country",
    control,
    rules: { required: { value: true, message: "Required" } },
  });

  function onSubmit(data) {
    console.log(data);
    alert("Success");
  }

  return (
    <form onSubmit={handleSubmit(onSubmit)} className="form">
      <FormGroup errorMessage={errors?.email?.message}>
        <label className="label" htmlFor="email">
          Email
        </label>
        <input
          className="input"
          type="email"
          id="email"
          {...register("email", {
            required: { value: true, message: "Required" },
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
        <ReactSelect
          isClearable
          classNamePrefix="react-select"
          id="country"
          options={COUNTRY_OPTIONS}
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

## Basic `useReducer` example

`Counter.jsx`

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

</br>

## `useReducer` (in `useFetch` for API hook)

`App.jsx`

```jsx
import { useState, useEffect } from "react";
import { useFetch } from "./useFetch";

const URLS = {
  USERS: "https://jsonplaceholder.typicode.com/users",
  POSTS: "https://jsonplaceholder.typicode.com/posts",
  COMMENTS: "https://jsonplaceholder.typicode.com/comments",
};

const OPTIONS = {
  method: "POST",
  body: JSON.stringify({ name: "Lance" }),
  headers: { "Content-type": "application/json" },
};

function App() {
  const [url, setUrl] = useState(URLS.USERS);

  const { data, isLoading, isError } = useFetch(url);

  return (
    <>
      <div>
        <label>
          <input
            type="radio"
            checked={url === URLS.USERS}
            onChange={() => setUrl(URLS.USERS)}
          />
          Users
        </label>
        <label>
          <input
            type="radio"
            checked={url === URLS.POSTS}
            onChange={() => setUrl(URLS.POSTS)}
          />
          Posts
        </label>
        <label>
          <input
            type="radio"
            checked={url === URLS.COMMENTS}
            onChange={() => setUrl(URLS.COMMENTS)}
          />
          Comments
        </label>
      </div>
      {isLoading ? (
        <h1>Loading...</h1>
      ) : isError ? (
        <h1>Error</h1>
      ) : (
        <pre>{JSON.stringify(data, null, 2)}</pre>
      )}
    </>
  );
}

export default App;
```

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
        isError: false,
        isLoading: true,
      };
    case ACTIONS.FETCH_SUCCESS:
      return {
        data: payload.data,
        isLoading: false,
        isError: false,
      };
    case ACTIONS.FETCH_ERROR:
      return {
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

## `useContext`

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

## Router without a Library

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

`Home.jsx`

```jsx
export default function Home() {
  return <div>Home</div>;
}
```

`About.jsx`

```jsx
export default function About() {
  return <div>About</div>;
}
```

`Stre.jsx`

```jsx
export default function Store() {
  return <div>Store</div>;
}
```

# React Router `createBrowserRouter`

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

`router.jsx`

```jsx
import { createBrowserRouter } from "react-router-dom";
import Home from "./pages/Home";
import Store from "./pages/Store";
import About from "./pages/About";

export const router = createBrowserRouter([
  { path: "/", element: <Home /> },
  { path: "/store", element: <Store /> },
  { path: "/about", element: <About /> },
]);
```

`App.jsx`

```jsx
import Navbar from "./Navbar";
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
      <Navbar />
      {component}
    </>
  );
}
```

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
      </ul>
    </nav>
  );
}
```

`pages/Home.jsx`

```jsx
import Navbar from "../Navbar";

export default function Home() {
  return (
    <>
      <Navbar />
      <h1>Home</h1>
    </>
  );
}
```

`pages/Store.jsx`

```jsx
import Navbar from "../Navbar";

export default function Store() {
  return (
    <>
      <Navbar />
      <h1>Store</h1>
    </>
  );
}
```

`pages/About.jsx`

```jsx
import Navbar from "../Navbar";

export default function About() {
  return (
    <>
      <Navbar />
      <h1>About</h1>
    </>
  );
}
```

## react router

[react router](https://github.com/rootdown001/solutions-react-router)

## advanced todo list

[advanced todo list](https://github.com/rootdown001/solutions-adv-todo)

## Nice loading spinner & blur effect

styles.css

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

## Basic Blog Project Walkthrough

refer to [Basic Blog Project](https://github.com/rootdown001/React-Simplified-Beginner-Projects-main/tree/master/75-76-basic-blog-project/after)

(Below are SELECT jsx pages with comments from Kyle's video)

## React Router Actions (search and new)

refer to [React Router Actions](https://github.com/rootdown001/solutions-router-actions)

## Advanced Blog Project Walkthrough

refer to [Basic Blog Project](https://github.com/rootdown001/React-Simplified-Beginner-Projects-main/tree/master/78-79-advanced-blog-project/after)

## createPortal example / Secret button example

refer to [Solutions - usePortal](https://github.com/rootdown001/solutions-createPortal)

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

## Modal Project Walkthrough

refer to [Modal Walkthrough](https://github.com/rootdown001/React-Simplified-Advanced-Projects-main/tree/main/04-05-modal/after)

## `useMutationLogger` custom hook

writes to console "DOM Changed" everytime it changes

`useMutationLogger.js`

```javascript
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

## slow a component down (to see efects on screen)

just put this in a component to slow it down by 100 ms

```jsx
const now = performance.now();
while (now > performance.now() - 100) {
  // Do nothing
}
```

## `useOnlineStatus`

Can watch Online / Offline change with toggling network

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

## Date Picker

refer to [Date Picker](https://github.com/WebDevSimplified/React-Simplified-Advanced-Projects/tree/main/09-10-date-picker/after)

## `parseLinkHeader()`

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

## Infinite Scroll Project

refer to [Infinate Scroll Project Walkthrough](https://github.com/rootdown001/React-Simplified-Advanced-Projects-main/tree/main/16-17-infinite-scroll/after)

`App`

```jsx
import { useCallback, useEffect, useRef, useState } from "react";
import "./styles.css";
import { parseLinkHeader } from "./parseLinkHeader";

const LIMIT = 50;

export default function App() {
  const [photos, setPhotos] = useState([]);
  const [isLoading, setIsLoading] = useState(false);
  const nextPhotoUrlRef = useRef();

  async function fetchPhotos(url, { overwrite = false } = {}) {
    setIsLoading(true);
    try {
      await new Promise((res) => setTimeout(res, 2000));
      const res = await fetch(url);
      nextPhotoUrlRef.current = parseLinkHeader(res.headers.get("Link")).next;
      const photos = await res.json();
      if (overwrite) {
        setPhotos(photos);
      } else {
        setPhotos((prevPhotos) => {
          return [...prevPhotos, ...photos];
        });
      }
    } catch (error) {
      console.error(error);
    } finally {
      setIsLoading(false);
    }
  }

  const imageRef = useCallback((image) => {
    if (image == null || nextPhotoUrlRef.current == null) return;

    const observer = new IntersectionObserver((entries) => {
      if (entries[0].isIntersecting) {
        fetchPhotos(nextPhotoUrlRef.current);
        observer.unobserve(image);
      }
    });

    observer.observe(image);
  }, []);

  useEffect(() => {
    fetchPhotos(
      `http://localhost:3000/photos-short-list?_page=1&_limit=${LIMIT}`,
      {
        overwrite: true,
      }
    );
  }, []);

  return (
    <div className="grid">
      {photos.map((photo, index) => (
        <img
          src={photo.url}
          key={photo.id}
          ref={index === photos.length - 1 ? imageRef : undefined}
        />
      ))}
      {isLoading &&
        Array.from({ length: LIMIT }, (_, index) => index).map((n) => {
          return (
            <div key={n} className="skeleton">
              Loading...
            </div>
          );
        })}
    </div>
  );
}
```
