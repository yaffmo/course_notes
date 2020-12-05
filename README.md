# React Router Hook Notes

\[toc\]

## [`useHistory`](https://reactrouter.com/web/api/Hooks/usehistory)

The `useHistory` hook gives you access to the [`history`](https://reactrouter.com/web/api/history) instance that you may use to navigate.

that you may use to navigate. test test

```jsx
import { useHistory } from "react-router-dom";

function HomeButton() {
  let history = useHistory();

  function handleClick() {
    history.push("/home");
  }

  return (
    <button type="button" onClick={handleClick}>
      Go home
    </button>
  );
}
```

## [`useLocation`](https://reactrouter.com/web/api/Hooks/uselocation)

The `useLocation` hook returns the [`location`](https://reactrouter.com/web/api/location) object that represents the current URL. You can think about it like a `useState` that returns a new `location` whenever the URL changes.

This could be really useful e.g. in a situation where you would like to trigger a new “page view” event using your web analytics tool whenever a new page loads, as in the following example:

```jsx
import React from "react";
import ReactDOM from "react-dom";
import {
  BrowserRouter as Router,
  Switch,
  useLocation
} from "react-router-dom";

function usePageViews() {
  let location = useLocation();
  React.useEffect(() => {
    ga.send(["pageview", location.pathname]);
  }, [location]);
}

function App() {
  usePageViews();
  return <Switch>...</Switch>;
}

ReactDOM.render(
  <Router>
    <App />
  </Router>,
  node
);
```

## [`useParams`](https://reactrouter.com/web/api/Hooks/useparams)

`useParams` returns an object of key/value pairs of URL parameters. Use it to access `match.params` of the current `<Route>`.

```jsx
import React from "react";
import ReactDOM from "react-dom";
import {
  BrowserRouter as Router,
  Switch,
  Route,
  useParams
} from "react-router-dom";

function BlogPost() {
  let { slug } = useParams();
  return <div>Now showing post {slug}</div>;
}

ReactDOM.render(
  <Router>
    <Switch>
      <Route exact path="/">
        <HomePage />
      </Route>
      <Route path="/blog/:slug">
        <BlogPost />
      </Route>
    </Switch>
  </Router>,
  node
);
```

## [`useRouteMatch`](https://reactrouter.com/web/api/Hooks/useroutematch)

The `useRouteMatch` hook attempts to [match](https://reactrouter.com/web/api/match) the current URL in the same way that a `<Route>` would. It’s mostly useful for getting access to the match data without actually rendering a `<Route>`.

Now, instead of

```jsx
import { Route } from "react-router-dom";

function BlogPost() {
  return (
    <Route
      path="/blog/:slug"
      render={({ match }) => {
        // Do whatever you want with the match...
        return <div />;
      }}
    />
  );
}
```

you can just

```jsx
import { useRouteMatch } from "react-router-dom";

function BlogPost() {
  let match = useRouteMatch("/blog/:slug");

  // Do whatever you want with the match...
  return <div />;
}
```

The `useRouteMatch` hook either:

* takes no argument and returns the match object of the current `<Route>`
* takes a single argument, which is identical to [props argument of matchPath](https://reactrouter.com/web/api/matchPath/props). It can be either a pathname as a string \(like the example above\) or an object with the matching props that `Route` accepts, like this:

```jsx
const match = useRouteMatch({
  path: "/BLOG/:slug/",
  strict: true,
  sensitive: true
});
```

