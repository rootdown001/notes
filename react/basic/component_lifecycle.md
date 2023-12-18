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
