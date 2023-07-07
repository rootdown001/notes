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

## Form Input with `useState`

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
