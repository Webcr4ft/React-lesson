# How Do Default and Named Exports Work in React Components?

## Introduction

In earlier lessons, you learned how to work with imports and exports in JavaScript. In this lesson, we will look at how to import and export components in React.

* **Export:** Makes a component available to other files.
* **Import:** Allows a file to use a component that was exported elsewhere.

In React projects, imports and exports allow developers to reuse and combine components to build user interfaces.

In this lesson, you will learn how to export React components using **default exports** and **named exports**, and how to import them where they are needed.

## 1. Using Default Exports

In this example, we have a `Cat` component that belongs in a file called `Cat.jsx`.

We use the `.jsx` extension because this file contains JSX.

The `Cat` component returns JSX markup with a title and image for a cat called Mr. Whiskers.

### Example: Cat.jsx

    function Cat() {
      return (
        <div className="card">
          <h2>Mr. Whiskers</h2>
          <img
            src="https://cdn.freecodecamp.org/curriculum/cat-photo-app/running-cats.jpg"
            alt="Cats walking on dirt ground near a bundle of straw."
          />
        </div>
      );
    }

### Exporting the Component

If we want to use the `Cat` component in another file, we need to export it.

    function Cat() {
      return (
        <div className="card">
          <h2>Mr. Whiskers</h2>
          <img
            src="https://cdn.freecodecamp.org/curriculum/cat-photo-app/running-cats.jpg"
            alt="Cats walking on dirt ground near a bundle of straw."
          />
        </div>
      );
    }

    export default Cat;

We use the `default` keyword because this will be the default export from the module.

You can also export the component on the same line as its definition.

### Alternative Method

    export default function Cat() {
      return (
        <div className="card">
          <h2>Mr. Whiskers</h2>
          <img
            src="https://cdn.freecodecamp.org/curriculum/cat-photo-app/running-cats.jpg"
            alt="Cats walking on dirt ground near a bundle of straw."
          />
        </div>
      );
    }

Both approaches export the `Cat` component as the default export.

## 2. Importing a Default Export

You can import child components into other parent component files or import them into the root component file.

For this example, we will import the `Cat` component inside the root component file.

Every React project has a top-level component, typically called `App`, defined in a file named `App.jsx`.

### Example: App.jsx

    export default function App() {
      return null; // Return your component here
    }

This file is usually located in the `src` directory of your project.

### Importing the Cat Component

To use the `Cat` component inside the root `App` component, import it like this:

    import Cat from "./Cat";

    export default function App() {
      return null; // Return your component here
    }

The following line imports the default export from the `Cat` module:

    import Cat from "./Cat";

Now, we can return the `Cat` component inside the `App` component.

### Complete Example: App.jsx

    import Cat from "./Cat";

    export default function App() {
      return (
        <Cat />
      );
    }

### Understanding the Import

* `import` tells JavaScript that we want to use something exported from another file.
* `Cat` is the name we give the imported component.
* `"./Cat"` specifies the module's relative path.
* `<Cat />` renders the imported component.

**Important:** When importing a default export, you can choose the name used in the importing file.

For example:

    import MyCat from "./Cat";

    export default function App() {
      return <MyCat />;
    }

This works because default exports do not require the imported name to match the original component name.

This approach works well when a file is responsible for exporting a single main component.

Remember that a file can have **only one default export**, making it ideal for a file that primarily contains a single component.

## 3. Using Named Exports

Now that you know how to use default exports, let's look at named exports.

**Named exports** allow a file to share multiple components or functions.

Unlike default exports, named exports must be imported using the exact name they were exported with, unless you rename them using the `as` keyword.

This is useful when a file serves as a library of components.

For example, here is a file containing two components: `Cat` and `Dog`.

### Example: Animals.jsx

    // Animals.jsx

    export function Cat() {
      return <h2>Mr. Whiskers</h2>;
    }

    export function Dog() {
      return <h2>Fido</h2>;
    }

We use the `export` keyword individually on each component.

Both `Cat` and `Dog` are now named exports.

### Importing Named Exports

To use these components in another file, we import them using curly braces.

### Example: App.jsx

    import { Cat, Dog } from "./Animals";

    export default function App() {
      return (
        <div>
          <Cat />
          <Dog />
        </div>
      );
    }

The `{ Cat, Dog }` syntax tells JavaScript to import the specific named exports from the `Animals.jsx` file.

The names inside the curly braces must match the exported names exactly.

### Renaming Named Exports with `as`

If you need to avoid naming conflicts or prefer a different name in the current file, you can rename a component during import using the `as` keyword.

For example:

    import { Cat as Kitty } from "./Animals";

    export default function App() {
      return <Kitty />;
    }

Here:

* `Cat` is the original exported name.
* `as Kitty` renames the imported component to `Kitty`.
* `<Kitty />` uses the renamed component.

The original export remains named `Cat`. Only the name used in the importing file changes.

## 4. Combining Default and Named Exports

A file can have **one default export and multiple named exports**.

For example, consider a file containing both the `Cat` and `Dog` components.

### Example: Animals.jsx

    // Animals.jsx

    export default function Cat() {
      return <h2>Mr. Whiskers</h2>;
    }

    export function Dog() {
      return <h2>Fido</h2>;
    }

In this example:

* `Cat` is the default export.
* `Dog` is a named export.

### Importing Both Types of Exports

When importing mixed exports, the default export comes first without braces, followed by the named exports inside curly braces.

### Example: App.jsx

    // App.jsx

    import Cat, { Dog } from "./Animals";

    export default function App() {
      return (
        <div>
          <Cat />
          <Dog />
        </div>
      );
    }

In this example:

* `Cat` is the default export, so it does not use curly braces.
* `Dog` is a named export, so it requires curly braces.

Both components can now be used inside the `App` component.

## 5. Default Exports vs. Named Exports

| Feature | Default Export | Named Export |
|---|---|---|
| Keyword | `export default` | `export` |
| Number allowed per file | One | Multiple |
| Import syntax | `import Cat from "./Cat"` | `import { Cat } from "./Animals"` |
| Curly braces required when importing? | No | Yes |
| Can the imported name be changed? | Yes, directly | Yes, using `as` |
| Common use | A file's main component | Sharing multiple components or functions |

### Default Export Example

    // Cat.jsx

    export default function Cat() {
      return <h2>Mr. Whiskers</h2>;
    }

Import it:

    import Cat from "./Cat";

### Named Export Example

    // Animals.jsx

    export function Cat() {
      return <h2>Mr. Whiskers</h2>;
    }

    export function Dog() {
      return <h2>Fido</h2>;
    }

Import them:

    import { Cat, Dog } from "./Animals";

## Conclusion

Understanding how default and named exports work allows you to organize React components across multiple files and reuse them wherever they are needed.

In this lesson, we covered:

* **Default exports:** Used to export one main value from a file.
* **Named exports:** Used to export multiple components or functions from a file.
* **Importing default exports:** Does not require curly braces, and the imported name can be chosen freely.
* **Importing named exports:** Requires curly braces and matching exported names unless you use `as`.
* **Combining exports:** A file can contain one default export and multiple named exports.

As you build larger applications, choosing the right export style will help keep your code clear and maintainable, making it easier to work with components created by others.
