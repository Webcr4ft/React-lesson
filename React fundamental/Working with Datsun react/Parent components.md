# How Do You Pass Props from a Parent Component to a Child Component in React?

## Introduction

In the previous lessons, we learned how to build small components in React like this:

    function Greeting() {
      const developerName = "Jessica";

      return <h1>Hi {developerName}!</h1>;
    }

We can choose to nest this component inside another parent component or the root component like this:

    function App() {
      return <Greeting />;
    }

    function Greeting() {
      const developerName = "Jessica";

      return <h1>Hi {developerName}!</h1>;
    }

## Why Use Props?

While this code will run fine and display the result of `Hi Jessica!` on the screen, it is not that flexible of a component.

What if we wanted to display a different name like Naomi, Tom, or Oliver?

This is where **React props** come in.

- **Props** is short for *properties*.
- Props are the way for parent components to pass data down to child components.
- Props can be of any type:
  - Strings
  - Numbers
  - Booleans
  - Objects
  - Arrays

## Passing Props to a Child Component

Let's update our example from earlier to now accept a `name` prop:

    function App() {
      return <Greeting name="Jessica" />;
    }

    export default App;

    function Greeting(props) {
      console.log(props);

      return <h1>Hi {props.name}!</h1>;
    }

For the child component called `Greeting`, we are now using `props.name` instead of hardcoding the name `"Jessica"`.

We are also logging `props` to the console, which shows an object.

Then, inside the parent `App` component, we are passing the value to the `name` prop so it can be passed down to the child.

The result will be the same on the screen as earlier, but now we have created a more flexible component.

### Reusing the Child Component

Now we have the ability to reuse the child component several times and pass in different names each time:

    function App() {
      return (
        <>
          <Greeting name="Naomi" />
          <Greeting name="Tom" />
          <Greeting name="Jessica" />
          <Greeting name="Oliver" />
        </>
      );
    }

## Destructuring Props

You can also choose to use **object destructuring** in the props to make it more readable.

Here's how you could rewrite the `Greeting` component:

    function Greeting({ name }) {
      return <h1>Hi {name}!</h1>;
    }

This code achieves the same result but makes it clearer which props the component is expecting to receive.

## The `children` Prop

React also provides a special prop called `children`.

Any JSX you place between a component's opening and closing tags is passed to the component as `children`.

### Example

    function Card({ children }) {
      return <div className="card">{children}</div>;
    }

    function App() {
      return (
        <Card>
          <h2>Hello</h2>
        </Card>
      );
    }

In this case, the JSX inside `<Card>...</Card>` becomes the `children` prop.

### Rendered Output in the DOM

    <div class="card">
      <h2>Hello</h2>
    </div>

This pattern is often used for **component composition**, where a component wraps other UI elements.

## Passing Multiple Props With the Spread Operator

Sometimes, you can have a lot of properties that you need to pass as props.

Instead of passing them one by one, you can use the **spread operator (`...`)** after converting them to an object.

### Example: Creating a `DeveloperCard` Component

Here is an example of a new child component called `DeveloperCard`:

    function DeveloperCard({ name, age, country }) {
      return (
        <div className="developer-card">
          <h1>Developer: {name}</h1>
          <p>Age: {age}</p>
          <p>Country: {country}</p>
        </div>
      );
    }

This `DeveloperCard` component accepts three props:

- `name`
- `age`
- `country`

### Passing Multiple Props From the Parent Component

In the parent `App` component, we can use the spread syntax to pass all the properties from an object as individual props to the child component:

    function App() {
      const developerObj = {
        name: "Alice",
        age: 30,
        country: "USA",
      };

      return (
        <div className="App">
          <DeveloperCard {...developerObj} />
        </div>
      );
    }

This is particularly useful when working with arrays of objects and passing multiple sets of properties to child components.

For example, you might have a list of developers where each object in the array has the same structure but represents a different person.

You will learn more about how to render lists in arrays in future lessons.

## Important Things to Remember About Props

Using props in React makes your components more flexible and reusable, allowing you to build more complex UIs.

However, it is important to note that **props are immutable**, meaning they should not be changed by the component receiving them.

If you need to handle user input and modify data, you should use **state** instead.

You'll learn more about managing state in future lessons.
