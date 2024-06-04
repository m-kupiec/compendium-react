# React Reference

## TOC

### Funamentals

# Fundamentals

> React components are JavaScript functions that return markup
>
> [React](https://react.dev/learn)

JSX:
- Definition:
  - "The markup syntax . . . is called *JSX*" ([React](https://react.dev/learn))
- Escaping into JavaScript:
  - "JSX lets you put markup into JavaScript. Curly braces let you “escape back” into JavaScript" ([React](https://react.dev/learn))
  - "You can also “escape into JavaScript” from JSX attributes, but you have to use curly braces *instead of quotes*" ([React](https://react.dev/learn))
  - "`style={{}}` is not a special syntax, but a regular `{}` object inside the `style={ }` JSX curly braces. You can use the `style` attribute when your styles depend on JavaScript variables." ([React](https://react.dev/learn))
  - "you can use the conditional `?` operator. Unlike `if`, it works inside JSX . . . you can also use a shorter logical `&&` syntax" ([React](https://react.dev/learn))
- Requirements:
  - "You have to close tags like `<br />`." ([React](https://react.dev/learn))
  - "Your component also can’t return multiple JSX tags. You have to wrap them into a shared parent, like a `<div>...</div>` or an empty `<>...</>` wrapper" ([React](https://react.dev/learn))

> React does not prescribe how you add CSS files. In the simplest case, you’ll add a `<link>` tag to your HTML.
>
> [React](https://react.dev/learn)

```jsx
/* MyComponent.jsx */

const titleSpan = <span style={{
    color: 'blue',
  }}>...</span>;

export default function MyComponent() {
  return (
    <h1>{titleSpan}</h1>
    {Math.random() > 0.5 ? (
      <h2>...1...</h2>
    ) : (
      <h2>...2...</h2>
    )}
  );
}
```

```jsx
/* App.jsx */

import MyComponent from './components/MyComponent.jsx';
import myLogo      from './assets/logo.svg';
import                  './styles/App.css';

export default function App() {
  return (
    <>
      <MyComponent />
      <p className="class1">...</p>
      <img src={myLogo} alt="App">
    </>
  )
}
```

```jsx
/* main.jsx */

import React    from 'react';
import ReactDOM from 'react-dom/client';
import App      from './App.jsx';
import               './styles/index.css';

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
)
```

```html
<!-- index.html -->

<!-- ... -->
  <body>
    <div id="root"></div>

    <script src="/src/main.jsx" type="module"></script>
  </body>
</html>
```
