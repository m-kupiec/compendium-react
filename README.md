# React Reference

## TOC

### Funamentals

### Application Design & Development

- **Step 1: Designing a Component Hierarchy**
- **Step 2: Building a Static Prototype**
- **Step 3: Identifying the State**
- **Step 4: Implementing the State Flowing Down**
- **Step 5: Implementing the State Updates**

# Fundamentals

Components:
- General:
  - "React components are JavaScript functions that return markup" ([React](https://react.dev/learn))
    - "They return JSX markup" ([React](https://react.dev/learn/your-first-component))
  - "Components are used to render, manage, and update the UI elements in your application" ([React](https://react.dev/learn/tutorial-tic-tac-toe))
- Internals:
  - "JSX looks like HTML, but under the hood it is transformed into plain JavaScript objects. You can’t return two objects from a function without wrapping them into an array. This explains why you also can’t return two JSX tags without wrapping them into another tag or a Fragment." ([React](https://react.dev/learn/writing-markup-with-jsx))
- Built-in components:
  - HTML elements:
    - "`className="square"` is a button property or *prop* . . . The DOM `<button>` element’s `onClick` attribute has a special meaning to React because it is a built-in component. For custom components like `Square`, the naming is up to you. You could give any name to the `Square`’s `onSquareClick` prop or `Board`’s `handleClick` function, and the code would work the same." ([React](https://react.dev/learn/tutorial-tic-tac-toe))
    - "`<img />` is written like HTML, but it is actually JavaScript under the hood!" ([React](https://react.dev/learn/your-first-component))
    - "JavaScript has limitations on variable names. For example, their names can’t contain dashes or be reserved words like `class`. Since `class` is a reserved word, in React you write `className` instead, named after the corresponding DOM property" ([React](https://react.dev/learn/writing-markup-with-jsx))
    - "For historical reasons, `aria-*` and `data-*` attributes are written as in HTML with dashes." ([React](https://react.dev/learn/writing-markup-with-jsx))
  - `<Fragment>`:
    - "empty tag is called a *Fragment*. Fragments let you group things without leaving any trace in the browser HTML tree." ([React](https://react.dev/learn/writing-markup-with-jsx))
- Props:
  - "In React, it’s conventional to use `onSomething` names for props which represent events and `handleSomething` for the function definitions which handle those events." ([React](https://react.dev/learn/tutorial-tic-tac-toe))
- Organization:
  - "many websites only use React to add interactivity to existing HTML pages. They have many root components instead of a single one for the entire page." ([React](https://react.dev/learn/your-first-component))
  - "a root component file, named `App.js` . . . Depending on your setup, your root component could be in another file, though. If you use a framework with file-based routing, such as Next.js, your root component will be different for every page." ([React](https://react.dev/learn/importing-and-exporting-components))
  - "Components are regular JavaScript functions, so you can keep multiple components in the same file. This is convenient when components are relatively small or tightly related to each other. If this file gets crowded, you can always move . . . to a separate file." ([React](https://react.dev/learn/your-first-component))

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
    - "It’s strongly recommended that you assign proper keys whenever you build dynamic lists. If you don’t have an appropriate key, you may want to consider restructuring your data so that you do." ([React](https://react.dev/learn/tutorial-tic-tac-toe))
- Requirements:
  - "You have to close tags like `<br />`." ([React](https://react.dev/learn))
  - "Your component also can’t return multiple JSX tags. You have to wrap them into a shared parent, like a `<div>...</div>` or an empty `<>...</>` wrapper" ([React](https://react.dev/learn))

Hooks:
- "Functions starting with `use` are called Hooks." ([React](https://react.dev/learn))
- "You can only call Hooks at the top of your components (or other Hooks). If you want to use `useState` in a condition or a loop, extract a new component and put it there." ([React](https://react.dev/learn))

State:
- Defining state:
  - "you may notice that `xIsNext === true` when `currentMove` is even and `xIsNext === false` when `currentMove` is odd. In other words, if you know the value of `currentMove`, then you can always figure out what `xIsNext` should be. There’s no reason for you to store both of these in state. In fact, always try to avoid redundant state. Simplifying what you store in state reduces bugs and makes your code easier to understand. Change `Game` so that it doesn’t store `xIsNext` as a separate state variable and instead figures it out based on the `currentMove` . . . You no longer need the `xIsNext` state declaration or the calls to `setXIsNext`. Now, there’s no chance for `xIsNext` to get out of sync with `currentMove`, even if you make a mistake while coding the components." ([React](https://react.dev/learn/tutorial-tic-tac-toe))
- Sharing state:
  - "To collect data from multiple children, or to have two child components communicate with each other, declare the shared state in their parent component instead. The parent component can pass that state back down to the children via props. This keeps the child components in sync with each other and with their parent." ([React](https://react.dev/learn/tutorial-tic-tac-toe))
  - "Lifting state into a parent component is common when React components are refactored." ([React](https://react.dev/learn/tutorial-tic-tac-toe))
- Data immutability and rendering:
  - "Calling the `setSquares` function lets React know the state of the component has changed. This will trigger a re-render of the components that use the `squares` state (`Board`) as well as its child components (the `Square` components that make up the board)." ([React](https://react.dev/learn/tutorial-tic-tac-toe))
  - "Avoiding direct data mutation lets you keep previous versions of the data intact, and reuse them later." ([React](https://react.dev/learn/tutorial-tic-tac-toe))
  - "By default, all child components re-render automatically when the state of a parent component changes. . . . Immutability makes it very cheap for components to compare whether their data has changed or not. You can learn more about how React chooses when to re-render a component in the `memo` API reference." ([React](https://react.dev/learn/tutorial-tic-tac-toe))

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

# Application Design & Development

## Step 1: Designing a Component Hierarchy

> Imagine that you already have a JSON API and a mockup from a designer. The JSON API returns some data that looks like this:
>
> ```json
> [
>   { category: "Fruits", price: "$1", stocked: true, name: "Apple" },
>   { category: "Fruits", price: "$1", stocked: true, name: "Dragonfruit" },
>   { category: "Fruits", price: "$2", stocked: false, name: "Passionfruit" },
>   { category: "Vegetables", price: "$2", stocked: true, name: "Spinach" },
>   { category: "Vegetables", price: "$4", stocked: false, name: "Pumpkin" },
>   { category: "Vegetables", price: "$1", stocked: true, name: "Peas" }
> ]
> ```
>
> Start by drawing boxes around every component and subcomponent in the mockup and naming them. If you work with a designer, they may have already named these components in their design tool.
>
> . . . a component should ideally only do one thing. If it ends up growing, it should be decomposed into smaller subcomponents.
>
> . . . Separate your UI into components, where each component matches one piece of your data model.
>
> ![Image](/assets/s_thinking-in-react_ui_outline.png)
>
> Now that you’ve identified the components in the mockup, arrange them into a hierarchy. . . .
> - FilterableProductTable
>   - SearchBar
>   - ProductTable
>     - ProductCategoryRow
>     - ProductRow
>
> [React](https://react.dev/learn/thinking-in-react)

## Step 2: Building a Static Prototype

> The most straightforward approach is to build a version that renders the UI from your data model without adding any interactivity… yet! It’s often easier to build the static version first and add interactivity later. Building a static version requires a lot of typing and no thinking, but adding interactivity requires a lot of thinking and not a lot of typing.
>
> . . . you’ll want to build components that reuse other components and pass data using props
>
> . . . don’t use state at all to build this static version. State is reserved only for interactivity, that is, data that changes over time.
>
> . . . You can either build “top down” by starting with building the components higher up in the hierarchy . . . or “bottom up” by working from components lower down . . . In simpler examples, it’s usually easier to go top-down, and on larger projects, it’s easier to go bottom-up.
>
> . . . After building your components, you’ll have a library of reusable components that render your data model. . . . The component at the top of the hierarchy . . . will take your data model as a prop. This is called *one-way data flow* because the data flows down from the top-level component to the ones at the bottom of the tree.
>
> [React](https://react.dev/learn/thinking-in-react)

## Step 3: Identifying the State

> To make the UI interactive, you need to let users change your underlying data model. You will use *state* for this.
>
> Think of state as the minimal set of changing data that your app needs to remember. The most important principle for structuring state is to keep it DRY (Don’t Repeat Yourself). Figure out the absolute minimal representation of the state your application needs and compute everything else on-demand. . . .
>
> - Does it **remain unchanged** over time? If so, it isn’t state.
> - Is it **passed in from a parent** via props? If so, it isn’t state.
> - **Can you compute it** based on existing state or props in your component? If so, it *definitely* isn’t state!
>
> [React](https://react.dev/learn/thinking-in-react)

> 1. The original list of products is **passed in as props, so it’s not state**.
> 2. The search text seems to be state since it changes over time and can’t be computed from anything.
> 3. The value of the checkbox seems to be state since it changes over time and can’t be computed from anything.
> 4. The filtered list of products **isn’t state because it can be computed** by taking the original list of products and filtering it according to the search text and value of the checkbox.
>
> [React](https://react.dev/learn/thinking-in-react)

> Props and state are different, but they work together. A parent component will often keep some information in state (so that it can change it), and *pass it down* to child components as their props.
>
> [React](https://react.dev/learn/thinking-in-react)

## Step 4: Implementing the State Flowing Down

> you need to identify which component is responsible for changing this state, or owns the state. Remember: React uses one-way data flow, passing data down the component hierarchy from parent to child component. . . .
>
> For each piece of state in your application:
> 1. Identify *every* component that renders something based on that state.
> 2. Find their closest common parent component—a component above them all in the hierarchy.
> 3. Decide where the state should live:
>    1. Often, you can put the state directly into their common parent.
>    2. You can also put the state into some component above their common parent.
>    3. If you can’t find a component where it makes sense to own the state, create a new component solely for holding the state and add it somewhere in the hierarchy above the common parent component.
>
> Add state to the component with the `useState()` Hook. . . . Add . . . state variables at the top . . . and specify their initial state . . . Then, pass . . . as props
>
> [React](https://react.dev/learn/thinking-in-react)

## Step 5: Implementing the State Updates

> to change the state according to user input, you will need to support data flowing the other way: the form components deep in the hierarchy need to update the state . . .
>
> The state is owned by `FilterableProductTable`, so only it can call `setFilterText` and `setInStockOnly`. To let `SearchBar` update the `FilterableProductTable`’s state, you need to pass these functions down to `SearchBar`
>
> [React](https://react.dev/learn/thinking-in-react)
