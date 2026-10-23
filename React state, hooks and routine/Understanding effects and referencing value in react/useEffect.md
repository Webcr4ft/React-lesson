# What Are Effects in React, and How Does the useEffect Hook Work?

In React, an effect is anything that happens outside the component rendering process. That is, anything React does not handle directly as part of rendering the UI.

Common examples include fetching data, updating the browser tab's title, reading from or writing to the browser's local storage, getting the user's location, and much more. These operations interact with the outside world and are known as **side effects**.

React provides the `useEffect` Hook to let you handle those side effects. `useEffect` lets you run a function after the component renders or updates.

Let's see how the `useEffect` Hook works and why it's essential for modern React development.

## Importing and Using useEffect

To use the `useEffect` Hook, you first need to import it:

```jsx
import { useEffect } from "react";
```

Then you use it as a function, like this:

```jsx
useEffect(() => {
  // Your side effect logic goes here
}, [dependencies]);
```

The effect function runs after the component renders, while the optional `dependencies` argument controls when the effect runs.

Note that `dependencies` can be an array of reactive values (state, props, and variables declared in the component), an empty array, or omitted entirely.

Here's how all of those options control how `useEffect` works:

- **Dependencies containing reactive values:** The effect runs after the initial render and again whenever one of those values changes.
- **An empty dependency array (`[]`):** The effect runs after the initial mount. In development Strict Mode, React may run an additional setup and cleanup cycle.
- **No dependency array:** The effect runs after every render.

## Example: Using useEffect in a Counter

In this `Counter` application, we don't pass in a `dependencies` argument, so the effect runs after the component renders and every time it updates.

```jsx
import { useState, useEffect } from "react";

const Counter = () => {
  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log("Component renders");
  });

  return (
    <div
      style={{
        display: "flex",
        alignItems: "center",
        flexDirection: "column",
      }}
    >
      <h2>{count}</h2>

      <div>
        <button onClick={() => setCount(count + 1)}>
          Increase
        </button>

        <button onClick={() => setCount(count - 1)}>
          Decrease
        </button>
      </div>
    </div>
  );
};

export default Counter;
```

## Using an Empty Dependency Array

If we pass in an empty array as a dependency, the effect runs after the component first mounts.

```jsx
useEffect(() => {
  console.log("Component renders");
}, []);
```

## Using a State Variable as a Dependency

If you pass in the `count` state as a dependency, the effect runs after the initial render and whenever `count` changes.

```jsx
useEffect(() => {
  document.title = `The current count is ${count}`;
  console.log("Component renders");
}, [count]);
```

In this example:

- `document.title` updates the browser tab's title.
- `[count]` tells React to run the effect again whenever `count` changes.

## Cleaning Up Effects

If an effect sets up something that persists beyond the component's rendering lifecycle, you might need a cleanup function to stop or remove it.

For example, if your effect uses `setInterval()`, adds an event listener with `window.addEventListener()`, or connects to a server, you'll need a cleanup function to call `clearInterval()`, `window.removeEventListener()`, or disconnect from the server, respectively.

Here's the syntax for returning a cleanup function from the `useEffect` Hook:

```jsx
useEffect(() => {
  // Your side effect logic here

  return () => {
    // Cleanup logic here (optional)
  };
}, [dependencies]);
```

React runs the cleanup function before running the effect again when its dependencies change and when the component unmounts.

## Example: Cleaning Up a Scroll Event Listener

If you add a scroll event listener, you can clean it up by removing it in your cleanup function.

```jsx
useEffect(() => {
  const handleScroll = () => {
    // Handle scroll logic
  };

  window.addEventListener("scroll", handleScroll);

  return () => {
    window.removeEventListener("scroll", handleScroll);
  };
}, []);
```

In this example:

1. `handleScroll` defines the function that handles scrolling.
2. `addEventListener()` registers the function to listen for scroll events.
3. The cleanup function removes the event listener using `removeEventListener()`.
4. The empty dependency array means the effect is set up after the initial mount, with cleanup performed when the component unmounts or when React runs its development Strict Mode checks.
