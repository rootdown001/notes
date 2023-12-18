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
