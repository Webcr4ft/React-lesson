# What Are Components in React, and How Do They Work?

## What Are React Components?

**Components** are the building blocks of React applications. They allow developers to break down complex user interfaces into smaller, manageable pieces, making it easier to develop and maintain large-scale applications.

There are two types of components in React:

* Functional components
* Class-based components

In modern React, developers primarily use functional components, and every example in this lesson uses a functional component.

At a high level, you can think of components as JavaScript functions that return elements describing the user interface (UI).

This UI is described using **JSX**, a syntax extension for JavaScript that looks similar to HTML but allows you to write UI elements in a more declarative way.

## 1. A Basic Component

Let's look at an example of a React component:

    function Greeting() {
      const name = "John";

      return (
        <h1 className="title">
          {/* The result will be Hello John */}
          Hello {name}
        </h1>
      );
    }

In this example, we've defined a component called `Greeting`.

### Understanding the Code

* **`function Greeting()`**: Defines a React functional component named `Greeting`.
* **`const name = "John"`**: Creates a variable named `name` and assigns it the value `"John"`.
* **`return`**: Specifies the JSX that the component will render.
* **`<h1>`**: Defines a heading element.
* **`{name}`**: Embeds the value of the JavaScript variable `name` inside the JSX.
* **`className="title"`**: Applies a CSS class named `title` to the heading.

The browser displays the heading:

**Hello John**

### Why Do We Use `className` Instead of `class`?

In regular HTML, we use the `class` attribute to assign a CSS class to an element.

However, in JSX, we use **`className`** instead.

**HTML:**

    <h1 class="title">Hello World</h1>

**JSX:**

    <h1 className="title">Hello World</h1>

This is because `class` is a reserved keyword in JavaScript. JSX uses `className` to specify CSS classes.

### Comments in JSX

In JSX, comments must be wrapped in curly braces.

**Example:**

    {/* Block Comments */}

Outside JSX, in the body of a component, you can write regular JavaScript comments.

**Single-line comment:**

    // This is a comment

**Multi-line comment:**

    /*
    This is a
    multi-line comment.
    */

## 2. Component Naming

You may have noticed that the component name starts with a capital letter.

For example:

    function Greeting() {
      return <h1>Hello World</h1>;
    }

React uses capitalization to distinguish between custom components and built-in HTML elements.

* **Capitalized names:** React treats these as custom components.
* **Lowercase names:** React treats these as built-in HTML elements.

For example:

    <div>Hello World</div>

Here, `div` is a standard HTML element.

However:

    <Greeting />

Here, `Greeting` is a custom React component.

**Important:** Always start the names of your React components with a capital letter.

## 3. Using a Component

To use the `Greeting` component in our application, we write:

    <Greeting />

This renders an `h1` element containing the text `Hello John`, assuming the component is defined as shown earlier.

Notice that the component ends with `/>`.

In JSX, elements and components must be properly closed.

For elements or components without children, you can use self-closing syntax.

**Example:**

    <Greeting />

You can also write a self-closing HTML element like this:

    <img src="image.jpg" alt="Example" />

Both examples use `/>` to close the element.

## 4. Rendering Multiple Elements

So far, we have looked at rendering a single `h1` element. However, React components can also render multiple elements.

Consider the following example:

    function Greeting() {
      const name = "John";

      // This will throw an error
      return (
        <h1>Hello {name}</h1>
        <p>Nice to meet you.</p>
      );
    }

We are trying to display two elements:

* An `h1` element containing `Hello John`.
* A `p` element containing `Nice to meet you.`

However, this code produces an error because the two JSX elements are adjacent and do not share a parent element.

The error message may say:

`Adjacent JSX elements must be wrapped in an enclosing tag.`

### Why Does This Error Occur?

A React component must return a single JSX element at its top level.

When you want to return multiple sibling elements, you need to wrap them inside a common parent element.

There are two common ways to do this:

1. Wrap them in a regular HTML element, such as a `div`.
2. Use a React Fragment.

## 5. React Fragments

**React Fragments** allow you to group multiple elements without adding an unnecessary HTML element to the DOM.

### Method 1: Using `Fragment`

First, import `Fragment` from React.

    import { Fragment } from "react";

    function Greeting() {
      const name = "John";

      return (
        <Fragment>
          <h1>Hello {name}</h1>
          <p>Nice to meet you.</p>
        </Fragment>
      );
    }

In this example:

* `<Fragment>` groups the two elements together.
* `<h1>` displays `Hello John`.
* `<p>` displays `Nice to meet you.`

The fragment allows both elements to be returned without introducing an extra HTML element into the DOM.

### Method 2: Using Fragment Shorthand

You can also use empty JSX tags as a shorthand for React Fragments.

This is usually the simpler approach.

    function Greeting() {
      const name = "John";

      return (
        <>
          <h1>Hello {name}</h1>
          <p>Nice to meet you.</p>
        </>
      );
    }

Here:

* `<>` opens the fragment.
* `</>` closes the fragment.
* Both elements are grouped together without adding an extra DOM element.

The rendered page will display:

**Hello John**

Nice to meet you.

### Method 3: Using a `div`

Another option is to wrap the elements in a regular HTML element.

    function Greeting() {
      const name = "John";

      return (
        <div>
          <h1>Hello {name}</h1>
          <p>Nice to meet you.</p>
        </div>
      );
    }

This works because both elements are now inside one parent element.

However, unlike a Fragment, the `div` creates an additional element in the DOM.

## Conclusion

React components are reusable building blocks that help developers create user interfaces more efficiently.

In this lesson, we covered:

* **Components:** JavaScript functions that return UI elements.
* **JSX:** A syntax extension for describing user interfaces.
* **`className`:** The JSX attribute used to assign CSS classes.
* **Component naming:** Custom React components should start with capital letters.
* **Self-closing syntax:** Components and elements without children can use `/>`.
* **Rendering multiple elements:** Multiple sibling elements need a common parent.
* **React Fragments:** Allow you to group elements without adding an extra DOM element.

Understanding these concepts provides a strong foundation for building React applications and learning more advanced features in future lessons.
