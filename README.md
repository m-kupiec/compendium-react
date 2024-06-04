# React Reference

## TOC

### Funamentals

- **Components**

# Fundamentals

## Components

> React components are JavaScript functions that return markup
>
> [React](https://react.dev/learn)

JSX requirements:
- "You have to close tags like `<br />`." ([React](https://react.dev/learn))
- "Your component also can’t return multiple JSX tags. You have to wrap them into a shared parent, like a `<div>...</div>` or an empty `<>...</>` wrapper" ([React](https://react.dev/learn))

```jsx
function MyComponent() {
  return (
    <h1>...</h1>
  );
}

export default function MyApp() {
  return (
    <>
      <MyComponent />
      <p>...</p>
    </>
  )
}
```
