# How Does Conditional Rendering Work in React Components?

## What Is Conditional Rendering?

Conditional rendering in React allows you to create **dynamic user interfaces**. It is used to show different content based on certain conditions or states within your application.

The most common approaches to conditional rendering include:

- Using `if` statements
- Using the ternary (`?:`) operator
- Using the logical AND (`&&`) operator

## 1. Conditional Rendering Using an `if` Statement

The simplest form of conditional rendering uses an `if` statement.

Here's an example:

    function Greeting({ isLoggedIn }) {
      if (isLoggedIn) {
        return <h1>Welcome back!</h1>;
      }

      return <h1>Please sign in</h1>;
    }

In this example, the `Greeting` component checks the `isLoggedIn` prop.

- If `isLoggedIn` is `true`, it returns a welcome message.
- Otherwise, it prompts the user to sign in.

### Using the `Greeting` Component in the Parent `App` Component

Here is an example using the `Greeting` component inside the parent `App` component:

    function App() {
      return (
        <div className="App">
          <Greeting isLoggedIn={false} />
        </div>
      );
    }

Since `isLoggedIn` is set to `false`, the output will be:

    Please sign in

## 2. Conditional Rendering Using the Ternary (`?:`) Operator

For simpler conditions, the **ternary operator (`?:`)** is often used directly within JSX.

It allows for inline conditional rendering, which can make your code more concise.

### Example

    function Greeting({ isLoggedIn }) {
      return (
        <h1>
          {isLoggedIn ? "Welcome back!" : "Please sign in."}
        </h1>
      );
    }

This code achieves the same result as the previous example but in a more compact form.

The ternary operator checks `isLoggedIn` and renders the appropriate message:

- If `isLoggedIn` is `true`, it displays `"Welcome back!"`.
- If `isLoggedIn` is `false`, it displays `"Please sign in."`.

### Ternary Operator Syntax

    condition ? expressionIfTrue : expressionIfFalse

- `condition`: The condition to check.
- `expressionIfTrue`: The value returned if the condition is `true`.
- `expressionIfFalse`: The value returned if the condition is `false`.

## 3. Conditional Rendering Using the Logical AND (`&&`) Operator

Another common pattern for conditional rendering is using the **logical AND (`&&`) operator**.

This is particularly useful when you want to render something, or nothing, based on a condition.

### Example

    function Notification({ message }) {
      return (
        <div>
          {message && <p>{message}</p>}
        </div>
      );
    }

In this example, the paragraph element containing the message is rendered only if the `message` prop is truthy.

- If `message` contains a non-empty string, the paragraph is rendered.
- If `message` is an empty string, `null`, or `undefined`, nothing is displayed for the message.

**Important:** Be careful when using `&&` with numbers. If the value before `&&` is `0`, React can render the number `0` on the screen.

For example:

    function Notification({ unreadMessages }) {
      return (
        <div>
          {unreadMessages > 0 && <p>You have unread messages.</p>}
        </div>
      );
    }

Here, the message appears only when `unreadMessages` is greater than `0`.

## Conclusion

By mastering these techniques of conditional rendering, you can build more interactive and user-friendly applications that adapt to changing data and user interactions.

### Quick Summary

| Method | Use |
|---|---|
| `if` statement | Best for more complex conditions or different return statements. |
| Ternary (`?:`) operator | Best for choosing between two outputs. |
| Logical AND (`&&`) operator | Best for displaying something only when a condition is true. |
