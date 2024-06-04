# React Reference

## TOC

### Funamentals

# Fundamentals

Components:
- "React components are JavaScript functions that return markup" ([React](https://react.dev/learn))
- "Components are used to render, manage, and update the UI elements in your application" ([React](https://react.dev/learn/tutorial-tic-tac-toe))
- "`className="square"` is a button property or *prop* . . . The DOM `<button>` element’s `onClick` attribute has a special meaning to React because it is a built-in component. For custom components like `Square`, the naming is up to you. You could give any name to the `Square`’s `onSquareClick` prop or `Board`’s `handleClick` function, and the code would work the same." ([React](https://react.dev/learn/tutorial-tic-tac-toe))

Styling:

> React does not prescribe how you add CSS files. In the simplest case, you’ll add a `<link>` tag to your HTML.
>
> [React](https://react.dev/learn)

JSX:
- Definition:
  - "The markup syntax . . . is called *JSX*" ([React](https://react.dev/learn))
- Escaping into JavaScript:
  - "JSX lets you put markup into JavaScript. Curly braces let you “escape back” into JavaScript" ([React](https://react.dev/learn))
  - HTML attributes:
    - "You can also “escape into JavaScript” from JSX attributes, but you have to use curly braces *instead of quotes*" ([React](https://react.dev/learn))
    - "`style={{}}` is not a special syntax, but a regular `{}` object inside the `style={ }` JSX curly braces. You can use the `style` attribute when your styles depend on JavaScript variables." ([React](https://react.dev/learn))
  - Control flow:
    - "you can use the conditional `?` operator. Unlike `if`, it works inside JSX . . . you can also use a shorter logical `&&` syntax" ([React](https://react.dev/learn))
    - "You will rely on JavaScript features like `for` loop and the array `map()` function to render lists of components." ([React](https://react.dev/learn))
    - "`<li>` has a `key` attribute. For each item in a list, you should pass a string or a number that uniquely identifies that item among its siblings." ([React](https://react.dev/learn))
- Requirements:
  - "You have to close tags like `<br />`." ([React](https://react.dev/learn))
  - "Your component also can’t return multiple JSX tags. You have to wrap them into a shared parent, like a `<div>...</div>` or an empty `<>...</>` wrapper" ([React](https://react.dev/learn))

Hooks:
- "Functions starting with `use` are called Hooks." ([React](https://react.dev/learn))
- "You can only call Hooks at the top of your components (or other Hooks). If you want to use `useState` in a condition or a loop, extract a new component and put it there." ([React](https://react.dev/learn))

State and props:
- Sharing state:
  - "To collect data from multiple children, or to have two child components communicate with each other, declare the shared state in their parent component instead. The parent component can pass that state back down to the children via props. This keeps the child components in sync with each other and with their parent." ([React](https://react.dev/learn/tutorial-tic-tac-toe))
  - "Lifting state into a parent component is common when React components are refactored." ([React](https://react.dev/learn/tutorial-tic-tac-toe))
- Rendering changes:
  - "Calling the `setSquares` function lets React know the state of the component has changed. This will trigger a re-render of the components that use the `squares` state (`Board`) as well as its child components (the `Square` components that make up the board)." ([React](https://react.dev/learn/tutorial-tic-tac-toe))

```jsx
/* MyComponent.jsx */

const titleSpan = <span style={{
    color: 'blue',
  }}>...</span>;
const arr = [1, 2, 3];

export default function MyComponent({ counter, handler }) {
  return (
    <>
      <h1>{titleSpan}</h1>

      <button onClick={handler}>{counter}</button>

      {Math.random() > 0.5 ? (
        <h2>...1...</h2>
      ) : (
        <h2>...2...</h2>
      )}

      <ul>
        {arr.map(item =>
          <li key={item}>
            {item}
          </li>
        )}
      </ul>
    </>
  );
}
```

```jsx
/* App.jsx */

import { useState } from 'react';
import MyComponent  from './components/MyComponent.jsx';
import myLogo       from './assets/logo.svg';
import                   './styles/App.css';

export default function App() {
  const [ count, setCount ] = useState(0);

  function handleClick() {
    setCount(count + 1);
    // OR:
    // setCount(c => c + 1);
  }

  return (
    <>
      <MyComponent counter={count} handler={handleClick} />
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
