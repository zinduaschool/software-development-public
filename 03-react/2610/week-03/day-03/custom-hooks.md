# Custom Hooks

[← Day 3 overview](README.md)

- Custom Hooks in React let us extract component logic into reusable functions.
- A custom Hook is a JavaScript function that can use other Hooks provided by React. It lets us organise logic into separate, reusable modules.

## Why use custom Hooks?

- **Encapsulation:** keeps component logic separate and reusable.
- **Reusability:** use the same hook across different components.
- **Code organisation:** reduces clutter in components.
- **Readability:** complex logic is abstracted into a single function.

## Tips for building custom Hooks

- Start by identifying repetitive logic across your components.
- Extract this logic into a function whose name starts with `use`, like `useFormInput` or `useFetch`.
- Use React's built-in Hooks inside your custom Hook as needed.
- Return anything that will be useful for the component using the Hook.
- Each custom Hook should be responsible for a single piece of functionality.

## Example 1: useCounter

We'll build a `useCounter` hook that holds the logic for incrementing and decrementing a counter.

```jsx
// useCounter.js
import { useState } from 'react'

function useCounter(initialValue = 0) {
  const [count, setCount] = useState(initialValue) // state to keep track of count

  const increment = () => setCount((prevState) => prevState + 1)
  const decrement = () => setCount((prevState) => prevState - 1)

  return [count, increment, decrement]
}

export default useCounter
```

To use the `useCounter` Hook, call it inside a function component:

```jsx
// CounterComponent.jsx
import useCounter from './useCounter'

function CounterComponent() {
  const [count, increment, decrement] = useCounter()

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
      <button onClick={decrement}>Decrement</button>
    </div>
  )
}

export default CounterComponent
```

## Example 2: useFetch

If we need to fetch data from an API in several components, we can create a `useFetch` Hook.

```jsx
// useFetch.js
import { useState, useEffect } from 'react'

const useFetch = (url) => {
  const [data, setData] = useState(null)
  const [loading, setLoading] = useState(true)
  const [error, setError] = useState(null)

  useEffect(() => {
    fetch(url)
      .then((response) => {
        if (!response.ok) throw new Error('Failed to fetch data')
        return response.json()
      })
      .then((result) => setData(result))
      .catch((err) => setError(err.message))
      .finally(() => setLoading(false))
  }, [url])

  return { data, loading, error }
}

export default useFetch
```

Now any component can fetch data in one line:

```jsx
// UsersList.jsx
import useFetch from './useFetch'

const UsersList = () => {
  const { data, loading, error } = useFetch('https://jsonplaceholder.typicode.com/users')

  if (loading) return <p>Loading...</p>
  if (error) return <p>Error: {error}</p>

  return (
    <div>
      <h1>Users List</h1>
      <ul>
        {data.map((user) => (
          <li key={user.id}>{user.name}</li>
        ))}
      </ul>
    </div>
  )
}

export default UsersList
```

## Example 3: useToggle

This hook toggles a boolean value, which is useful for modals, dark mode, show/hide and so on.

```jsx
// useToggle.js
import { useState } from 'react'

const useToggle = (initialValue = false) => {
  const [value, setValue] = useState(initialValue)
  const toggle = () => setValue((prev) => !prev)
  return [value, toggle]
}

export default useToggle
```

Using it in a component. The hook returns `[value, toggle]`, and we can name them whatever fits the component:

```jsx
// ToggleComponent.jsx
import useToggle from './useToggle'

const ToggleComponent = () => {
  const [isVisible, toggleVisibility] = useToggle()

  return (
    <div>
      <button onClick={toggleVisibility}>
        {isVisible ? 'Hide' : 'Show'} Text
      </button>
      {isVisible && <p>This is some text!</p>}
    </div>
  )
}

export default ToggleComponent
```
