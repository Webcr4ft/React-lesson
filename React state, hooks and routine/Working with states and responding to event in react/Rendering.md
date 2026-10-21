# What Is Rendering in React, and How Are Components Displayed on the Screen?

In React, **rendering** is the process by which components appear in the user interface (UI), usually in the browser.

React takes your JavaScript, JSX, and CSS, figures out what the UI should look like, and displays it in the user interface.

The rendering process consists of three stages:

1. Trigger
2. Render
3. Commit

Let's look at these stages in more detail.

## 1. The Trigger Stage

The trigger stage occurs when React detects that something has changed and the user interface might need to be updated.

This change is often caused by an update to the component's state or props.

For example, noticing that it's time for dinner might trigger you to go into the kitchen to start cooking.

In the `Counter` component below, clicking the increment or decrement button updates the state and triggers React to render the component again.

### Example: Counter Component

    import { useState } from "react";

    function Counter() {
      const [count, setCount] = useState(0);

      return (
        <div>
          <h1>{count}</h1>

          <button onClick={() => setCount(count - 1)}>
            Decrement
          </button>

          <button onClick={() => setCount(count + 1)}>
            Increment
          </button>
        </div>
      );
    }

    export default Counter;

### Explanation

* `useState(0)` creates a state variable called `count` with an initial value of `0`.
* `setCount(count - 1)` decreases the count by one.
* `setCount(count + 1)` increases the count by one.
* When the state changes, React schedules the component to render again.

For example, if the count is `0` and you click the Increment button, the state updates to `1`, triggering a re-render.

## 2. The Render Stage

After a render is triggered, React enters the render stage.

During this stage, React calls your component function to figure out what the user interface should look like based on the current state and props.

React then determines what changes, if any, are needed in the displayed interface.

The **Virtual DOM** is a lightweight representation of the UI that React uses as part of its rendering process. React uses reconciliation to determine which changes are necessary.

Think of this stage as the point where you enter the kitchen, gather your ingredients, and prepare your dinner.

### Example: The Counter Component

Consider this JSX:

    <h1>{count}</h1>

If `count` is `0`, React calculates that the heading should display:

    <h1>0</h1>

When the count changes to `1`, React renders the component again and calculates that the heading should display:

    <h1>1</h1>

However, rendering alone does not mean the change has already been applied to the screen. That happens during the commit stage when a DOM update is necessary.

### Key Points

* React calls the component function again during rendering.
* The component uses its current state and props to determine the UI.
* React compares the newly calculated UI with the previous result.
* Rendering does not always lead to a visible DOM change.

## 3. The Commit Stage

The commit stage is where React applies the necessary changes to the real DOM.

The **DOM (Document Object Model)** represents the structure of a webpage that the browser displays.

During the commit stage, React applies the required DOM updates so the user can see the changes.

Think of this stage as serving the dinner you have prepared. The food becomes available to eat, just as the updated interface becomes visible to the user.

### Example: The Counter Component

Suppose the count changes from `0` to `1`.

During the commit stage, React updates the heading in the DOM so the browser displays:

    <h1>1</h1>

The user can now see the updated count on the screen.

### Key Points

* React applies necessary changes to the real DOM.
* Only the DOM changes that are needed are applied.
* The browser displays the updated interface.

## 4. Why Is React Rendering Efficient?

React helps developers build interactive applications without manually updating every DOM element.

Instead of requiring you to directly change the DOM whenever state changes, React calculates what the UI should look like and determines the necessary updates.

For example, when the count changes from `0` to `1`, React does not need to recreate the entire webpage. It can update the heading that displays the count while leaving the buttons unchanged.

However, React may call a component function again even when the resulting DOM does not change. Rendering and committing DOM changes are separate processes.

## 5. Quick Summary

| Stage | What Happens |
|---|---|
| Trigger | A state update, props change, or other update schedules a render. |
| Render | React calls components and determines what the UI should look like. |
| Commit | React applies necessary changes to the real DOM. |

## Conclusion

Rendering is how React determines what should appear in the user interface and updates the screen when necessary.

The three stages to remember are:

* **Trigger:** Something causes React to schedule a render.
* **Render:** React calls components to calculate the updated UI.
* **Commit:** React applies the necessary changes to the DOM.

Understanding these stages helps you understand how React responds to state changes and keeps your application's user interface up to date.
