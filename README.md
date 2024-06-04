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

> React does not prescribe how you add CSS files. In the simplest case, you’ll add a `<link>` tag to your HTML.
>
> [React](https://react.dev/learn)

```jsx
/* MyComponent.jsx */

export default function MyComponent() {
  return (
    <h1>...</h1>
  );
}
```

```jsx
/* MyApp.jsx */

import MyComponent from './components/MyComponent.jsx';
import myLogo      from './assets/logo.svg';
import                  './styles/MyApp.css';

export default function MyApp() {
  return (
    <>
      <MyComponent />
      <p className="class1">...</p>
      <img src={myLogo} alt="MyApp">
    </>
  )
}
```
