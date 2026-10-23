# How Do You Reference Values Using Refs?

In React, there may be situations where you need direct access to a DOM element. That's where **refs** come in handy. Refs can also store mutable values, but state is a better choice for that.

In vanilla JavaScript, you use the `getElementById()` and `querySelector()` methods to access DOM elements. In React, you use refs to access elements in the DOM.

One of the main differences is that, with refs, you don't need identifiers like IDs and classes to reference elements.

## Creating and Attaching a Ref

React provides a `useRef()` Hook that lets you create and use refs.

First, import the Hook from React:

    import { useRef } from "react";

Next, create a variable that holds the ref, initializing it to `null`:

    const sectionRef = useRef(null);

Finally, attach the ref to an element in your JSX using the `ref` attribute:

    <section ref={sectionRef}>
      {/* Section content */}
    </section>

## Reading Ref Values

If you log the ref to the console, you'll see an object with a `current` property. Initially, its value is `null`:

    console.log(sectionRef); // { current: null }

You can also log the current value directly using the `current` property:

    console.log(sectionRef.current); // null

The value of the ref depends on the component's lifecycle.

For example:

- **Before the element is mounted:** `sectionRef.current` is `null`.
- **After the element is mounted:** `sectionRef.current` refers to the DOM element the ref is attached to.
- **After the element is unmounted:** React resets `sectionRef.current` to `null`.

## Focusing an Input with a Ref

A typical example of using a ref is to focus an input element when a user clicks a button.

Here's how to do that:

    import { useRef } from "react";

    const Focus = () => {
      const inputRef = useRef(null);

      const handleFocus = () => {
        if (inputRef.current) {
          inputRef.current.focus();
        }
      };

      return (
        <div>
          <input
            ref={inputRef}
            type="text"
            placeholder="Enter text"
          />

          <button onClick={handleFocus}>
            Focus Input
          </button>
        </div>
      );
    };

    export default Focus;

In the code above:

1. The `inputRef` is created using `useRef(null)`.
2. The ref is attached to the `<input>` element using `ref={inputRef}`.
3. The button's `onClick` event calls the `handleFocus` function.
4. The function checks whether `inputRef.current` exists.
5. If it exists, the function calls its `focus()` method, placing the cursor inside the input field.

Because `<input>` is a built-in HTML element, React assigns the actual DOM element to the ref's `current` property.

Therefore, you can use:

    inputRef.current.focus();

## Best Practices for Using Refs

Here are some best practices to follow when working with refs:

- **Use refs mainly to interact with the DOM.** They are useful for focusing inputs, measuring elements, and other DOM operations.
- **Don't use refs for basic state management.** Use `useState` when a value needs to trigger a re-render and update the user interface.
- **Check that `ref.current` exists before accessing its properties.** This helps prevent errors when the ref has not yet been attached to an element or the element has been removed.

For example:

    const handleFocus = () => {
      if (inputRef.current) {
        inputRef.current.focus();
      }
    };

This ensures that `focus()` is called only when the input element is available.
