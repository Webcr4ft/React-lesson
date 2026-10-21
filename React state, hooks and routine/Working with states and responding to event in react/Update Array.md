# How Do You Update Arrays in State?

In React, updating arrays in state is quite straightforward, but it can be easy to make mistakes, especially if you're coming from vanilla JavaScript, where you can modify arrays directly.

In React, state is treated as **immutable**, meaning you should not modify the existing array directly. Instead, create a new array and use the state setter function to update it.

React relies on a new array reference to detect changes and update the user interface when necessary.

Let's look at how you can update arrays held in React state.

## 1. Why Direct Array Mutation Doesn't Work

One of the most common mistakes when updating arrays in React state is directly modifying the array using methods such as:

* `push()` — Adds an item to the end of an array.
* `pop()` — Removes the last item from an array.
* `splice()` — Adds, removes, or replaces items in an array by modifying the original array.

These methods mutate the original array, which you should avoid when working with React state.

### Example: Incorrect Approach Using push()

    import { useState } from "react";

    function ItemsList() {
      const [items, setItems] = useState([
        { id: 0, name: "Item 1" },
        { id: 1, name: "Item 2" },
        { id: 2, name: "Item 3" },
      ]);

      const addItem = () => {
        const newItem = {
          id: items.length + 1,
          name: `Item ${items.length + 1}`,
        };

        items.push(newItem); // Modifies the original array
        setItems(items); // Same array reference
      };

      return (
        <div>
          <button onClick={addItem}>Add Item</button>

          <ul>
            {items.map((item) => (
              <li key={item.id}>{item.name}</li>
            ))}
          </ul>
        </div>
      );
    }

    export default ItemsList;

### Why Is This Incorrect?

The problem is these two lines:

    items.push(newItem);
    setItems(items);

* `items.push(newItem)` modifies the original array.
* `setItems(items)` passes the same array reference back to React.

Because the array reference has not changed, React may skip re-rendering the component.

As a result, the new item might not appear in the user interface immediately.

**Remember:** Avoid modifying arrays directly when they are stored in React state.

## 2. Removing Items With pop()

You might also be tempted to remove an item using `pop()`.

### Example: Incorrect Approach

    const removeItem = () => {
      items.pop(); // Modifies the original array
      setItems(items); // Same array reference
    };

This has the same problem as using `push()`.

* `pop()` modifies the original array.
* `setItems(items)` passes the same array reference to React.
* React may skip re-rendering because the state reference has not changed.

Instead of modifying the existing array, create a new array that contains the items you want to keep.

## 3. Adding Items Using the Spread Operator

To add an item to an array in React state, you can use the **spread operator (`...`)**.

The spread operator allows you to copy the existing array's elements into a new array.

### Example: Correct Approach

    const addItem = () => {
      const newItem = {
        id: items.length + 1,
        name: `Item ${items.length + 1}`,
      };

      setItems((prevItems) => [...prevItems, newItem]);
    };

### Explanation

* `prevItems` represents the previous state array.
* `...prevItems` copies the existing items into a new array.
* `newItem` is added to the end of the new array.
* `setItems()` updates the state with the new array.

For example, suppose the original array is:

    [
      { id: 0, name: "Item 1" },
      { id: 1, name: "Item 2" },
      { id: 2, name: "Item 3" }
    ]

After adding a new item, the array becomes:

    [
      { id: 0, name: "Item 1" },
      { id: 1, name: "Item 2" },
      { id: 2, name: "Item 3" },
      { id: 3, name: "Item 4" }
    ]

A new array is created, and the existing items are preserved.

**Important:** Using `items.length + 1` to generate IDs can cause duplicate IDs if items are removed and new ones are added. For a simple demonstration, it is fine, but real applications should use a reliable unique ID generator.

## 4. Removing Items Using filter()

To remove an item from an array without modifying the original array, you can use the `filter()` method.

The `filter()` method creates a new array containing only the elements that satisfy a condition.

### Example

    const removeItem = (id) => {
      setItems((prevItems) =>
        prevItems.filter((item) => item.id !== id)
      );
    };

### Explanation

* `removeItem(id)` receives the ID of the item to remove.
* `prevItems` represents the previous state array.
* `filter()` checks each item in the array.
* `item.id !== id` keeps every item whose ID does not match the supplied ID.
* `setItems()` updates the state with the filtered array.

For example, suppose the array contains:

    [
      { id: 0, name: "Item 1" },
      { id: 1, name: "Item 2" },
      { id: 2, name: "Item 3" }
    ]

If you call:

    removeItem(1);

The resulting array becomes:

    [
      { id: 0, name: "Item 1" },
      { id: 2, name: "Item 3" }
    ]

The item with ID `1` is removed, while the other items remain.

## 5. Displaying Array Items With map()

When working with arrays in React, you will often use the `map()` method to display their contents.

The `map()` method creates a new array by applying a function to each element.

For example:

    {items.map((item) => (
      <li key={item.id}>{item.name}</li>
    ))}

### Explanation

* `items.map()` loops through the items and creates a list element for each one.
* `item` represents the current item being processed.
* `item.name` displays the item's name.
* `key={item.id}` provides React with a stable key to identify each item in the list.

The `key` should be unique among the sibling elements in the list.

## 6. Complete Example: Adding and Removing Items

Here is the complete code for an interactive list that allows you to add and remove items.

    import { useState } from "react";

    function ItemsList() {
      const [items, setItems] = useState([
        { id: 0, name: "Item 1" },
        { id: 1, name: "Item 2" },
        { id: 2, name: "Item 3" },
      ]);

      const addItem = () => {
        const newItem = {
          id: items.length + 1,
          name: `Item ${items.length + 1}`,
        };

        setItems((prevItems) => [...prevItems, newItem]);
      };

      const removeItem = (id) => {
        setItems((prevItems) =>
          prevItems.filter((item) => item.id !== id)
        );
      };

      return (
        <div>
          <button onClick={addItem}>Add Item</button>

          <ul>
            {items.map((item) => (
              <li key={item.id}>
                {item.name}{" "}
                <button onClick={() => removeItem(item.id)}>
                  Remove
                </button>
              </li>
            ))}
          </ul>
        </div>
      );
    }

    export default ItemsList;

### How the Complete Code Works

1. `useState()` creates an array containing three initial items.
2. `addItem()` creates a new item.
3. `[...prevItems, newItem]` creates a new array containing the existing items and the new item.
4. `removeItem(id)` removes the item with the matching ID using `filter()`.
5. `items.map()` displays each item in the user interface.
6. `key={item.id}` helps React identify each list item.
7. `onClick={addItem}` calls the function when the Add Item button is clicked.
8. `onClick={() => removeItem(item.id)}` calls the removal function with the selected item's ID.

Whenever the state array changes, React can re-render the component to reflect the updated list.

## 7. Common Array Methods for React State

| Operation | Recommended Approach | Purpose |
|---|---|---|
| Add an item | `[...prevItems, newItem]` | Creates a new array with the item at the end |
| Add an item at the beginning | `[newItem, ...prevItems]` | Creates a new array with the item at the beginning |
| Remove an item | `filter()` | Creates a new array without the selected item |
| Update an item | `map()` | Creates a new array with the selected item updated |
| Find an item | `find()` | Returns the first item that matches a condition |
| Replace an item | `map()` | Creates a new array with the selected item replaced |

### Example: Updating an Existing Item

You can use `map()` to update an item without modifying the original array.

    const updateItem = (id, newName) => {
      setItems((prevItems) =>
        prevItems.map((item) =>
          item.id === id
            ? { ...item, name: newName }
            : item
        )
      );
    };

Here, `map()` creates a new array. If an item's ID matches the supplied ID, the code creates a new object with the updated name. Otherwise, the original item object is returned unchanged.

## Conclusion

Updating arrays in React state is straightforward once you understand the importance of immutability.

The most important things to remember are:

* Do not directly mutate arrays stored in React state.
* Avoid methods like `push()`, `pop()`, and `splice()` when modifying state arrays.
* Use the spread operator (`...`) to create a new array when adding items.
* Use `filter()` to remove items.
* Use `map()` to update or display items.
* Use the setter function, such as `setItems()`, to update the state.
* Use an updater function when the next state depends on the previous state.
* Give list items stable, unique keys using `key`.

**The most important patterns to remember are:**

### Adding an Item

    setItems((prevItems) => [...prevItems, newItem]);

### Removing an Item

    setItems((prevItems) =>
      prevItems.filter((item) => item.id !== id)
    );

### Updating an Item

    setItems((prevItems) =>
      prevItems.map((item) =>
        item.id === id
          ? { ...item, name: newName }
          : item
      )
    );

These patterns help you update arrays safely while allowing React to keep your user interface in sync with your state.
