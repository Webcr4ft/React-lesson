# How Do You Update Objects in State?

Updating objects in state in React can be tricky if you're used to changing object property values directly.

React treats state as **immutable**, meaning you should not modify it directly. Instead, you should create a new object with the updated values and pass it to the state setter function.

Let's look at what happens when you modify objects in React state directly and then explore the correct ways to update them.

## 1. Why Direct State Mutation Doesn't Work

Let's say you have an object in your component state that represents a user's profile, and you want the user to update their age.

### Example: Incorrect Approach

    import { useState } from "react";

    function Profile() {
      const [user, setUser] = useState({
        name: "John Doe",
        age: 31,
        city: "LA",
      });

      // Change user age directly
      const handleAgeChange = (e) => {
        user.age = e.target.value;
        console.log(user);
      };

      return (
        <div>
          <h1>User Profile</h1>
          <p>Name: {user.name}</p>
          <p>Age: {user.age}</p>
          <p>City: {user.city}</p>

          <h2>Update User Age</h2>
          <input
            type="number"
            value={user.age}
            onChange={handleAgeChange}
          />
        </div>
      );
    }

    export default Profile;

### Why Is This Incorrect?

The code directly modifies the `user.age` property instead of using the setter function, `setUser`.

Even though `console.log(user)` may show the new age in the console, React will not automatically re-render the component because you did not update the state using its setter function.

**Remember:** Never mutate an object in state directly. Use the setter function to update it.

## 2. Using the Setter Function

You can update the state object using the setter function, `setUser`.

For example:

    const handleAgeChange = (e) => {
      setUser({
        age: e.target.value,
      });
    };

This updates the age, but it also removes the `name` and `city` properties from the state object.

Why?

Because the new object passed to `setUser` contains only one property: `age`.

React does not automatically merge the new object with the previous state object.

To preserve the other properties, you need to copy the existing object before updating it.

## 3. Preserving State With an Updater Function and Spread Syntax

To preserve the other properties, you can use an **updater function** and the **spread syntax**.

The updater function receives the previous state as an argument and returns the next state.

### Example

    const handleAgeChange = (e) => {
      setUser((prevUser) => {
        const updatedUser = {
          ...prevUser,
          age: e.target.value,
        };

        console.log("Previous State:", prevUser);
        console.log("Updated State:", updatedUser);

        return updatedUser;
      });
    };

### Explanation

* `setUser()` updates the state.
* `prevUser` represents the previous state value supplied to the updater function.
* `...prevUser` copies the existing properties into a new object.
* `age: e.target.value` updates the age using the input value.
* `return updatedUser` returns the new object as the next state.

For example, if the previous state is:

    {
      name: "John Doe",
      age: 31,
      city: "LA"
    }

And the user changes the age to `32`, the updated state becomes:

    {
      name: "John Doe",
      age: "32",
      city: "LA"
    }

Notice that the name and city remain unchanged.

**Important:** Input values are usually strings, even when the input type is `number`. If you need the age as a number, you can convert it using `Number(e.target.value)`.

This approach ensures that the existing properties are preserved while the selected property is updated.

## 4. Updating Properties With Individual Event Handlers

You can create separate event handlers to update the name, age, and city individually.

### Updating the Name

    const handleNameChange = (e) => {
      setUser((prevUser) => ({
        ...prevUser,
        name: e.target.value,
      }));
    };

### Updating the City

    const handleCityChange = (e) => {
      setUser((prevUser) => ({
        ...prevUser,
        city: e.target.value,
      }));
    };

### Explanation

Both functions follow the same pattern:

* `prevUser` receives the previous state.
* `...prevUser` copies all existing properties.
* The specified property is updated with the new input value.
* The new object is returned to update the state.

This approach is useful when you want separate event handlers for different input fields.

## 5. Using a Single Change Handler

Instead of creating separate event handlers for each property, you can use one event handler to update all the fields.

### Example

    const handleChange = (e) => {
      const { name, value } = e.target;

      setUser((prevUser) => ({
        ...prevUser,
        [name]: value,
      }));
    };

### Explanation

* `e.target` refers to the input element that triggered the event.
* `name` identifies the property to update.
* `value` contains the new value entered by the user.
* `...prevUser` preserves the other properties.
* `[name]: value` updates the property whose name matches the input's `name` attribute.

For example, if the input has `name="age"`, the expression `[name]: value` updates the `age` property.

If the input has `name="city"`, it updates the `city` property instead.

The square brackets around `name` make it a **computed property name**, allowing JavaScript to use the value of the variable as the object's property name.

## 6. Complete Example: Updating a User Profile

Here is the complete code using a single change handler to update the user's name, age, and city.

    import { useState } from "react";

    function Profile() {
      const [user, setUser] = useState({
        name: "John Doe",
        age: 31,
        city: "LA",
      });

      const handleChange = (e) => {
        const { name, value } = e.target;

        setUser((prevUser) => ({
          ...prevUser,
          [name]: value,
        }));
      };

      return (
        <div>
          <h1>User Profile</h1>

          <p>Name: {user.name}</p>
          <p>Age: {user.age}</p>
          <p>City: {user.city}</p>

          <h2>Update User Age</h2>
          <input
            type="number"
            name="age"
            value={user.age}
            onChange={handleChange}
          />

          <h2>Update User Name</h2>
          <input
            type="text"
            name="name"
            value={user.name}
            onChange={handleChange}
          />

          <h2>Update User City</h2>
          <input
            type="text"
            name="city"
            value={user.city}
            onChange={handleChange}
          />
        </div>
      );
    }

    export default Profile;

### How the Complete Code Works

1. `useState()` creates the `user` state object with three properties: `name`, `age`, and `city`.
2. Each input has a `name` attribute that identifies the property it updates.
3. When the user types into an input, `onChange={handleChange}` calls the event handler.
4. `const { name, value } = e.target` extracts the input's name and current value.
5. `setUser()` creates a new object using the spread syntax and updates the appropriate property.
6. React re-renders the component to display the updated profile.

Because the inputs use `value={user.name}`, `value={user.age}`, and `value={user.city}`, they are controlled inputs. Their displayed values are determined by the React state.

## 7. Quick Summary

| Concept | Meaning |
|---|---|
| State mutation | Directly changing an existing state object or its properties |
| `setUser()` | Updates the user state |
| `prevUser` | The previous state value passed to an updater function |
| `...prevUser` | Copies the existing properties into a new object |
| `e.target` | The input element that triggered the event |
| `e.target.name` | Identifies which property the input updates |
| `e.target.value` | Contains the input's current value |
| `[name]: value` | Updates a property using a computed property name |
| `onChange` | Handles changes to an input's value |

## Conclusion

When updating objects in React state, remember these important rules:

* Never modify state objects directly.
* Use the setter function, such as `setUser()`, to update state.
* Use the spread syntax (`...`) to preserve existing properties.
* Use an updater function when the next state depends on the previous state.
* Use individual event handlers or a single change handler to update object properties.
* Remember that React does not automatically merge new objects with previous state objects.

**The most important pattern to remember is:**

    setUser((prevUser) => ({
      ...prevUser,
      age: newAge,
    }));

This creates a new object, preserves the existing properties, and updates only the `age` property.
