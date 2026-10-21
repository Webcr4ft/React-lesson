# How Do Events Work in React?

Event handling is an essential part of every interactive website.

React uses a **Synthetic Event System**, which wraps native browser events like `click`, `keydown`, and `submit`.

This system helps events behave consistently across different browsers.

## 1. How React Events Differ from HTML Events

React event handlers work similarly to native browser events, but there are some differences.

* **HTML:** Uses lowercase event attributes like `onclick` and `onsubmit`.
* **React:** Uses camelCase event attributes like `onClick` and `onSubmit`.
* **HTML:** Event handlers can be written as strings.
* **React:** Event handlers are passed as functions.

In React, the event handler function is passed to an element through a JSX attribute.

## 2. Writing an Event Handler

### Example: Regular HTML

<button onclick="alert('Button clicked!')">
  Click Me
</button>

### Example: React

function handleClick() {
  console.log("Button clicked!");
}

function App() {
  return <button onClick={handleClick}>Click Me</button>;
}

export default App;

### Key Points

* `handleClick` is the event handler function.
* `onClick` tells React which event to handle.
* `console.log()` displays a message in the browser console.
* Do not add parentheses to `handleClick` in `onClick`. Passing `handleClick` gives React a reference to the function instead of calling it immediately.

**Naming convention:** React event handler functions usually start with the prefix `handle`, such as `handleClick` and `handleSubmit`.

## 3. The Synthetic Event Object

When a user action triggers an event, React passes a **Synthetic Event object** to the event handler.

It behaves similarly to a native JavaScript event object and provides useful properties, including:

* `event.type` — The type of event.
* `event.target` — The element where the event originated.
* `event.currentTarget` — The element to which the event handler is attached.

### Example

function handleClick(event) {
  console.log(event);
}

In this example, `event` is a parameter that receives the event object. Logging it allows you to inspect its properties in the browser console.

## 4. Preventing Default Behavior and Event Bubbling

### A. Preventing Default Behavior

Some browser events have default behaviors. For example, submitting a form may cause the browser to navigate or reload the page.

You can prevent the default behavior by calling `event.preventDefault()`.

**Example:**

function handleSubmit(event) {
  event.preventDefault();
  console.log("Form submitted!");
}

function MyForm() {
  return (
    <form onSubmit={handleSubmit}>
      <input type="text" />
      <button type="submit">Submit</button>
    </form>
  );
}

export default MyForm;

**Explanation:**

* `onSubmit={handleSubmit}` connects the form to the event handler.
* `event.preventDefault()` prevents the browser's default form submission behavior.
* `console.log()` displays a message in the console.

### B. Stopping Event Bubbling

Event bubbling occurs when an event triggered on a child element propagates to its parent elements.

You can stop this behavior using `event.stopPropagation()`.

**Example:**

function handleChildClick(event) {
  event.stopPropagation();
  console.log("Child clicked!");
}

Here, `event.stopPropagation()` prevents the click event from continuing to propagate to parent elements.

## 5. Passing Data to Event Handlers

Sometimes, you need to pass additional information to an event handler. For example, you might want to identify an item before deleting it.

You can do this using an inline arrow function.

### Example

function handleDelete(id) {
  console.log("Deleting item:", id);
}

function DeleteButton() {
  return (
    <button onClick={() => handleDelete(1)}>
      Delete Item
    </button>
  );
}

export default DeleteButton;

### Explanation

* `handleDelete(id)` accepts an ID as an argument.
* `() => handleDelete(1)` creates an arrow function that calls `handleDelete` with the ID `1`.
* The handler runs when the button is clicked.

**Important:** Do not write `onClick={handleDelete(1)}` when you want the function to run only after a click. That expression calls the function immediately during rendering.

## 6. Inline Event Handlers and Performance

Inline arrow functions are common in React and are useful when you need to pass additional arguments to an event handler.

For example:

<button onClick={() => handleDelete(1)}>
  Delete Item
</button>

However, an inline arrow function creates a new function when the component renders.

This is usually fine for everyday use. In performance-sensitive situations, unnecessary function creation may matter, particularly when combined with memoized components.

**Remember:** React does not automatically eliminate every performance cost through its Virtual DOM. Use inline handlers when they make your code clearer, and optimize only when there is a demonstrated need.

## 7. Quick Summary

| Concept | Meaning |
|---|---|
| `onClick` | Handles click events |
| `onSubmit` | Handles form submission events |
| `handleClick` | A function that handles an event |
| `event` | The event object passed to a handler |
| `event.target` | The element where the event originated |
| `event.currentTarget` | The element with the attached handler |
| `event.preventDefault()` | Prevents the default browser behavior |
| `event.stopPropagation()` | Stops an event from propagating further |
| `onClick={() => handleDelete(1)}` | Passes additional data to a handler |

## Conclusion

React event handling allows you to make websites interactive by responding to user actions.

The most important things to remember are:

* Use camelCase event attributes, such as `onClick` and `onSubmit`.
* Pass functions to event attributes instead of calling them immediately.
* Use the event object when you need information about an event.
* Use `preventDefault()` to prevent default browser behavior.
* Use `stopPropagation()` to stop event bubbling.
* Use arrow functions when you need to pass additional arguments to event handlers.
