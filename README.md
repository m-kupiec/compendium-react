# React Reference

## TOC

### Introduction

- **Example**
- **Project Structure**
  - Component Organization
  - The Module Dependency Tree
  - Styling
- **Serving UI**
  - Overview
  - The Render Tree

### Components

- **General**
- **Data**
  - Props
    - General
    - Immutability
    - Forwarding
    - The `children` Prop
  - State
  - Local Variables
- **Hooks**
  - Introduction
  - Usage
  - Patterns
- **Markup**
  - General
  - Internals
  - Escaping Into JavaScript
    - General
    - Element Attributes
    - Control Flow
      - Conditionals
      - Loops
        - General
        - The `key` Attribute
  - Returned Value
- **Purity**
  - General
  - `StrictMode`
- **Built-In Components**
  - HTML Elements
  - `<Fragment>`

### State

- **Structure**
  - Overview
  - Grouping Related Data
  - Avoiding Contradiction
  - Avoiding Redundancy
  - Avoiding Duplication
  - Avoiding Dependence on Props
  - Avoiding Deep State Nesting
- **Sharing**
  - Bounds
  - Lifting Up
- **Changes**
  - Preservation/Destruction
  - Update
    - Scheduling
    - Immutability
      - Role
      - Best Practices
        - Objects
        - Arrays
    - Setter Function
      - Role
      - Operation
      - Best Practices
  - Reset
    - Different Position
    - Different `key`
- **Management**
  - Reducers
    - Definition
    - Usage
    - Best Practices
      - Actions
      - Reducer
    - Example
    - Use Cases
  - Context
    - Definition
    - Operation
    - Example
    - Use Cases
    - Alternatives
  - Reducers With Context
    - General
    - Example

### Refs

- **Introduction**
- **Use Cases**
- **Operation**
  - General
  - DOM Nodes
- **Usage**
  - General
  - DOM Nodes
    - General
    - Referencing Lists
    - Flushing the DOM
  - Components

### Events

- **Architecture**
- **Naming Convention**
- **Propagation**
- **_Passing Handlers_ Pattern**

### Side Effects

- **Distinction**
  - By Cause
    - Event
    - Rendering
  - By Value Reactivity
- **Effects**
  - Operation
  - Lifecycle
  - Basic Usage
  - Dependencies
    - Impact
    - Linting
    - Identification
      - Reactive Values
      - Mutable Values
    - Patterns
      - Moving Non-Reactive Values Outside Component
      - Updating State Without Using Reactive Values
      - Avoiding Objects/Functions as Dependencies
  - Cleanup Function
    - Usage
    - Operation
    - Strict Mode
      - Operation
      - Working with Preventive APIs
    - Use Cases
      - Positive
      - Negative
  - Best Practices
    - Separating Independent Processeses
    - Extracting into Effect Events
    - Extracting into Custom Hooks
  - Use Cases
    - General
    - Data Fetching
      - Introduction
      - Challenges
      - Usage
- **External Store Subscriptions**
  - Introduction
  - Usage
  - Best Practices

### Hooks

- **`useMemo`**
- **Custom**

### Application Design & Development

- **Step 1: Designing a Component Hierarchy**
- **Step 2: Building a Static Prototype**
- **Step 3: Identifying the State**
- **Step 4: Implementing the State Flowing Down**
- **Step 5: Implementing the State Updates**

### Component Design & Development

- **Overview**
- **Step 1: Identifying Visual States**
- **Step 2: Identifying State Triggers**
- **Step 3: Drafting State**
- **Step 4: Refactoring State**
- **Step 5: Connecting State and Triggers**

### TypeScript Intergration

- **Setup**
- **Type Checking**
  - Type Assertion
  - Intrinsic vs. Value-Based Elements
  - Result Type
- **Usage**
  - General
  - `useState`
  - `useReducer`
  - `useContext`
  - `useMemo`
  - `useCallback`
- **Built-In Types**
  - DOM Events
  - Children
  - `style` Property

# Introduction

## Example

```html
<!-- index.html -->

<!-- ... -->
  <body>
    <div id="root"></div>

    <script src="/src/main.jsx" type="module"></script>
  </body>
</html>
```

```jsx
/* main.jsx */

import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App.jsx";
import "./styles/index.css";

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
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
/* MyComponent.jsx */

const titleSpan = (
  <span
    style={{
      color: "blue",
    }}
  >
    ...
  </span>
);
const arr = [1, 2, 3];

export default function MyComponent({ counter, handler }) {
  return (
    <>
      <h1>{titleSpan}</h1>

      <button onClick={handler}>{counter}</button>

      {Math.random() > 0.5 ? <h2>...1...</h2> : <h2>...2...</h2>}

      <ul>
        {arr.map((item) => (
          <li key={item}>{item}</li>
        ))}
      </ul>
    </>
  );
}
```

## Project Structure

### Component Organization

"many websites only use React to add interactivity to existing HTML pages. They have many root components instead of a single one for the entire page." ([React](https://react.dev/learn/your-first-component))

"a root component file, named `App.js` . . . Depending on your setup, your root component could be in another file, though. If you use a framework with file-based routing, such as Next.js, your root component will be different for every page." ([React](https://react.dev/learn/importing-and-exporting-components))

"Components are regular JavaScript functions, so you can keep multiple components in the same file. This is convenient when components are relatively small or tightly related to each other. If this file gets crowded, you can always move . . . to a separate file." ([React](https://react.dev/learn/your-first-component))

### The Module Dependency Tree

> As we break up our components and logic into separate files, we create JS modules where we may export components, functions, or constants. . . .
>
> ![Image](/assets/module_dependency_tree.webp)
>
> . . . The root node of the tree is the root module, also known as the entrypoint file. It often is the module that contains the root component.
>
> . . . Non-component modules, like `inspirations.js`, are also represented in this tree.
>
> . . . `Copyright.js` appears under `App.js` but in the render tree, `Copyright`, the component, appears as a child of `InspirationGenerator`. This is because `InspirationGenerator` accepts JSX as children props, so it renders `Copyright` as a child component but does not import the module.
>
> [React](https://react.dev/learn/understanding-your-ui-as-a-tree)

"bundlers will use the dependency tree to determine what modules should be included." ([React](https://react.dev/learn/understanding-your-ui-as-a-tree))

"Large bundle sizes can delay the time for your UI to get drawn. Getting a sense of your app’s dependency tree may help with debugging these issues. . . . Dependency trees are useful for debugging large bundle sizes that slow time to paint" ([React](https://react.dev/learn/understanding-your-ui-as-a-tree))

### Styling

"React does not prescribe how you add CSS files. In the simplest case, you’ll add a `<link>` tag to your HTML." ([React](https://react.dev/learn))

## Serving UI

### Overview

> process of requesting and serving UI has three steps:
>
> 1. Triggering a render . . .
> 2. Rendering the component . . .
> 3. Committing to the DOM
>
> [React](https://react.dev/learn/render-and-commit)

> After you trigger a render, React calls your components to figure out what to display on screen. “Rendering” is React calling your components.
>
> - On initial render, React will call the root component.
> - For subsequent renders, React will call the function component whose state update triggered the render.
>
> This process is recursive: if the updated component returns some other component, React will render that component next, and if that component also returns something, it will render that component next, and so on. The process will continue until there are no more nested components and React knows exactly what should be displayed on screen.
>
> [React](https://react.dev/learn/render-and-commit)

> After rendering (calling) your components, React will modify the DOM.
>
> - For the initial render, React will use the `appendChild()` DOM API to put all the DOM nodes it has created on screen.
> - For re-renders, React will apply the minimal necessary operations (calculated while rendering!) to make the DOM match the latest rendering output.
>
> [React](https://react.dev/learn/render-and-commit)

"React only changes the DOM nodes if there’s a difference between renders. . . . you can add some text into the `<input>`, updating its `value`, but the text doesn’t disappear when the component re-renders" ([React](https://react.dev/learn/render-and-commit))

"After rendering is done and React updated the DOM, the browser will repaint the screen. Although this process is known as “browser rendering”, we’ll refer to it as “painting” to avoid confusion throughout the docs." ([React](https://react.dev/learn/render-and-commit))

### The Render Tree

> There are two reasons for a component to render:
>
> 1. It’s the component’s **initial render**.
> 2. The component’s (or one of its ancestors’) **state has been updated**.
>
> [React](https://react.dev/learn/render-and-commit)

"As we nest components, we have the concept of parent and child components, where each parent component may itself be a child of another component. When we render a React app, we can model this relationship in a tree, known as the render tree." ([React](https://react.dev/learn/understanding-your-ui-as-a-tree))

> The root node in a React render tree is the root component of the app. In this case, the root component is `App` and it is the first component React renders. . . .
>
> ![Image](/assets/conditional_render_tree.webp)
>
> With conditional rendering, across different renders, the render tree may render different components.
>
> [React](https://react.dev/learn/understanding-your-ui-as-a-tree)

"the render tree is only composed of React components. React, as a UI framework, is platform agnostic. On react.dev, we showcase examples that render to the web, which uses HTML markup as its UI primitives. But a React app could just as likely render to a mobile or desktop platform, which may use different UI primitives like UIView or FrameworkElement. These platform UI primitives are not a part of React. React render trees can provide insight to our React app regardless of what platform your app renders to." ([React](https://react.dev/learn/understanding-your-ui-as-a-tree))

"Although render trees may differ across render passes, these trees are generally helpful for identifying what the `top-level` and `leaf components` are in a React app. Top-level components are the components nearest to the root component and affect the rendering performance of all the components beneath them and often contain the most complexity. Leaf components are near the bottom of the tree and have no child components and are often frequently re-rendered. . . . Identifying [top-level components] . . . is useful for understanding and debugging rendering performance." ([React](https://react.dev/learn/understanding-your-ui-as-a-tree))

# Components

## General

"React components are JavaScript functions that return markup" ([React](https://react.dev/learn))

- "They return JSX markup" ([React](https://react.dev/learn/your-first-component))

"Components are used to render, manage, and update the UI elements in your application" ([React](https://react.dev/learn/tutorial-tic-tac-toe))

## Data

### Props

#### General

"React component functions accept a single argument, a `props` object . . . Usually you don’t need the whole `props` object itself, so you destructure it into individual props." ([React](https://react.dev/learn/passing-props-to-a-component))

- > ```jsx
  > function Avatar(props) {
  >   let person = props.person;
  >   let size = props.size;
  >   // ...
  > }
  > ```
  >
  > ```jsx
  > function Avatar({ person, size }) {
  >   // ...
  > }
  > ```
  >
  > [React](https://react.dev/learn/passing-props-to-a-component)

"If you want to give a prop a default value to fall back on when no value is specified, you can do it with the destructuring by putting `=` and the default value right after the parameter" ([React](https://react.dev/learn/passing-props-to-a-component))

#### Immutability

"props are immutable . . . When a component needs to change its props (for example, in response to a user interaction or new data), it will have to “ask” its parent component to pass it different props—a new object! Its old props will then be cast aside, and eventually the JavaScript engine will reclaim the memory taken by them." ([React](https://react.dev/learn/passing-props-to-a-component))

#### Forwarding

> Forwarding props with the JSX spread syntax . . .
>
> ```jsx
> function Profile({ person, size, isSepia, thickBorder }) {
>   return (
>     <div className="card">
>       <Avatar
>         person={person}
>         size={size}
>         isSepia={isSepia}
>         thickBorder={thickBorder}
>       />
>     </div>
>   );
> }
> ```
>
> Some components forward all of their props to their children, like how this `Profile` does with `Avatar`. Because they don’t use any of their props directly, it can make sense to use a more concise “spread” syntax:
>
> ```jsx
> function Profile(props) {
>   return (
>     <div className="card">
>       <Avatar {...props} />
>     </div>
>   );
> }
> ```
>
> Use spread syntax with restraint. If you’re using it in every other component, something is wrong. Often, it indicates that you should split your components and pass children as JSX.
>
> [React](https://react.dev/learn/passing-props-to-a-component)

#### The `children` Prop

"When you nest content inside a JSX tag, the parent component will receive that content in a prop called `children`. . . . You can think of a component with a `children` prop as having a “hole” that can be “filled in” by its parent components with arbitrary JSX. You will often use the `children` prop for visual wrappers: panels, grids, etc." ([React](https://react.dev/learn/passing-props-to-a-component))

- ```jsx
  import MyComponent from "./components/MyComponent.jsx";
  import NestedComponent from "./components/NestedComponent.jsx";

  export default function App() {
    return (
      <>
        <MyComponent>
          <NestedComponent />
        </MyComponent>
      </>
    );
  }
  ```

- ```jsx
  export default function MyComponent({ children }) {
    return (
      <>
        <h1>MyComponent</h1>
        <section>{children}</section>
      </>
    );
  }
  ```

### State

> To update a component with new data, two things need to happen:
>
> 1. **Retain** the data between renders.
> 2. **Trigger** React to render the component with new data (re-rendering).
>
> The `useState` Hook provides those two things:
>
> 1. A **state variable** to retain the data between renders.
> 2. A **state setter function** to update the variable and trigger React to render the component again.
>
> [React](https://react.dev/learn/state-a-components-memory)

### Local Variables

"Local variables don’t persist between renders." ([React](https://react.dev/learn/state-a-components-memory))

"Changes to local variables won’t trigger renders." ([React](https://react.dev/learn/state-a-components-memory))

## Hooks

### Introduction

"React applications are built from components. Components are built from Hooks, whether built-in or custom." ([React](https://react.dev/learn/reusing-logic-with-custom-hooks))

"Functions starting with `use` are called Hooks." ([React](https://react.dev/learn))

"_Hooks_ are special functions that are only available while React is rendering . . . They let you “hook into” different React features." ([React](https://react.dev/learn/state-a-components-memory))

### Usage

"Hooks—functions starting with `use`—can only be called at the top level of your components or your own Hooks. You can’t call Hooks inside conditions, loops, or other nested functions. Hooks are functions, but it’s helpful to think of them as unconditional declarations about your component’s needs. You “use” React features at the top of your component similar to how you “import” modules at the top of your file." ([React](https://react.dev/learn/state-a-components-memory))

### Patterns

"If you want to use `useState` in a condition or a loop, extract a new component and put it there." ([React](https://react.dev/learn))

## Markup

### General

"The markup syntax . . . is called _JSX_" ([React](https://react.dev/learn))

"You have to close tags like `<br />`." ([React](https://react.dev/learn))

### Internals

"`<img />` is written like HTML, but it is actually JavaScript under the hood!" ([React](https://react.dev/learn/your-first-component))

"JSX looks like HTML, but under the hood it is transformed into plain JavaScript objects. You can’t return two objects from a function without wrapping them into an array. This explains why you also can’t return two JSX tags without wrapping them into another tag or a Fragment." ([React](https://react.dev/learn/writing-markup-with-jsx))

"JSX elements aren’t “instances” because they don’t hold any internal state and aren’t real DOM nodes." ([React](https://react.dev/learn/conditional-rendering))

### Escaping Into JavaScript

#### General

"JSX lets you put markup into JavaScript. Curly braces let you “escape back” into JavaScript" ([React](https://react.dev/learn))

"JavaScript inside the JSX `{` and `}` executes right away" ([React](https://react.dev/learn/responding-to-events))

#### Element Attributes

"You can also “escape into JavaScript” from JSX attributes, but you have to use curly braces _instead of quotes_" ([React](https://react.dev/learn))

"`style={{}}` is not a special syntax, but a regular `{}` object inside the `style={ }` JSX curly braces. You can use the `style` attribute when your styles depend on JavaScript variables." ([React](https://react.dev/learn))

#### Control Flow

##### Conditionals

"you can use the conditional `?` operator. Unlike `if`, it works inside JSX . . . you can also use a shorter logical `&&` syntax" ([React](https://react.dev/learn))

"If the first operand of the `&&` operator after the automatic `Boolean` conversion becomes `false` but itself is not strictly `false` nor `null`/`undefined`, it will be rendered by React - examples include `0`, `NaN`, or `""`" (see [React](https://react.dev/learn/conditional-rendering))

##### Loops

###### General

"You will rely on JavaScript features like `for` loop and the array `map()` function to render lists of components." ([React](https://react.dev/learn))

###### The `key` Attribute

"JSX elements directly inside a `map()` call always need keys!" ([React](https://react.dev/learn/rendering-lists))

"`<li>` has a `key` attribute. For each item in a list, you should pass a string or a number that uniquely identifies that item among its siblings." ([React](https://react.dev/learn))

Key generation rules:

- "**Keys must be unique among siblings.** However, it’s okay to use the same keys for JSX nodes in different arrays." ([React](https://react.dev/learn/rendering-lists))
- **Keys must not change** or that defeats their purpose! Don’t generate them while rendering. . . . do not generate keys on the fly, e.g. with `key={Math.random()}`. This will cause keys to never match up between renders, leading to all your components and DOM being recreated every time. Not only is this slow, but it will also lose any user input inside the list items. Instead, use a stable ID based on the data." ([React](https://react.dev/learn/rendering-lists))
- "It’s strongly recommended that you assign proper keys whenever you build dynamic lists. If you don’t have an appropriate key, you may want to consider restructuring your data so that you do." ([React](https://react.dev/learn/tutorial-tic-tac-toe))
- "If your data is coming from a database, you can use the database keys/IDs, which are unique by nature. . . . If your data is generated and persisted locally (e.g. notes in a note-taking app), use an incrementing counter, `crypto.randomUUID()` or a package like `uuid` when creating items." ([React](https://react.dev/learn/rendering-lists))
- "You might be tempted to use an item’s index in the array as its key. In fact, that’s what React will use if you don’t specify a `key` at all. But the order in which you render items will change over time if an item is inserted, deleted, or if the array gets reordered. Index as a key often leads to subtle and confusing bugs." ([React](https://react.dev/learn/rendering-lists))

### Returned Value

"Your component also can’t return multiple JSX tags. You have to wrap them into a shared parent, like a `<div>...</div>` or an empty `<>...</>` wrapper" ([React](https://react.dev/learn))

"In some situations, you won’t want to render anything at all. . . . A component must return something. In this case, you can return `null` . . . In practice, returning `null` from a component isn’t common . . . More often, you would conditionally include or exclude the component in the parent component’s JSX." ([React](https://react.dev/learn/conditional-rendering))

## Purity

### General

> a pure function is a function with the following characteristics:
>
> - **It minds its own business.** It does not change any objects or variables that existed before it was called.
> - **Same inputs, same output.** Given the same inputs, a pure function should always return the same result.
>
> [React](https://react.dev/learn/keeping-components-pure)

"Components should only _return_ their JSX, and not _change_ any objects or variables that existed before rendering—that would make them impure!" ([React](https://react.dev/learn/keeping-components-pure))

"Pure functions don’t mutate variables outside of the function’s scope or objects that were created before the call—that makes them impure! However, it’s completely fine to change variables and objects that you’ve _just_ created while rendering. . . . it’s fine because you’ve created them during the same render . . . No code outside of [it] . . . will ever know that this happened. This is called “local mutation”—it’s like your component’s little secret." ([React](https://react.dev/learn/keeping-components-pure))

"React assumes that every component you write is a pure function. This means that React components you write must always return the same JSX given the same inputs" ([React](https://react.dev/learn/keeping-components-pure))

### `StrictMode`

"You should never change preexisting variables or objects while your component is rendering. React offers a “Strict Mode” in which it calls each component’s function twice during development. By calling the component functions twice, Strict Mode helps find components that break these rules. . . . Pure functions only calculate, so calling them twice won’t change anything" ([React](https://react.dev/learn/keeping-components-pure))

"Strict Mode has no effect in production, so it won’t slow down the app for your users." ([React](https://react.dev/learn/keeping-components-pure))

"To opt into Strict Mode, you can wrap your root component into `<React.StrictMode>`" ([React](https://react.dev/learn/keeping-components-pure))

## Built-In Components

### HTML Elements

"`className="square"` is a button property or _prop_ . . . The DOM `<button>` element’s `onClick` attribute has a special meaning to React because it is a built-in component. For custom components like `Square`, the naming is up to you. You could give any name to the `Square`’s `onSquareClick` prop or `Board`’s `handleClick` function, and the code would work the same." ([React](https://react.dev/learn/tutorial-tic-tac-toe))

"JavaScript has limitations on variable names. For example, their names can’t contain dashes or be reserved words like `class`. Since `class` is a reserved word, in React you write `className` instead, named after the corresponding DOM property" ([React](https://react.dev/learn/writing-markup-with-jsx))

"For historical reasons, `aria-*` and `data-*` attributes are written as in HTML with dashes." ([React](https://react.dev/learn/writing-markup-with-jsx))

### `<Fragment>`

"empty tag is called a _Fragment_. Fragments let you group things without leaving any trace in the browser HTML tree." ([React](https://react.dev/learn/writing-markup-with-jsx))

"What do you do when each item needs to render not one, but several DOM nodes? The short `<>...</>` Fragment syntax won’t let you pass a key, so you need to either group them into a single `<div>`, or use the slightly longer and more explicit `<Fragment>` syntax . . . Fragments disappear from the DOM, so this will produce a flat list" ([React](https://react.dev/learn/rendering-lists))

# State

## Structure

### Overview

> When you write a component that holds some state, you’ll have to make choices about how many state variables to use and what the shape of their data should be. . . . there are a few principles that can guide you to make better choices:
>
> 1. **Group related state.** If you always update two or more state variables at the same time, consider merging them into a single state variable.
> 2. **Avoid contradictions in state.** When the state is structured in a way that several pieces of state may contradict and “disagree” with each other, you leave room for mistakes. Try to avoid this.
> 3. **Avoid redundant state.** If you can calculate some information from the component’s props or its existing state variables during rendering, you should not put that information into that component’s state.
> 4. **Avoid duplication in state.** When the same data is duplicated between multiple state variables, or within nested objects, it is difficult to keep them in sync. Reduce duplication when you can.
> 5. **Avoid deeply nested state.** Deeply hierarchical state is not very convenient to update. When possible, prefer to structure state in a flat way.
>
> The goal behind these principles is _to make state easy to update without introducing mistakes_.
>
> [React](https://react.dev/learn/choosing-the-state-structure)

### Grouping Related Data

"It is a good idea to have multiple state variables if their state is unrelated . . . But if you find that you often change two state variables together, it might be easier to combine them into one. For example, if you have a form with many fields, it’s more convenient to have a single state variable that holds an object than state variable per field." ([React](https://react.dev/learn/state-a-components-memory))

"if some two state variables always change together, it might be a good idea to unify them into a single state variable." ([React](https://react.dev/learn/choosing-the-state-structure))

"Another case where you’ll group data into an object or an array is when you don’t know how many pieces of state you’ll need. For example, it’s helpful when you have a form where the user can add custom fields." ([React](https://react.dev/learn/choosing-the-state-structure))

### Avoiding Contradiction

"For example, if you forget to call `setIsSent` and `setIsSending` together, you may end up in a situation where both `isSending` and `isSent` are `true` at the same time. The more complex your component is, the harder it is to understand what happened. Since `isSending` and `isSent` should never be `true` at the same time, it is better to replace them with one `status` state variable" ([React](https://react.dev/learn/choosing-the-state-structure))

### Avoiding Redundancy

"If you can calculate some information from the component’s props or its existing state variables during rendering, you should not put that information into that component’s state." ([React](https://react.dev/learn/choosing-the-state-structure))

"you may notice that `xIsNext === true` when `currentMove` is even and `xIsNext === false` when `currentMove` is odd. In other words, if you know the value of `currentMove`, then you can always figure out what `xIsNext` should be. There’s no reason for you to store both of these in state. In fact, always try to avoid redundant state. Simplifying what you store in state reduces bugs and makes your code easier to understand. Change `Game` so that it doesn’t store `xIsNext` as a separate state variable and instead figures it out based on the `currentMove` . . . You no longer need the `xIsNext` state declaration or the calls to `setXIsNext`. Now, there’s no chance for `xIsNext` to get out of sync with `currentMove`, even if you make a mistake while coding the components." ([React](https://react.dev/learn/tutorial-tic-tac-toe))

### Avoiding Duplication

"instead of a `selectedItem` object (which creates a duplication with objects inside `items`), you hold the `selectedId` in state, and then get the `selectedItem` by searching the `items` array for an item with that ID . . . You didn’t need to hold _the selected item_ in state, because only the _selected ID_ is essential. The rest could be calculated during render." ([React](https://react.dev/learn/choosing-the-state-structure))

### Avoiding Dependence on Props

"Don’t mirror props in state . . . if the parent component passes a different value . . . state variable would not be updated! The state is only initialized during the first render. . . . ”Mirroring” props into state only makes sense when you want to ignore all updates for a specific prop. By convention, start the prop name with `initial` or `default` to clarify that its new values are ignored" ([React](https://react.dev/learn/choosing-the-state-structure))

"adjusting state based on props or other state makes your data flow more difficult to understand and debug. Always check whether you can reset all state with a key or calculate everything during rendering instead. For example, instead of storing (and resetting) the selected _item_, you can store the selected _item ID_" ([React](https://react.dev/learn/you-might-not-need-an-effect))

### Avoiding Deep State Nesting

"Updating nested state involves making copies of objects all the way up from the part that changed." ([React](https://react.dev/learn/choosing-the-state-structure))

"If the state is too nested to update easily, consider making it “flat” . . . (also known as “normalized”)" ([React](https://react.dev/learn/choosing-the-state-structure))

"Sometimes, you can also reduce state nesting by moving some of the nested state into the child components. This works well for ephemeral UI state that doesn’t need to be stored, like whether an item is hovered." ([React](https://react.dev/learn/choosing-the-state-structure))

## Sharing

### Bounds

"State is local to a component instance . . . if you render the same component twice, each copy will have completely isolated state" ([React](https://react.dev/learn/state-a-components-memory))

"state is fully private to the component declaring it. The parent component can’t change it." ([React](https://react.dev/learn/state-a-components-memory))

### Lifting Up

"To collect data from multiple children, or to have two child components communicate with each other, declare the shared state in their parent component instead. The parent component can pass that state back down to the children via props. This keeps the child components in sync with each other and with their parent." ([React](https://react.dev/learn/tutorial-tic-tac-toe))

"Lifting state into a parent component is common when React components are refactored." ([React](https://react.dev/learn/tutorial-tic-tac-toe))

"It is common to call a component with some local state “uncontrolled”. . . . In contrast, you might say a component is “controlled” when the important information in it is driven by props rather than its own local state. This lets the parent component fully specify its behavior. . . . Uncontrolled components are easier to use within their parents because they require less configuration. But they’re less flexible when you want to coordinate them together. Controlled components are maximally flexible, but they require the parent components to fully configure them with props. In practice, “controlled” and “uncontrolled” aren’t strict technical terms—each component usually has some mix of both local state and props. However, this is a useful way to talk about how components are designed and what capabilities they offer." ([React](https://react.dev/learn/sharing-state-between-components))

## Changes

### Preservation/Destruction

"when React removes a component, it destroys its state. . . . React preserves a component’s state for as long as it’s being rendered at its position in the UI tree." ([React](https://react.dev/learn/preserving-and-resetting-state))

> In this example, there are two different `<Counter />` tags:
>
> ```jsx
> /* ... */
> return (
>   <div>
>     {isFancy ? (
>       <Counter isFancy={true} />
>     ) : (
>       <Counter isFancy={false} />
>     )}
> /* ... */
> ```
>
> When you tick or clear the checkbox, the counter state does not get reset. Whether `isFancy` is `true` or `false`, you always have a `<Counter />` as the first child of the `div` returned from the root `App` component . . . It’s the same component at the same position, so from React’s perspective, it’s the same counter.
>
> Remember that it’s the position in the UI tree—not in the JSX markup—that matters to React! . . . React doesn’t know where you place the conditions in your function. All it “sees” is the tree you return. In both cases, the `App` component returns a `<div>` with `<Counter />` as a first child. To React, these two counters have the same “address”: the first child of the first child of the root. This is how React matches them up between the previous and next renders, regardless of how you structure your logic.
>
> [React](https://react.dev/learn/preserving-and-resetting-state)

"Different components at the same position reset state . . . you switch between different component types at the same position. Initially, the first child of the `<div>` contained a `Counter`. But when you swapped in a `p`, React removed the `Counter` from the UI tree and destroyed its state." ([React](https://react.dev/learn/preserving-and-resetting-state))

"Also, when you render a different component in the same position, it resets the state of its entire subtree." ([React](https://react.dev/learn/preserving-and-resetting-state))

"As a rule of thumb, if you want to preserve the state between re-renders, the structure of your tree needs to “match up” from one render to another. If the structure is different, the state gets destroyed because React destroys state when it removes a component from the tree." ([React](https://react.dev/learn/preserving-and-resetting-state))

> you should not nest component function definitions.
>
> ```jsx
> import { useState } from "react";
>
> export default function MyComponent() {
>   const [counter, setCounter] = useState(0);
>
>   function MyTextField() {
>     const [text, setText] = useState("");
>
>     return <input value={text} onChange={(e) => setText(e.target.value)} />;
>   }
>
>   return (
>     <>
>       <MyTextField />
>       <button
>         onClick={() => {
>           setCounter(counter + 1);
>         }}
>       >
>         Clicked {counter} times
>       </button>
>     </>
>   );
> }
> ```
>
> Every time you click the button, the input state disappears! This is because a _different_ `MyTextField` function is created for every render of `MyComponent`. You’re rendering a _different_ component in the same position, so React resets all state below. This leads to bugs and performance problems. To avoid this problem, always declare component functions at the top level, and don’t nest their definitions.
>
> [React](https://react.dev/learn/preserving-and-resetting-state)

> There are a few ways to keep the state “alive” for a component that’s no longer visible:
>
> - You could render _all_ chats instead of just the current one, but hide all the others with CSS. The chats would not get removed from the tree, so their local state would be preserved. This solution works great for simple UIs. But it can get very slow if the hidden trees are large and contain a lot of DOM nodes.
> - You could lift the state up and hold the pending message for each recipient in the parent component. This way, when the child components get removed, it doesn’t matter, because it’s the parent that keeps the important information. This is the most common solution.
> - You might also use a different source in addition to React state. For example, you probably want a message draft to persist even if the user accidentally closes the page. To implement this, you could have the `Chat` component initialize its state by reading from the `localStorage`, and save the drafts there too.
>
> [React](https://react.dev/learn/preserving-and-resetting-state)

### Update

#### Scheduling

"Setting state only changes it for the next render. . . . The state stored in React may have changed by the time the `alert` runs, but it was scheduled using a snapshot of the state at the time the user interacted with it! A state variable’s value never changes within a render, even if its event handler’s code is asynchronous. . . . the value of `number` continues to be `0` even after `setNumber(number + 5)` was called . . . React keeps the state values “fixed” within one render’s event handlers. . . . Event handlers created in the past have the state values from the render in which they were created." ([React](https://react.dev/learn/state-as-a-snapshot))

"React waits until all code in the event handlers has run before processing your state updates. . . . This behavior, also known as batching, makes your React app run much faster. . . . React does not batch across _multiple_ intentional events like clicks—each click is handled separately. Rest assured that React only does batching when it’s generally safe to do." ([React](https://react.dev/learn/queueing-a-series-of-state-updates))

#### Immutability

##### Role

"state behaves more like a snapshot. Setting it does not change the state variable you already have, but instead triggers a re-render." ([React](https://react.dev/learn/state-as-a-snapshot))

"Avoiding direct data mutation lets you keep previous versions of the data intact, and reuse them later." ([React](https://react.dev/learn/tutorial-tic-tac-toe))

"By default, all child components re-render automatically when the state of a parent component changes. . . . Immutability makes it very cheap for components to compare whether their data has changed or not. You can learn more about how React chooses when to re-render a component in the `memo` API reference." ([React](https://react.dev/learn/tutorial-tic-tac-toe))

"When you store objects in state, mutating them will not trigger renders and will change the state in previous render “snapshots”." ([React](https://react.dev/learn/updating-objects-in-state))

##### Best Practices

###### Objects

"you shouldn’t change objects that you hold in the React state directly. Instead, when you want to update an object, you need to create a new one (or make a copy of an existing one), and then set the state to use that copy. . . . Mutation is only a problem when you change _existing_ objects that are already in state. Mutating an object you’ve just created is okay because _no other code references it yet_. Changing it isn’t going to accidentally impact something that depends on it. This is called a “local mutation”. You can even do local mutation while rendering. Very convenient and completely okay!" ([React](https://react.dev/learn/updating-objects-in-state))

"even if you copy an array, you can’t mutate existing [object] items inside of it directly. This is because copying is shallow—the new array will contain the same items as the original one. So if you modify an object inside the copied array, you are mutating the existing state. . . . When updating nested state, you need to create copies from the point where you want to update, and all the way up to the top level." ([React](https://react.dev/learn/updating-arrays-in-state))

> Updating a nested object . . .
>
> ```jsx
> setPerson({
>   ...person, // Copy other fields
>   artwork: {
>     // but replace the artwork
>     ...person.artwork, // with the same one
>     city: "New Delhi", // but in New Delhi!
>   },
> });
> ```
>
> [React](https://react.dev/learn/updating-objects-in-state)

"In general, you should only mutate objects that you have just created." ([React](https://react.dev/learn/updating-arrays-in-state))

###### Arrays

"Arrays are mutable in JavaScript, but you should treat them as immutable when you store them in state. Just like with objects, when you want to update an array stored in state, you need to create a new one (or make a copy of an existing one), and then set state to use the new array. . . . In JavaScript, arrays are just another kind of object." ([React](https://react.dev/learn/updating-arrays-in-state))

> you should treat arrays in React state as read-only. This means that you shouldn’t reassign items inside an array like `arr[0] = 'bird'`, and you also shouldn’t use methods that mutate the array, such as `push()` and `pop()`. Instead, every time you want to update an array, you’ll want to pass a new array to your state setting function. . . .
>
> |           | avoid (mutates the array)  | prefer (returns a new array) |
> | --------- | -------------------------- | ---------------------------- |
> | adding    | `push`, `unshift`          | `concat`, `[...arr]` . . .   |
> | removing  | `pop`, `shift`, `splice`   | `filter`, `slice` . . .      |
> | replacing | `splice`, `arr[i] =` . . . | `map` . . .                  |
> | sorting   | `reverse`, `sort`          | copy the array first . . .   |
>
> [React](https://react.dev/learn/updating-arrays-in-state)

"to remove an item from an array is to _filter it out_. In other words, you will produce a new array that will not contain that item. To do this, use the `filter` method" ([React](https://react.dev/learn/updating-arrays-in-state))

"To replace an item, create a new array with `map`. Inside your `map` call, you will receive the item index as the second argument. Use it to decide whether to return the original item (the first argument) or something else" ([React](https://react.dev/learn/updating-arrays-in-state))

> to insert an item at a particular position that’s neither at the beginning nor at the end. To do this, you can use the `...` array spread syntax together with the `slice()` method. . . .
>
> ```jsx
> const nextArtists = [
>   // Items before the insertion point:
>   ...artists.slice(0, insertAt),
>   // New item:
>   { id: nextId++, name: name },
>   // Items after the insertion point:
>   ...artists.slice(insertAt),
> ];
> ```
>
> [React](https://react.dev/learn/updating-arrays-in-state)

#### Setter Function

##### Role

"Calling the `setSquares` function lets React know the state of the component has changed. This will trigger a re-render of the components that use the `squares` state (`Board`) as well as its child components (the `Square` components that make up the board)." ([React](https://react.dev/learn/tutorial-tic-tac-toe))

"without using the state setting function, React has no idea that object has changed. So React does not do anything in response." ([React](https://react.dev/learn/updating-objects-in-state))

##### Operation

> It is an uncommon use case, but if you would like to update the same state variable multiple times before the next render, instead of passing the next state value like `setNumber(number + 1)`, you can pass a function that calculates the next state based on the previous one in the queue, like `setNumber(n => n + 1)`. . . . `n => n + 1` is called an updater function. When you pass it to a state setter:
>
> 1. React queues this function to be processed after all the other code in the event handler has run.
> 2. During the next render, React goes through the queue and gives you the final updated state.
>
> . . . React takes the return value of your previous updater function and passes it to the next updater
>
> [React](https://react.dev/learn/queueing-a-series-of-state-updates)

`setNumber(number + 5); setNumber(n => n + 1);` (for `number === 0` initially) will result in `number === 6` in the next render (see [React](https://react.dev/learn/queueing-a-series-of-state-updates))

`setNumber(number + 5); setNumber(n => n + 1); setNumber(number + 4);` (for `number === 0` initially) will result in `number === 4` in the next render (see [React](https://react.dev/learn/queueing-a-series-of-state-updates))

"`setState(5)` actually works like `setState(n => 5)`, but `n` is unused!" ([React](https://react.dev/learn/queueing-a-series-of-state-updates))

##### Best Practices

"Updater functions run during rendering, so **updater functions must be pure** and only _return_ the result. Don’t try to set state from inside of them or run other side effects. In Strict Mode, React will run each updater function twice (but discard the second result) to help you find mistakes." ([React](https://react.dev/learn/queueing-a-series-of-state-updates))

> It’s common to name the updater function argument by the first letters of the corresponding state variable:
>
> ```jsx
> setEnabled((e) => !e);
> setLastName((ln) => ln.reverse());
> setFriendCount((fc) => fc * 2);
> ```
>
> If you prefer more verbose code, another common convention is to repeat the full state variable name, like `setEnabled(enabled => !enabled)`, or to use a prefix like `setEnabled(prevEnabled => !prevEnabled)`.
>
> [React](https://react.dev/learn/queueing-a-series-of-state-updates)

### Reset

#### Different Position

> By default, React preserves state of a component while it stays at the same position.
>
> ```jsx
> /* ... */
> {
>   isPlayerA ? <Counter person="Taylor" /> : <Counter person="Sarah" />;
> }
> /* ... */
> ```
>
> Currently, when you change the player, the score is preserved. The two `Counter`s appear in the same position, so React sees them as _the same_ `Counter` whose `person` prop has changed. . . . If you want these two `Counter`s to be independent, you can render them in two different positions:
>
> ```jsx
> /* ... */
> {
>   isPlayerA && <Counter person="Taylor" />;
> }
> {
>   !isPlayerA && <Counter person="Sarah" />;
> }
> /* ... */
> ```
>
> Each Counter’s state gets destroyed each time it’s removed from the DOM. . . . This solution is convenient when you only have a few independent components rendered in the same place.
>
> [React](https://react.dev/learn/preserving-and-resetting-state)

#### Different `key`

> There is also another, more generic, way to reset a component’s state. . . . You can use keys to make React distinguish between any components. By default, React uses order within the parent (“first counter”, “second counter”) to discern between components. But keys let you tell React that this is not just a _first_ counter, or a _second_ counter, but a specific counter . . . In this example, the two `<Counter />`s don’t share state even though they appear in the same place in JSX:
>
> ```jsx
> {
>   isPlayerA ? (
>     <Counter key="Taylor" person="Taylor" />
>   ) : (
>     <Counter key="Sarah" person="Sarah" />
>   );
> }
> ```
>
> [React](https://react.dev/learn/preserving-and-resetting-state)

"keys are not globally unique. They only specify the position _within_ the parent." ([React](https://react.dev/learn/preserving-and-resetting-state))

"Resetting state with a key is particularly useful when dealing with forms." ([React](https://react.dev/learn/preserving-and-resetting-state))

## Management

### Reducers

#### Definition

"named after the `reduce()` operation that you can perform on arrays. . . . The function you pass to `reduce` is known as a “reducer”. It takes the _result so far_ and the _current item_, then it returns the _next result_. React reducers are an example of the same idea: they take the _state so far_ and the _action_, and return the _next state_. In this way, they accumulate actions over time into state." ([React](https://react.dev/learn/extracting-state-logic-into-a-reducer))

"Managing state with reducers is slightly different from directly setting state. Instead of telling React “what to do” by setting state, you specify “what the user just did” by dispatching “actions” from your event handlers. (The state update logic will live elsewhere!)" ([React](https://react.dev/learn/extracting-state-logic-into-a-reducer))

#### Usage

> To convert from `useState` to `useReducer`:
>
> 1. Dispatch actions from event handlers.
> 2. Write a reducer function that returns the next state for a given state and action.
> 3. Replace `useState` with `useReducer`.
>
> [React](https://react.dev/learn/extracting-state-logic-into-a-reducer)

#### Best Practices

##### Actions

"it should contain the minimal information about _what happened_." ([React](https://react.dev/learn/extracting-state-logic-into-a-reducer))

"An action object can have any shape. By convention, it is common to give it a string `type` that describes what happened, and pass any additional information in other fields. The `type` is specific to a component, so in this example either `'added'` or `'added_task'` would be fine. Choose a name that says what happened!" ([React](https://react.dev/learn/extracting-state-logic-into-a-reducer))

"**Each action describes a single user interaction, even if that leads to multiple changes in the data.** For example, if a user presses “Reset” on a form with five fields managed by a reducer, it makes more sense to dispatch one reset_form action rather than five separate set_field actions." ([React](https://react.dev/learn/extracting-state-logic-into-a-reducer))

##### Reducer

"Because the reducer function takes state . . . as an argument, you can declare it outside of your component. This decreases the indentation level and can make your code easier to read." ([React](https://react.dev/learn/extracting-state-logic-into-a-reducer))

"it’s a convention to use `switch` statements inside reducers. . . . We recommend wrapping each case block into the `{` and `}` curly braces so that variables declared inside of different `case`s don’t clash with each other." ([React](https://react.dev/learn/extracting-state-logic-into-a-reducer))

"**Reducers must be pure.** Similar to state updater functions, reducers run during rendering! (Actions are queued until the next render.) This means that reducers must be pure—same inputs always result in the same output. They should not send requests, schedule timeouts, or perform any side effects (operations that impact things outside the component). They should update objects and arrays without mutations." ([React](https://react.dev/learn/extracting-state-logic-into-a-reducer))

#### Example

Taken from [React](https://react.dev/learn/extracting-state-logic-into-a-reducer):

```jsx
import { useReducer } from "react";
import AddTask from "./AddTask.js";
import TaskList from "./TaskList.js";

export default function TaskApp() {
  const [tasks, dispatch] = useReducer(tasksReducer, initialTasks);

  function handleAddTask(text) {
    dispatch({
      type: "added",
      id: nextId++,
      text: text,
    });
  }

  function handleChangeTask(task) {
    dispatch({
      type: "changed",
      task: task,
    });
  }

  function handleDeleteTask(taskId) {
    dispatch({
      type: "deleted",
      id: taskId,
    });
  }

  return (
    <>
      <h1>Prague itinerary</h1>
      <AddTask onAddTask={handleAddTask} />
      <TaskList
        tasks={tasks}
        onChangeTask={handleChangeTask}
        onDeleteTask={handleDeleteTask}
      />
    </>
  );
}

function tasksReducer(tasks, action) {
  switch (action.type) {
    case "added": {
      return [
        ...tasks,
        {
          id: action.id,
          text: action.text,
          done: false,
        },
      ];
    }
    case "changed": {
      return tasks.map((t) => {
        if (t.id === action.task.id) {
          return action.task;
        } else {
          return t;
        }
      });
    }
    case "deleted": {
      return tasks.filter((t) => t.id !== action.id);
    }
    default: {
      throw Error("Unknown action: " + action.type);
    }
  }
}

let nextId = 3;
const initialTasks = [
  { id: 0, text: "Visit Kafka Museum", done: true },
  { id: 1, text: "Watch a puppet show", done: false },
  { id: 2, text: "Lennon Wall pic", done: false },
];
```

#### Use Cases

General:

- "It’s a matter of preference. You can always convert between `useState` and `useReducer` back and forth: they are equivalent!" ([React](https://react.dev/learn/extracting-state-logic-into-a-reducer))
- "We recommend using a reducer if you often encounter bugs due to incorrect state updates in some component, and want to introduce more structure to its code." ([React](https://react.dev/learn/extracting-state-logic-into-a-reducer))
- "You don’t have to use reducers for everything: feel free to mix and match! You can even `useState` and `useReducer` in the same component." ([React](https://react.dev/learn/extracting-state-logic-into-a-reducer))

Code maintainability:

- "`useReducer` can help cut down on the code if many event handlers modify state in a similar way." ([React](https://react.dev/learn/extracting-state-logic-into-a-reducer))
- "when the state updates . . . get more complex, they can bloat your component’s code and make it difficult to scan. In this case, `useReducer` lets you cleanly separate the _how_ of update logic from the _what happened_ of event handlers." ([React](https://react.dev/learn/extracting-state-logic-into-a-reducer))
- "Component logic can be easier to read when you separate concerns like this. Now the event handlers only specify _what happened_ by dispatching actions, and the reducer function determines _how the state updates_ in response to them." ([React](https://react.dev/learn/extracting-state-logic-into-a-reducer))

Debugging:

- "When you have a bug with `useState`, it can be difficult to tell _where_ the state was set incorrectly, and _why_. With `useReducer`, you can add a console log into your reducer to see every state update, and _why_ it happened (due to which `action`). If each `action` is correct, you’ll know that the mistake is in the reducer logic itself." ([React](https://react.dev/learn/extracting-state-logic-into-a-reducer))

Testing:

- "A reducer is a pure function that doesn’t depend on your component. This means that you can export and test it separately in isolation. While generally it’s best to test components in a more realistic environment, for complex state update logic it can be useful to assert that your reducer returns a particular state for a particular initial state and action." ([React](https://react.dev/learn/extracting-state-logic-into-a-reducer))

### Context

#### Definition

"Context lets the parent component make some information available to any component in the tree below it—no matter how deep—without passing it explicitly through props." ([React](https://react.dev/learn/passing-data-deeply-with-context))

"Context lets you write components that “adapt to their surroundings” and display themselves differently depending on _where_ (or, in other words, _in which context_) they are being rendered." ([React](https://react.dev/learn/passing-data-deeply-with-context))

#### Operation

"How context works might remind you of CSS property inheritance. In CSS, you can specify `color: blue` for a `<div>`, and any DOM node inside of it, no matter how deep, will inherit that color unless some other DOM node in the middle overrides it with `color: green`. Similarly, in React, the only way to override some context coming from above is to wrap children into a context provider with a different value." ([React](https://react.dev/learn/passing-data-deeply-with-context))

"different React contexts don’t override each other. Each context that you make with `createContext()` is completely separate from other ones . . . One component may use or provide many different contexts without a problem." ([React](https://react.dev/learn/passing-data-deeply-with-context))

"Context is not limited to static values. If you pass a different value on the next render, React will update all the components reading it below! This is why context is often used in combination with state." ([React](https://react.dev/learn/passing-data-deeply-with-context))

#### Example

Slightly modified version of the code from [React](https://react.dev/learn/passing-data-deeply-with-context):

```jsx
/* App.js */

import Heading from "./Heading.js";
import Section from "./Section.js";

export default function Page() {
  return (
    <Section>
      <Heading>Heading #1</Heading>

      <Section>
        <Heading>Heading #2</Heading>

        <Section>
          <Heading>Heading #3</Heading>
        </Section>
      </Section>
    </Section>
  );
}
```

```jsx
/* LevelContext.js */

import { createContext } from "react";

export const LevelContext = createContext(0);
```

```jsx
/* Section.js */

import { useContext } from "react";
import { LevelContext } from "./LevelContext.js";

export default function Section({ children }) {
  const level = useContext(LevelContext);

  return (
    <section>
      <LevelContext.Provider value={level + 1}>
        {children}
      </LevelContext.Provider>
    </section>
  );
}
```

```jsx
/* Heading.js */

import { useContext } from "react";
import { LevelContext } from "./LevelContext.js";

export default function Heading({ children }) {
  const level = useContext(LevelContext);

  switch (level) {
    case 0:
      throw Error("Heading must be inside a Section!");
    case 1:
      return <h1>{children}</h1>;
    case 2:
      return <h2>{children}</h2>;
    case 3:
      return <h3>{children}</h3>;
    default:
      throw Error("Unknown level");
  }
}
```

#### Use Cases

General:

- "The nearest common ancestor could be far removed from the components that need data, and lifting state up that high can lead to a situation called “prop drilling”." ([React](https://react.dev/learn/passing-data-deeply-with-context))
- "In general, if some information is needed by distant components in different parts of the tree, it’s a good indication that context will help you." ([React](https://react.dev/learn/passing-data-deeply-with-context))

"**Theming:** If your app lets the user change its appearance (e.g. dark mode), you can put a context provider at the top of your app, and use that context in components that need to adjust their visual look." ([React](https://react.dev/learn/passing-data-deeply-with-context))

"**Current account:** Many components might need to know the currently logged in user. Putting it in context makes it convenient to read it anywhere in the tree. Some apps also let you operate multiple accounts at the same time (e.g. to leave a comment as a different user). In those cases, it can be convenient to wrap a part of the UI into a nested provider with a different current account value." ([React](https://react.dev/learn/passing-data-deeply-with-context))

"**Routing:** Most routing solutions use context internally to hold the current route. This is how every link “knows” whether it’s active or not. If you build your own router, you might want to do it too." ([React](https://react.dev/learn/passing-data-deeply-with-context))

"**Managing state:** As your app grows, you might end up with a lot of state closer to the top of your app. Many distant components below may want to change it. It is common to use a reducer together with context to manage complex state and pass it down to distant components without too much hassle." ([React](https://react.dev/learn/passing-data-deeply-with-context))

#### Alternatives

> Just because you need to pass some props several levels deep doesn’t mean you should put that information into context. Here’s a few alternatives you should consider before using context:
>
> 1. **Start by passing props.** If your components are not trivial, it’s not unusual to pass a dozen props down through a dozen components. It may feel like a slog, but it makes it very clear which components use which data! The person maintaining your code will be glad you’ve made the data flow explicit with props.
> 2. **Extract components and pass JSX as children to them.** If you pass some data through many layers of intermediate components that don’t use that data (and only pass it further down), this often means that you forgot to extract some components along the way. For example, maybe you pass data props like `posts` to visual components that don’t use them directly, like `<Layout posts={posts} />`. Instead, make `Layout` take `children` as a prop, and render `<Layout><Posts posts={posts} /></Layout>`. This reduces the number of layers between the component specifying the data and the one that needs it.
>
> If neither of these approaches works well for you, consider context.
>
> [React](https://react.dev/learn/passing-data-deeply-with-context)

### Reducers With Context

#### General

"You can combine reducer with context to let any component read and update state above it . . . to manage state of a complex screen." ([React](https://react.dev/learn/scaling-up-with-reducer-and-context))

"You can have many context-reducer pairs like this in your app." ([React](https://react.dev/learn/scaling-up-with-reducer-and-context))

> To provide state and the dispatch function to components below:
>
> 1. Create two contexts (for state and for dispatch functions).
> 2. Provide both contexts from the component that uses the reducer.
> 3. Use either context from components that need to read them.
>
> [React](https://react.dev/learn/scaling-up-with-reducer-and-context)

> You can further declutter the components by moving all wiring into one file.
>
> - You can export a component like `TasksProvider` that provides context.
> - You can also export custom Hooks like `useTasks` and `useTasksDispatch` to read it.
>
> [React](https://react.dev/learn/scaling-up-with-reducer-and-context)

#### Example

Slightly modified version of the code from [React](https://react.dev/learn/scaling-up-with-reducer-and-context):

```jsx
/* App.js */

import AddTask from "./AddTask.js";
import TaskList from "./TaskList.js";
import { TasksProvider } from "./TasksContext.js";

export default function TaskApp() {
  return (
    <TasksProvider>
      <AddTask />
      <TaskList />
    </TasksProvider>
  );
}
```

```jsx
/* AddTask.js */

/* ... */
import { useTasksDispatch } from "./TasksContext.js";

export default function AddTask() {
  /* ... */
  const dispatch = useTasksDispatch();
  /* ... */
}
```

```jsx
/* TasksContext.js */

import { createContext, useContext, useReducer } from "react";

const TasksContext = createContext(null);
const TasksDispatchContext = createContext(null);

export function TasksProvider({ children }) {
  const [tasks, dispatch] = useReducer(tasksReducer, initialTasks);

  return (
    <TasksContext.Provider value={tasks}>
      <TasksDispatchContext.Provider value={dispatch}>
        {children}
      </TasksDispatchContext.Provider>
    </TasksContext.Provider>
  );
}

export function useTasks() {
  return useContext(TasksContext);
}

export function useTasksDispatch() {
  return useContext(TasksDispatchContext);
}

function tasksReducer(tasks, action) {
  switch (action.type) {
    case "added": {
      return [
        ...tasks,
        {
          id: action.id,
          text: action.text,
          done: false,
        },
      ];
    }
    case "changed": {
      return tasks.map((t) => {
        if (t.id === action.task.id) {
          return action.task;
        } else {
          return t;
        }
      });
    }
    case "deleted": {
      return tasks.filter((t) => t.id !== action.id);
    }
    default: {
      throw Error("Unknown action: " + action.type);
    }
  }
}

const initialTasks = [
  { id: 0, text: "Philosopher’s Path", done: true },
  { id: 1, text: "Visit the temple", done: false },
  { id: 2, text: "Drink matcha", done: false },
];
```

# Refs

## Introduction

"Refs are an escape hatch to hold onto values that aren’t used for rendering. . . . Like state, refs let you retain information between re-renders of a component. Unlike state, setting the ref’s `current` value does not trigger a re-render." ([React](https://react.dev/learn/referencing-values-with-refs))

## Use Cases

"If your component needs to store some value, but it doesn’t impact the rendering logic, choose refs." ([React](https://react.dev/learn/referencing-values-with-refs))

"sometimes you might need access to the DOM elements managed by React—for example, to focus a node, scroll to it, or measure its size and position. There is no built-in way to do those things in React, so you will need a _ref_ to the DOM node." ([React](https://react.dev/learn/manipulating-the-dom-with-refs))

> Typically, you will use a ref when your component needs to “step outside” React and communicate with external APIs—often a browser API that won’t impact the appearance of the component. Here are a few of these rare situations:
>
> - Storing timeout IDs
> - Storing and manipulating DOM elements . . .
> - Storing other objects that aren’t necessary to calculate the JSX.
>
> [React](https://react.dev/learn/referencing-values-with-refs)

## Operation

### General

> You can imagine that inside of React, useRef is implemented like this:
>
> ```jsx
> // Inside of React
> function useRef(initialValue) {
>   const [ref, unused] = useState({ current: initialValue });
>   return ref;
> }
> ```
>
> During the first render, `useRef` returns `{ current: initialValue }`. This object is stored by React, so during the next render the same object will be returned. Note how the state setter is unused in this example. It is unnecessary because `useRef` always needs to return the same object!
>
> [React](https://react.dev/learn/referencing-values-with-refs)

> when you mutate the current value of a ref, it changes immediately:
>
> ```jsx
> ref.current = 5;
> console.log(ref.current); // 5
> ```
>
> [React](https://react.dev/learn/referencing-values-with-refs)

### DOM Nodes

"During the first render, the DOM nodes have not yet been created, so `ref.current` will be `null`. And during the rendering of updates, the DOM nodes haven’t been updated yet. So it’s too early to read them. React sets `ref.current` during the commit. Before updating the DOM, React sets the affected `ref.current` values to `null`. After updating the DOM, React immediately sets them to the corresponding DOM nodes." ([React](https://react.dev/learn/manipulating-the-dom-with-refs))

## Usage

### General

> ```jsx
> import { useRef } from "react";
>
> /* ... */
> const ref = useRef(0);
> ```
>
> `useRef` returns an object like this:
>
> ```jsx
> {
>   current: 0; // The value you passed to useRef
> }
> ```
>
> [React](https://react.dev/learn/referencing-values-with-refs)

"the `ref.current` property . . . value is intentionally mutable, meaning you can both read and write to it. . . . As long as the object you’re mutating isn’t used for rendering, React doesn’t care what you do with the ref or its contents." ([React](https://react.dev/learn/referencing-values-with-refs))

"Don’t read or write `ref.current` during rendering. If some information is needed during rendering, use state instead. Since React doesn’t know when `ref.current` changes, even reading it while rendering makes your component’s behavior difficult to predict." ([React](https://react.dev/learn/referencing-values-with-refs))

"Usually, you will access refs from event handlers. If you want to do something with a ref, but there is no particular event to do it in, you might need an Effect." ([React](https://react.dev/learn/manipulating-the-dom-with-refs))

### DOM Nodes

#### General

> To access a DOM node managed by React . . . pass your ref [(`const myRef = useRef(null);`)] as the `ref` attribute to the JSX tag for which you want to get the DOM node:
>
> ```html
> <div ref="{myRef}"></div>
> ```
>
> [React](https://react.dev/learn/manipulating-the-dom-with-refs)

> ```jsx
> import { useRef } from "react";
>
> export default function Form() {
>   const inputRef = useRef(null);
>
>   function handleClick() {
>     inputRef.current.focus();
>   }
>
>   return (
>     <>
>       <input ref={inputRef} />
>       <button onClick={handleClick}>Focus the input</button>
>     </>
>   );
> }
> ```
>
> [React](https://react.dev/learn/manipulating-the-dom-with-refs)

"If you stick to non-destructive actions like focusing and scrolling, you shouldn’t encounter any problems. However, if you try to modify the DOM manually, you can risk conflicting with the changes React is making. . . . Avoid changing DOM nodes managed by React. Modifying, adding children to, or removing children from elements that are managed by React can lead to inconsistent visual results or crashes . . . You can safely modify parts of the DOM that React has _no reason_ to update. For example, if some `<div>` is always empty in the JSX, React won’t have a reason to touch its children list. Therefore, it is safe to manually add or remove elements there." ([React](https://react.dev/learn/manipulating-the-dom-with-refs))

#### Referencing Lists

> sometimes you might need a ref to each item in the list, and you don’t know how many you will have. . . . You can’t call `useRef` in a loop, in a condition, or inside a `map()` call. . . . solution is to pass a function to the `ref` attribute. This is called a `ref` callback. React will call your `ref` callback with the DOM node when it’s time to set the ref, and with `null` when it’s time to clear it. This lets you maintain your own array or a Map, and access any ref by its index or some kind of ID. . . .
>
> ```jsx
> /* ... */
> const itemsRef = useRef(null);
> /* ... */
>
> function getMap() {
>   if (!itemsRef.current) {
>     // Initialize the Map on first usage.
>     itemsRef.current = new Map();
>   }
>   return itemsRef.current;
> }
>
> /* ... */
> <ul>
>   {catList.map((cat) => (
>     <li
>       key={cat}
>       ref={(node) => {
>         const map = getMap();
>         if (node) {
>           map.set(cat, node);
>         } else {
>           map.delete(cat);
>         }
>       }}
>     >
>       <img src={cat} />
>     </li>
>   ))}
> </ul>;
> /* ... */
> ```
>
> [React](https://react.dev/learn/manipulating-the-dom-with-refs)

#### Flushing the DOM

> Consider code like this, which adds a new todo and scrolls the screen down to the last child of the list. Notice how, for some reason, it always scrolls to the todo that was just before the last added one . . . The issue is with these two lines:
>
> ```jsx
> setTodos([...todos, newTodo]);
> listRef.current.lastChild.scrollIntoView();
> ```
>
> In React, state updates are queued. Usually, this is what you want. However, here it causes a problem because `setTodos` does not immediately update the DOM. So the time you scroll the list to its last element, the todo has not yet been added. This is why scrolling always “lags behind” by one item. To fix this issue, you can force React to update (“flush”) the DOM synchronously. To do this, import `flushSync` from `react-dom` and wrap the state update into a `flushSync` call:
>
> ```jsx
> flushSync(() => {
>   setTodos([...todos, newTodo]);
> });
> listRef.current.lastChild.scrollIntoView();
> ```
>
> This will instruct React to update the DOM synchronously right after the code wrapped in `flushSync` executes. As a result, the last todo will already be in the DOM by the time you try to scroll to it
>
> [React](https://react.dev/learn/manipulating-the-dom-with-refs)

### Components

"When you put a ref on a built-in component that outputs a browser element like `<input />`, React will set that ref’s `current` property to the corresponding DOM node (such as the actual `<input />` in the browser). However, if you try to put a ref on your own component, like `<MyInput />`, by default you will get `null`. . . . This happens because by default React does not let a component access the DOM nodes of other components. Not even for its own children! This is intentional. Refs are an escape hatch that should be used sparingly. Manually manipulating _another_ component’s DOM nodes makes your code even more fragile." ([React](https://react.dev/learn/manipulating-the-dom-with-refs))

"In design systems, it is a common pattern for low-level components like buttons, inputs, and so on, to forward their refs to their DOM nodes. On the other hand, high-level components like forms, lists, or page sections usually won’t expose their DOM nodes to avoid accidental dependencies on the DOM structure." ([React](https://react.dev/learn/manipulating-the-dom-with-refs))

> components that want to expose their DOM nodes have to opt in to that behavior. A component can specify that it “forwards” its ref to one of its children . . . can use the `forwardRef` API . . .
>
> ```jsx
> import { forwardRef, useRef } from "react";
>
> const MyInput = forwardRef((props, ref) => {
>   return <input {...props} ref={ref} />;
> });
>
> export default function Form() {
>   const inputRef = useRef(null);
>
>   function handleClick() {
>     inputRef.current.focus();
>   }
>
>   return (
>     <>
>       <MyInput ref={inputRef} />
>       <button onClick={handleClick}>Focus the input</button>
>     </>
>   );
> }
> ```
>
> [React](https://react.dev/learn/manipulating-the-dom-with-refs)

> Exposing a subset of the API with an imperative handle . . . In uncommon cases, you may want to restrict the exposed functionality. You can do that with `useImperativeHandle`:
>
> ```jsx
> import { forwardRef, useRef, useImperativeHandle } from "react";
>
> const MyInput = forwardRef((props, ref) => {
>   const restrictedRef = useRef(null);
>
>   useImperativeHandle(ref, () => ({
>     // Only expose focus and nothing else
>     focus() {
>       restrictedRef.current.focus();
>     },
>   }));
>
>   return <input {...props} ref={restrictedRef} />;
> });
> ```
>
> . . . `inputRef.current` inside the `Form` component will only have the `focus` method. In this case, the ref “handle” is not the DOM node, but the custom object you create inside `useImperativeHandle` call.
>
> [React](https://react.dev/learn/manipulating-the-dom-with-refs) (with a slightly modified code)

# Events

## Architecture

"If you use a design system, it’s common for components like buttons to contain styling but not specify behavior. Instead, components like `PlayButton` and `UploadButton` will pass event handlers down." ([React](https://react.dev/learn/responding-to-events))

## Naming Convention

"In React, it’s conventional to use `onSomething` names for props which represent events and `handleSomething` for the function definitions which handle those events." ([React](https://react.dev/learn/tutorial-tic-tac-toe))

"By convention, it is common to name event handlers as `handle` followed by the event name. You’ll often see `onClick={handleClick}`, `onMouseEnter={handleMouseEnter}`, and so on." ([React](https://react.dev/learn/responding-to-events))

## Propagation

"Event handlers will also catch events from any children your component might have. We say that an event “bubbles” or “propagates” up the tree" ([React](https://react.dev/learn/responding-to-events))

"All events propagate in React except `onScroll`, which only works on the JSX tag you attach it to." ([React](https://react.dev/learn/responding-to-events))

> In rare cases, you might need to catch all events on child elements, even if they stopped propagation. For example, maybe you want to log every click to analytics, regardless of the propagation logic. You can do this by adding `Capture` at the end of the event name:
>
> ```jsx
> <div
>   onClickCapture={() => {
>     /* this runs first */
>   }}
> >
>   <button onClick={(e) => e.stopPropagation()} />
>   <button onClick={(e) => e.stopPropagation()} />
> </div>
> ```
>
> . . . Capture events are useful for code like routers or analytics, but you probably won’t use them in app code.
>
> [React](https://react.dev/learn/responding-to-events)

## _Passing Handlers_ Pattern

"Explicitly calling an event handler prop from a child handler is a good alternative to propagation." ([React](https://react.dev/learn/responding-to-events))

> this click handler runs a line of code and then calls the `onClick` prop passed by the parent:
>
> ```jsx
> function Button({ onClick, children }) {
>   return (
>     <button
>       onClick={(e) => {
>         e.stopPropagation();
>         onClick();
>       }}
>     >
>       {children}
>     </button>
>   );
> }
> ```
>
> You could add more code to this handler before calling the parent `onClick` event handler, too. This pattern provides an _alternative_ to propagation. It lets the child component handle the event, while also letting the parent component specify some additional behavior. Unlike propagation, it’s not automatic. But the benefit of this pattern is that you can clearly follow the whole chain of code that executes as a result of some event.
>
> If you rely on propagation and it’s difficult to trace which handlers execute and why, try this approach instead.
>
> [React](https://react.dev/learn/responding-to-events)

# Side Effects

## Distinction

### By Cause

#### Event

"changes—updating the screen, starting an animation, changing the data—are called **side effects**. They’re things that happen _“on the side”_, not during rendering." ([React](https://react.dev/learn/keeping-components-pure))

"In React, side effects usually belong inside event handlers. . . . Even though event handlers are defined _inside_ your component, they don’t run _during_ rendering! So event handlers don’t need to be pure." ([React](https://react.dev/learn/keeping-components-pure))

#### Rendering

"If you’ve exhausted all other options and can’t find the right event handler for your side effect, you can still attach it to your returned JSX with a `useEffect` call in your component. This tells React to execute it later, after rendering, when side effects are allowed. However, this approach should be your last resort. When possible, try to express your logic with rendering alone." ([React](https://react.dev/learn/keeping-components-pure))

"in this text, capitalized “Effect” refers to the React-specific definition . . . i.e. a side effect caused by rendering. To refer to the broader programming concept, we’ll say “side effect”." ([React](https://react.dev/learn/synchronizing-with-effects))

"Effects let you specify side effects that are caused by rendering itself, rather than by a particular event. Sending a message in the chat is an _event_ because it is directly caused by the user clicking a specific button. However, setting up a server connection is an _Effect_ because it should happen no matter which interaction caused the component to appear." ([React](https://react.dev/learn/synchronizing-with-effects))

### By Value Reactivity

> Props, state, and variables declared inside your component’s body are called reactive values. . . . They participate in the rendering data flow . . . can change due to a re-render . . .
>
> Event handlers and Effects respond to changes differently:
>
> - **Logic inside event handlers is not reactive.** It will not run again unless the user performs the same interaction (e.g. a click) again. Event handlers can read reactive values without “reacting” to their changes.
> - **Logic inside Effects is reactive.** If your Effect reads a reactive value, you have to specify it as a dependency. Then, if a re-render causes that value to change, React will re-run your Effect’s logic with the new value.
>
> [React](https://react.dev/learn/separating-events-from-effects)

## Effects

### Operation

"Effects let you run some code after rendering so that you can synchronize your component with some system outside of React. . . . Effects run at the end of a commit after the screen updates. This is a good time to synchronize the React components with some external system (like network or a third-party library)." ([React](https://react.dev/learn/synchronizing-with-effects))

"Every time your component renders, React will update the screen _and then_ run the code inside `useEffect`. In other words, `useEffect` “delays” a piece of code from running until that render is reflected on the screen. . . . By wrapping the DOM update in an Effect, you let React update the screen first. Then your Effect runs." ([React](https://react.dev/learn/synchronizing-with-effects))

"Every time after your component re-renders, React will look at the array of dependencies that you have passed. If any of the values in the array is different from the value at the same spot that you passed during the previous render, React will re-synchronize your Effect." ([React](https://react.dev/learn/lifecycle-of-reactive-effects))

### Lifecycle

"Components can mount, update, and unmount. Each Effect has a separate lifecycle from the surrounding component. Each Effect describes a separate synchronization process that can start and stop. When you write and read Effects, think from each individual Effect’s perspective (how to start and stop synchronization) rather than from the component’s perspective (how it mounts, updates, or unmounts)." ([React](https://react.dev/learn/lifecycle-of-reactive-effects))

"Effects have a different lifecycle from components. Components may mount, update, or unmount. An Effect can only do two things: to start synchronizing something, and later to stop synchronizing it. . . . An Effect describes how to synchronize an external system to the current props and state. As your code changes, synchronization will need to happen more or less often." ([React](https://react.dev/learn/lifecycle-of-reactive-effects))

"Your Effect’s body specifies how to **start synchronizing** . . . The cleanup function returned by your Effect specifies how to **stop synchronizing**" ([React](https://react.dev/learn/lifecycle-of-reactive-effects))

"Previously, you were thinking from the component’s perspective. When you looked from the component’s perspective, it was tempting to think of Effects as “callbacks” or “lifecycle events” that fire at a specific time like “after a render” or “before unmount”. This way of thinking gets complicated very fast, so it’s best to avoid. Instead, always focus on a single start/stop cycle at a time. It shouldn’t matter whether a component is mounting, updating, or unmounting. All you need to do is to describe how to start synchronization and how to stop it. If you do it well, your Effect will be resilient to being started and stopped as many times as it’s needed. This might remind you how you don’t think whether a component is mounting or updating when you write the rendering logic that creates JSX. You describe what should be on the screen, and React figures out the rest." ([React](https://react.dev/learn/lifecycle-of-reactive-effects))

### Basic Usage

> ```jsx
> import { useEffect } from "react";
>
> export default function Component() {
>   useEffect(() => {
>     /* ... */
>   });
>
>   return <section>...</section>;
> }
> ```
>
> Based on [React](https://react.dev/learn/synchronizing-with-effects)

### Dependencies

#### Impact

> ```jsx
> useEffect(() => {
>   // This runs after every render
> });
>
> useEffect(() => {
>   // This runs only on mount (when the component appears)
> }, []);
>
> useEffect(() => {
>   // This runs on mount *and also* if either a or b have changed since the last render
> }, [a, b]);
> ```
>
> [React](https://react.dev/learn/synchronizing-with-effects)

"React compares the dependency values using the `Object.is` comparison." ([React](https://react.dev/learn/synchronizing-with-effects))

#### Linting

"You can’t “choose” your dependencies. Your dependencies must include every reactive value you read in the Effect. The linter enforces this." ([React](https://react.dev/learn/lifecycle-of-reactive-effects))

"If your linter is configured for React, it will check that every reactive value used by your Effect’s code is declared as its dependency" ([React](https://react.dev/learn/lifecycle-of-reactive-effects))

> ```jsx
> Lint Error
> 14:6 - React Hook useEffect has a missing dependency: 'isPlaying'. Either include it or remove the dependency array.
> ```
>
> . . . Notice that you can’t “choose” your dependencies. You will get a lint error if the dependencies you specified don’t match what React expects based on the code inside your Effect. This helps catch many bugs in your code. If you don’t want some code to re-run, edit the Effect code itself to not “need” that dependency.
>
> [React](https://react.dev/learn/synchronizing-with-effects)

> If you have an existing codebase, you might have some Effects that suppress the linter like this:
>
> ```jsx
> useEffect(() => {
>   // ...
>   // 🔴 Avoid suppressing the linter like this:
>   // eslint-ignore-next-line react-hooks/exhaustive-deps
> }, []);
> ```
>
> [React](https://react.dev/learn/lifecycle-of-reactive-effects)

"All errors flagged by the linter are legitimate. There’s always a way to fix the code to not break the rules." ([React](https://react.dev/learn/lifecycle-of-reactive-effects))

#### Identification

##### Reactive Values

"Props and state aren’t the only reactive values. Values that you calculate from them are also reactive. If the props or state change, your component will re-render, and the values calculated from them will also change. This is why all variables from the component body used by the Effect should be in the Effect dependency list." ([React](https://react.dev/learn/lifecycle-of-reactive-effects))

"Why doesn’t `serverUrl` need to be a dependency? This is because the `serverUrl` never changes due to a re-render. It’s always the same no matter how many times the component re-renders and why. Since `serverUrl` never changes, it wouldn’t make sense to specify it as a dependency. After all, dependencies only do something when they change over time! On the other hand, `roomId` may be different on a re-render. Props, state, and other values declared inside the component are reactive because they’re calculated during rendering and participate in the React data flow. If `serverUrl` was a state variable, it would be reactive. Reactive values must be included in dependencies" ([React](https://react.dev/learn/lifecycle-of-reactive-effects))

##### Mutable Values

"A mutable value like `ref.current` or things you read from it also can’t be a dependency. The ref object returned by `useRef` itself can be a dependency, but its `current` property is intentionally mutable. It lets you keep track of something without triggering a re-render. But since changing it doesn’t trigger a re-render, it’s not a reactive value, and React won’t know to re-run your Effect when it changes." ([React](https://react.dev/learn/lifecycle-of-reactive-effects))

- "the `ref` object has a stable identity: React guarantees you’ll always get the same object from the same `useRef` call on every render. It never changes, so it will never by itself cause the Effect to re-run. Therefore, it does not matter whether you include it or not. Including it is fine too . . . The `set` functions returned by `useState` also have stable identity, so you will often see them omitted from the dependencies too. If the linter lets you omit a dependency without errors, it is safe to do. Omitting always-stable dependencies only works when the linter can “see” that the object is stable. For example, if `ref` was passed from a parent component, you would have to specify it in the dependency array." ([React](https://react.dev/learn/synchronizing-with-effects))

"Mutable values (including global variables) aren’t reactive. A mutable value like `location.pathname` can’t be a dependency. It’s mutable, so it can change at any time completely outside of the React rendering data flow. Changing it wouldn’t trigger a re-render of your component. Therefore, even if you specified it in the dependencies, React wouldn’t know to re-synchronize the Effect when it changes. . . . Instead, you should read and subscribe to an external mutable value with `useSyncExternalStore`." ([React](https://react.dev/learn/lifecycle-of-reactive-effects))

#### Patterns

##### Moving Non-Reactive Values Outside Component

"you could . . . “prove” to the linter that these values aren’t reactive values, i.e. that they can’t change as a result of a re-render. For example, if `serverUrl` and `roomId` don’t depend on rendering and always have the same values, you can move them outside the component. Now they don’t need to be dependencies . . . You can also move them _inside the Effect_. They aren’t calculated during rendering, so they’re not reactive" ([React](https://react.dev/learn/lifecycle-of-reactive-effects))

##### Updating State Without Using Reactive Values

> If you want to update some state based on the previous state, pass an updater function. . . .
>
> This Effect updates the `messages` state variable with a newly created array every time a new message arrives . . . since `messages` is a reactive value read by an Effect, it must be a dependency:
>
> ```jsx
> function ChatRoom({ roomId }) {
>   const [messages, setMessages] = useState([]);
>   useEffect(() => {
>     const connection = createConnection();
>     connection.connect();
>     connection.on("message", (receivedMessage) => {
>       setMessages([...messages, receivedMessage]);
>     });
>     return () => connection.disconnect();
>   }, [roomId, messages]); // ✅ All dependencies declared
>   // ...
> }
> ```
>
> And making `messages` a dependency introduces a problem. Every time you receive a message, `setMessages()` causes the component to re-render with a new `messages` array that includes the received message. However, since this Effect now depends on `messages`, this will _also_ re-synchronize the Effect. So every new message will make the chat re-connect. The user would not like that!
>
> To fix the issue, don’t read `messages` inside the Effect. Instead, pass an updater function to `setMessages`:
>
> ```jsx
> function ChatRoom({ roomId }) {
>   const [messages, setMessages] = useState([]);
>   useEffect(() => {
>     const connection = createConnection();
>     connection.connect();
>     connection.on("message", (receivedMessage) => {
>       setMessages((msgs) => [...msgs, receivedMessage]);
>     });
>     return () => connection.disconnect();
>   }, [roomId]); // ✅ All dependencies declared
>   // ...
> }
> ```
>
> Notice how your Effect does not read the `messages` variable at all now. . . . As a result of this fix, receiving a chat message will no longer make the chat re-connect.
>
> [React](https://react.dev/learn/removing-effect-dependencies)

##### Avoiding Objects/Functions as Dependencies

"Avoid relying on objects and functions as dependencies. If you create objects and functions during rendering and then read them from an Effect, they will be different on every render. This will cause your Effect to re-synchronize every time." ([React](https://react.dev/learn/lifecycle-of-reactive-effects))

"In JavaScript, each newly created object and function is considered distinct from all the others. It doesn’t matter that the contents inside of them may be the same! . . . Object and function dependencies can make your Effect re-synchronize more often than you need. This is why, whenever possible, you should try to avoid objects and functions as your Effect’s dependencies. Instead, try moving them outside the component, inside the Effect, or extracting primitive values out of them. . . . Move static objects and functions outside your component . . . Move dynamic objects and functions inside your Effect . . . If your object depends on some reactive value that may change as a result of a re-render . . . you can’t pull it outside your component. You can, however, move its creation inside of your Effect’s code" ([React](https://react.dev/learn/removing-effect-dependencies))

"Read primitive values from objects . . . Sometimes, you may receive an object from props . . . The risk here is that the parent component will create the object during rendering . . . This would cause your Effect to re-connect every time the parent component re-renders. To fix this, read information from the object _outside_ the Effect, and avoid having object and function dependencies" ([React](https://react.dev/learn/removing-effect-dependencies))

"Calculate primitive values from functions . . . For example, suppose the parent component passes a function . . . To avoid making it a dependency (and causing it to re-connect on re-renders), call it outside the Effect. . . . This only works for pure functions because they are safe to call during rendering." ([React](https://react.dev/learn/removing-effect-dependencies))

"You can write your own functions to group pieces of logic inside your Effect. As long as you also declare them _inside_ your Effect, they’re not reactive values, and so they don’t need to be dependencies of your Effect." ([React](https://react.dev/learn/removing-effect-dependencies))

### Cleanup Function

#### Usage

> ```jsx
> useEffect(() => {
>   const connection = createConnection();
>
>   connection.connect();
>
>   return () => {
>     connection.disconnect();
>   };
> }, []);
> ```
>
> [React](https://react.dev/learn/synchronizing-with-effects)

#### Operation

"React will call your cleanup function each time before the Effect runs again, and one final time when the component unmounts" ([React](https://react.dev/learn/synchronizing-with-effects))

"Effects from each render are isolated from each other. If you’re curious how this works, you can read about closures." ([React](https://react.dev/learn/synchronizing-with-effects))

"Some Effects don’t return a cleanup function at all. More often than not, you’ll want to return one—but if you don’t, React will behave as if you returned an empty cleanup function." ([React](https://react.dev/learn/lifecycle-of-reactive-effects))

#### Strict Mode

##### Operation

"As the user navigates across the app, the connections would keep piling up. Bugs like this are easy to miss without extensive manual testing. To help you spot them quickly, in development React remounts every component once immediately after its initial mount. Seeing the `"✅ Connecting..."` log twice helps you notice the real issue: your code doesn’t close the connection when the component unmounts." ([React](https://react.dev/learn/synchronizing-with-effects))

> Now you get three console logs in development:
>
> ```
> "✅ Connecting..."
> "❌ Disconnected."
> "✅ Connecting..."
> ```
>
> This is the correct behavior in development. By remounting your component, React verifies that navigating away and back would not break your code. . . . There’s an extra connect/disconnect call pair because React is probing your code for bugs in development. This is normal—don’t try to make it go away! In production, you would only see `"✅ Connecting..."` printed once. Remounting components only happens in development to help you find Effects that need cleanup. You can turn off Strict Mode to opt out of the development behavior, but we recommend keeping it on.
>
> [React](https://react.dev/learn/synchronizing-with-effects)

##### Working with Preventive APIs

> Some APIs may not allow you to call them twice in a row. For example, the `showModal` method of the built-in `<dialog>` element throws if you call it twice. Implement the cleanup function and make it close the dialog:
>
> ```jsx
> useEffect(() => {
>   const dialog = dialogRef.current;
>   dialog.showModal();
>   return () => dialog.close();
> }, []);
> ```
>
> [React](https://react.dev/learn/synchronizing-with-effects)

#### Use Cases

##### Positive

"Imagine the `ChatRoom` component is a part of a larger app with many different screens. The user starts their journey on the `ChatRoom` page. The component mounts and calls `connection.connect()`. Then imagine the user navigates to another screen—for example, to the `Settings` page. The `ChatRoom` component unmounts. Finally, the user clicks Back and `ChatRoom` mounts again. This would set up a second connection—but the first connection was never destroyed!" ([React](https://react.dev/learn/synchronizing-with-effects))

> If your Effect subscribes to something, the cleanup function should unsubscribe:
>
> ```jsx
> useEffect(() => {
>   function handleScroll(e) {
>     console.log(window.scrollX, window.scrollY);
>   }
>   window.addEventListener("scroll", handleScroll);
>   return () => window.removeEventListener("scroll", handleScroll);
> }, []);
> ```
>
> [React](https://react.dev/learn/synchronizing-with-effects)

> If your Effect animates something in, the cleanup function should reset the animation to the initial values:
>
> ```jsx
> useEffect(() => {
>   const node = ref.current;
>   node.style.opacity = 1; // Trigger the animation
>   return () => {
>     node.style.opacity = 0; // Reset to the initial value
>   };
> }, []);
> ```
>
> [React](https://react.dev/learn/synchronizing-with-effects)

> If your Effect fetches something, the cleanup function should either abort the fetch or ignore its result:
>
> ```jsx
> useEffect(() => {
>   let ignore = false;
>
>   async function startFetching() {
>     const json = await fetchTodos(userId);
>     if (!ignore) {
>       setTodos(json);
>     }
>   }
>
>   startFetching();
>
>   return () => {
>     ignore = true;
>   };
> }, [userId]);
> ```
>
> You can’t “undo” a network request that already happened, but your cleanup function should ensure that the fetch that’s not relevant anymore does not keep affecting your application. If the `userId` changes from `'Alice'` to `'Bob'`, cleanup ensures that the `'Alice'` response is ignored even if it arrives after `'Bob'`.
>
> In development, you will see two fetches in the Network tab. There is nothing wrong with that. With the approach above, the first Effect will immediately get cleaned up so its copy of the `ignore` variable will be set to `true`. So even though there is an extra request, it won’t affect the state thanks to the `if (!ignore)` check. In production, there will only be one request.
>
> [React](https://react.dev/learn/synchronizing-with-effects)

##### Negative

> Sometimes you need to add UI widgets that aren’t written to React. For example, let’s say you’re adding a map component to your page. It has a `setZoomLevel()` method, and you’d like to keep the zoom level in sync with a `zoomLevel` state variable in your React code. Your Effect would look similar to this:
>
> ```jsx
> useEffect(() => {
>   const map = mapRef.current;
>   map.setZoomLevel(zoomLevel);
> }, [zoomLevel]);
> ```
>
> Note that there is no cleanup needed in this case.
>
> [React](https://react.dev/learn/synchronizing-with-effects)

### Best Practices

#### Separating Independent Processeses

"Check that your Effect represents an independent synchronization process. If your Effect doesn’t synchronize anything, it might be unnecessary. If it synchronizes several independent things, split it up." ([React](https://react.dev/learn/lifecycle-of-reactive-effects))

"Resist adding unrelated logic to your Effect only because this logic needs to run at the same time as an Effect you already wrote. For example, let’s say you want to send an analytics event when the user visits the room. You already have an Effect that depends on `roomId`, so you might feel tempted to add the analytics call there . . . But imagine you later add another dependency to this Effect that needs to re-establish the connection. If this Effect re-synchronizes, it will also call `logVisit(roomId)` for the same room, which you did not intend. Logging the visit is a separate process from connecting. Write them as two separate Effects . . . Each Effect in your code should represent a separate and independent synchronization process." ([React](https://react.dev/learn/lifecycle-of-reactive-effects))

"deleting one Effect wouldn’t break the other Effect’s logic. This is a good indication that they synchronize different things, and so it made sense to split them up. On the other hand, if you split up a cohesive piece of logic into separate Effects, the code may look “cleaner” but will be more difficult to maintain. This is why you should think whether the processes are same or separate, not whether the code looks cleaner." ([React](https://react.dev/learn/lifecycle-of-reactive-effects))

#### Extracting into Effect Events

"If you want to read the latest value of props or state without “reacting” to it and re-synchronizing the Effect, you can split your Effect into a reactive part (which you’ll keep in the Effect) and a non-reactive part (which you’ll extract into something called an _Effect Event_)." ([React](https://react.dev/learn/lifecycle-of-reactive-effects))

#### Extracting into Custom Hooks

"In general, whenever you have to resort to writing Effects, keep an eye out for when you can extract a piece of functionality into a custom Hook with a more declarative and purpose-built API . . . The fewer raw `useEffect` calls you have in your components, the easier you will find to maintain your application." ([React](https://react.dev/learn/you-might-not-need-an-effect))

### Use Cases

#### General

"Use Effects only for code that should run _because_ the component was displayed to the user." ([React](https://react.dev/learn/you-might-not-need-an-effect))

"Keep in mind that Effects are typically used to “step out” of your React code and synchronize with some _external_ system. This includes browser APIs, third-party widgets, network, and so on. If your Effect only adjusts some state based on other state, you might not need an Effect." ([React](https://react.dev/learn/synchronizing-with-effects))

"You can use a similar approach to wrap legacy non-React code (like jQuery plugins) into declarative React components." ([React](https://react.dev/learn/synchronizing-with-effects))

#### Data Fetching

##### Introduction

"Writing fetch calls inside Effects is a popular way to fetch data, especially in fully client-side apps." ([React](https://react.dev/learn/synchronizing-with-effects))

"You can also fetch data with Effects: for example, you can synchronize the search results with the current search query. Keep in mind that modern frameworks provide more efficient built-in data fetching mechanisms than writing Effects directly in your components." ([React](https://react.dev/learn/you-might-not-need-an-effect))

> data fetching is not trivial to do well, so we recommend the following approaches:
>
> - If you use a framework, use its built-in data fetching mechanism. Modern React frameworks have integrated data fetching mechanisms that are efficient and don’t suffer from the above pitfalls.
> - Otherwise, consider using or building a client-side cache. Popular open source solutions include React Query, useSWR, and React Router 6.4+.
>
> [React](https://react.dev/learn/synchronizing-with-effects)

##### Challenges

"You can build your own solution too, in which case you would use Effects under the hood, but add logic for deduplicating requests, caching responses, and avoiding network waterfalls (by preloading data or hoisting data requirements to routes)." ([React](https://react.dev/learn/synchronizing-with-effects))

"Handling race conditions is not the only difficulty with implementing data fetching. You might also want to think about caching responses (so that the user can click Back and see the previous screen instantly), how to fetch data on the server (so that the initial server-rendered HTML contains the fetched content instead of a spinner), and how to avoid network waterfalls (so that a child can fetch data without waiting for every parent). These issues apply to any UI library, not just React. Solving them is not trivial, which is why modern frameworks provide more efficient built-in data fetching mechanisms than fetching data in Effects." ([React](https://react.dev/learn/you-might-not-need-an-effect))

> - Effects **don’t run on the server**. This means that the initial server-rendered HTML will only include a loading state with no data. The client computer will have to download all JavaScript and render your app only to discover that now it needs to load the data. . . .
> - Fetching directly in Effects makes it easy to create **“network waterfalls”**. You render the parent component, it fetches some data, renders the child components, and then they start fetching their data. If the network is not very fast, this is significantly slower than fetching all data in parallel.
> - Fetching directly in Effects usually means you **don’t preload or cache data**. For example, if the component unmounts and then mounts again, it would have to fetch the data again.
> - It’s not very ergonomic. There’s quite a bit of **boilerplate code** involved when writing `fetch` calls in a way that doesn’t suffer from bugs like race conditions.
>
> [React](https://react.dev/learn/synchronizing-with-effects)

##### Usage

> If you don’t use a framework (and don’t want to build your own) but would like to make data fetching from Effects more ergonomic, consider extracting your fetching logic into a custom Hook like in this example:
>
> ```jsx
> function SearchResults({ query }) {
>   const [page, setPage] = useState(1);
>   const params = new URLSearchParams({ query, page });
>   const results = useData(`/api/search?${params}`);
>
>   function handleNextPageClick() {
>     setPage(page + 1);
>   }
>   // ...
> }
>
> function useData(url) {
>   const [data, setData] = useState(null);
>
>   useEffect(() => {
>     let ignore = false;
>
>     fetch(url)
>       .then((response) => response.json())
>       .then((json) => {
>         if (!ignore) {
>           setData(json);
>         }
>       });
>
>     return () => (ignore = true);
>   }, [url]);
>
>   return data;
> }
> ```
>
> You’ll likely also want to add some logic for error handling and to track whether the content is loading. . . . Although this alone won’t be as efficient as using a framework’s built-in data fetching mechanism, moving the data fetching logic into a custom Hook will make it easier to adopt an efficient data fetching strategy later.
>
> [React](https://react.dev/learn/you-might-not-need-an-effect)

## External Store Subscriptions

### Introduction

"Sometimes, your components may need to subscribe to some data outside of the React state. This data could be from a third-party library or a built-in browser API. Since this data can change without React’s knowledge, you need to manually subscribe your components to it. . . . Although it’s common to use Effects for this, React has a purpose-built Hook for subscribing to an external store that is preferred instead. . . . This approach is less error-prone than manually syncing mutable data to React state with an Effect." ([React](https://react.dev/learn/you-might-not-need-an-effect))

### Usage

> ```jsx
> import { useSyncExternalStore } from "react";
>
> function ChatIndicator() {
>   const isOnline = useOnlineStatus();
>
>   // ...
> }
>
> function useOnlineStatus() {
>   return useSyncExternalStore(
>     subscribe,
>     () => navigator.online,
>     () => true
>   );
> }
>
> function subscribe(callback) {
>   window.addEventListener("online", callback);
>   window.addEventListener("offline", callback);
>
>   return () => {
>     window.removeEventListener("online", callback);
>     window.removeEventListener("offline", callback);
>   };
> }
> ```
>
> From [React](https://react.dev/learn/you-might-not-need-an-effect) (modified)

### Best Practices

"Typically, you’ll write a custom Hook . . . so that you don’t need to repeat this code in the individual components." ([React](https://react.dev/learn/you-might-not-need-an-effect))

# Hooks

## `useMemo`

> ```jsx
> function TodoList({ todos, filter }) {
>   const [newTodo, setNewTodo] = useState("");
>   // ✅ This is fine if getFilteredTodos() is not slow.
>   const visibleTodos = getFilteredTodos(todos, filter);
>   // ...
> }
> ```
>
> Usually, this code is fine! But maybe `getFilteredTodos()` is slow or you have a lot of todos. In that case you don’t want to recalculate `getFilteredTodos()` if some unrelated state variable like newTodo has changed. You can cache (or “memoize”) an expensive calculation by wrapping it in a `useMemo` Hook . . .
>
> ```jsx
> import { useMemo, useState } from "react";
>
> function TodoList({ todos, filter }) {
>   const [newTodo, setNewTodo] = useState("");
>   // ✅ Does not re-run getFilteredTodos() unless todos or filter change
>   const visibleTodos = useMemo(
>     () => getFilteredTodos(todos, filter),
>     [todos, filter]
>   );
>   // ...
> }
> ```
>
> This tells React that you don’t want the inner function to re-run unless either `todos` or `filter` have changed. React will remember the return value of `getFilteredTodos()` during the initial render. During the next renders, it will check if `todos` or `filter` are different. If they’re the same as last time, `useMemo` will return the last result it has stored. But if they are different, React will call the inner function again (and store its result). The function you wrap in `useMemo` runs during rendering, so this only works for pure calculations.
>
> [React](https://react.dev/learn/you-might-not-need-an-effect)

"How to tell if a calculation is expensive? In general, unless you’re creating or looping over thousands of objects, it’s probably not expensive. If you want to get more confidence, you can add a console log to measure the time spent in a piece of code . . . If the overall logged time adds up to a significant amount (say, `1ms` or more), it might make sense to memoize that calculation. As an experiment, you can then wrap the calculation in useMemo to verify whether the total logged time has decreased for that interaction or not . . . `useMemo` won’t make the _first_ render faster. It only helps you skip unnecessary work on updates.

## Custom

"the code inside them describes what they want to do (use the online status!) rather than how to do it (by subscribing to the browser events). When you extract logic into custom Hooks, you can hide the gnarly details of how you deal with some external system or a browser API. The code of your components expresses your intent, not the implementation." ([React](https://react.dev/learn/reusing-logic-with-custom-hooks))

"You’ll likely often use custom Hooks created by others, but occasionally you might write one yourself! . . . Many excellent custom Hooks are maintained by the React community." ([React](https://react.dev/learn/reusing-logic-with-custom-hooks))

"Hook names must start with `use` followed by a capital letter . . . If your linter is configured for React, it will enforce this naming convention." ([React](https://react.dev/learn/reusing-logic-with-custom-hooks))

"Hooks may return arbitrary values." ([React](https://react.dev/learn/reusing-logic-with-custom-hooks))

"This convention guarantees that you can always look at a component and know where its state, Effects, and other React features might “hide”. For example, if you see a `getColor()` function call inside your component, you can be sure that it can’t possibly contain React state inside because its name doesn’t start with `use`. However, a function call like `useOnlineStatus()` will most likely contain calls to other Hooks inside! . . . Only Hooks and components can call other Hooks" ([React](https://react.dev/learn/reusing-logic-with-custom-hooks))

"If your function doesn’t call any Hooks, avoid the `use` prefix. Instead, write it as a regular function without the `use` prefix. . . . This ensures that your code can call this regular function anywhere, including conditions" ([React](https://react.dev/learn/reusing-logic-with-custom-hooks))

"You should give use prefix to a function (and thus make it a Hook) if it uses at least one Hook inside of it" ([React](https://react.dev/learn/reusing-logic-with-custom-hooks))

"Custom Hooks let you share stateful logic but not state itself. Each call to a Hook is completely independent from every other call to the same Hook." ([React](https://react.dev/learn/reusing-logic-with-custom-hooks))

"The code inside your custom Hooks will re-run during every re-render of your component. This is why, like components, custom Hooks need to be pure. Think of custom Hooks’ code as part of your component’s body!" ([React](https://react.dev/learn/reusing-logic-with-custom-hooks))

Best practices:

- "Ideally, your custom Hook’s name should be clear enough that even a person who doesn’t write code often could have a good guess about what your custom Hook does, what it takes, and what it returns: ✅ `useData(url)` ✅ `useImpressionLog(eventName, extraData)` ✅ `useChatRoom(options)` When you synchronize with an external system, your custom Hook name may be more technical and use jargon specific to that system. It’s good as long as it would be clear to a person familiar with that system: ✅ `useMediaQuery(query)` ✅ `useSocket(url)` ✅ `useIntersectionObserver(ref, options)`" ([React](https://react.dev/learn/reusing-logic-with-custom-hooks))
- "Keep custom Hooks focused on concrete high-level use cases. Avoid creating and using custom “lifecycle” Hooks that act as alternatives and convenience wrappers for the useEffect API itself: 🔴 `useMount(fn)` 🔴 `useEffectOnce(fn)` 🔴 `useUpdateEffect(fn)`" ([React](https://react.dev/learn/reusing-logic-with-custom-hooks))
- "A good custom Hook makes the calling code more declarative by constraining what it does. For example, `useChatRoom(options)` can only connect to the chat room, while `useImpressionLog(eventName, extraData)` can only send an impression log to the analytics. If your custom Hook API doesn’t constrain the use cases and is very abstract, in the long run it’s likely to introduce more problems than it solves." ([React](https://react.dev/learn/reusing-logic-with-custom-hooks))

Use cases:

- "whenever you write an Effect, consider whether it would be clearer to also wrap it in a custom Hook. You shouldn’t need Effects very often, so if you’re writing one, it means that you need to “step outside React” to synchronize with some external system or to do something that React doesn’t have a built-in API for. Wrapping it into a custom Hook lets you precisely communicate your intent and how the data flows through it. . . . With time, most of your app’s Effects will be in custom Hooks." ([React](https://react.dev/learn/reusing-logic-with-custom-hooks))
- "Effects are an “escape hatch”: you use them when you need to “step outside React” and when there is no better built-in solution for your use case. With time, the React team’s goal is to reduce the number of the Effects in your app to the minimum by providing more specific solutions to more specific problems. Wrapping your Effects in custom Hooks makes it easier to upgrade your code when these solutions become available." ([React](https://react.dev/learn/reusing-logic-with-custom-hooks))

# Application Design & Development

## Step 1: Designing a Component Hierarchy

> Imagine that you already have a JSON API and a mockup from a designer. The JSON API returns some data that looks like this:
>
> ```json
> [
>   { "category": "Fruits", "price": "$1", "stocked": true, "name": "Apple" },
>   {
>     "category": "Fruits",
>     "price": "$1",
>     "stocked": true,
>     "name": "Dragonfruit"
>   },
>   {
>     "category": "Fruits",
>     "price": "$2",
>     "stocked": false,
>     "name": "Passionfruit"
>   },
>   {
>     "category": "Vegetables",
>     "price": "$2",
>     "stocked": true,
>     "name": "Spinach"
>   },
>   {
>     "category": "Vegetables",
>     "price": "$4",
>     "stocked": false,
>     "name": "Pumpkin"
>   },
>   { "category": "Vegetables", "price": "$1", "stocked": true, "name": "Peas" }
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
>
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
> . . . After building your components, you’ll have a library of reusable components that render your data model. . . . The component at the top of the hierarchy . . . will take your data model as a prop. This is called _one-way data flow_ because the data flows down from the top-level component to the ones at the bottom of the tree.
>
> [React](https://react.dev/learn/thinking-in-react)

## Step 3: Identifying the State

> To make the UI interactive, you need to let users change your underlying data model. You will use _state_ for this.
>
> Think of state as the minimal set of changing data that your app needs to remember. The most important principle for structuring state is to keep it DRY (Don’t Repeat Yourself). Figure out the absolute minimal representation of the state your application needs and compute everything else on-demand. . . .
>
> - Does it **remain unchanged** over time? If so, it isn’t state.
> - Is it **passed in from a parent** via props? If so, it isn’t state.
> - **Can you compute it** based on existing state or props in your component? If so, it _definitely_ isn’t state!
>
> [React](https://react.dev/learn/thinking-in-react)

> 1. The original list of products is **passed in as props, so it’s not state**.
> 2. The search text seems to be state since it changes over time and can’t be computed from anything.
> 3. The value of the checkbox seems to be state since it changes over time and can’t be computed from anything.
> 4. The filtered list of products **isn’t state because it can be computed** by taking the original list of products and filtering it according to the search text and value of the checkbox.
>
> [React](https://react.dev/learn/thinking-in-react)

> Props and state are different, but they work together. A parent component will often keep some information in state (so that it can change it), and _pass it down_ to child components as their props.
>
> [React](https://react.dev/learn/thinking-in-react)

## Step 4: Implementing the State Flowing Down

> you need to identify which component is responsible for changing this state, or owns the state. Remember: React uses one-way data flow, passing data down the component hierarchy from parent to child component. . . .
>
> For each piece of state in your application:
>
> 1. Identify _every_ component that renders something based on that state.
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

# Component Design & Development

## Overview

"React provides a declarative way to manipulate the UI. Instead of manipulating individual pieces of the UI directly, you describe the different states that your component can be in, and switch between them in response to the user input. This is similar to how designers think about the UI. . . . In imperative programming . . . you have to write the exact instructions to manipulate the UI depending on what just happened. . . . Manipulating the UI imperatively works well enough for isolated examples, but it gets exponentially more difficult to manage in more complex systems. Imagine updating a page full of different forms like this one. Adding a new UI element or a new interaction would require carefully checking all existing code to make sure you haven’t introduced a bug (for example, forgetting to show or hide something). React was built to solve this problem. In React, you don’t directly manipulate the UI . . . Instead, you declare what you want to show, and React figures out how to update the UI." ([React](https://react.dev/learn/reacting-to-input-with-state))

Component design and development phases:

> 1. **Identify** your component’s different visual states
> 2. **Determine** what triggers those state changes
> 3. **Represent** the state in memory using useState
> 4. **Remove** any non-essential state variables
> 5. **Connect** the event handlers to set the state
>
> [React](https://react.dev/learn/reacting-to-input-with-state)

## Step 1: Identifying Visual States

"you need to visualize all the different “states” of the UI the user might see . . . Just like a designer, you’ll want to “mock up” or create “mocks” for the different states before you add logic. . . . a mock for just the visual part of the form . . . mock is controlled by a prop . . . Mocking lets you quickly iterate on the UI before you wire up any logic. . . . prototype of the same component . . . “controlled” by the . . . prop" ([React](https://react.dev/learn/reacting-to-input-with-state))

> If a component has a lot of visual states, it can be convenient to show them all on one page:
>
> ```jsx
> let statuses = ["empty", "typing", "submitting", "success", "error"];
>
> export default function App() {
>   return (
>     <>
>       {statuses.map((status) => (
>         <section key={status}>
>           <h4>Form ({status}):</h4>
>           <Form status={status} />
>         </section>
>       ))}
>     </>
>   );
> }
> ```
>
> Pages like this are often called “living styleguides” or “storybooks”.
>
> [React](https://react.dev/learn/reacting-to-input-with-state)

## Step 2: Identifying State Triggers

> You can trigger state updates in response to two kinds of inputs:
>
> 1. **Human inputs**, like clicking a button, typing in a field, navigating a link.
> 2. **Computer inputs**, like a network response arriving, a timeout completing, an image loading.
>
> [React](https://react.dev/learn/reacting-to-input-with-state)

> For the form you’re developing, you will need to change state in response to a few different inputs:
>
> - **Changing the text input** (human) should switch it from the _Empty_ state to the _Typing_ state or back, depending on whether the text box is empty or not.
> - **Clicking the Submit button** (human) should switch it to the _Submitting_ state.
> - **Successful network response** (computer) should switch it to the _Success_ state.
> - **Failed network response** (computer) should switch it to the _Error_ state with the matching error message.
>
> [React](https://react.dev/learn/reacting-to-input-with-state)

> To help visualize . . . [the] flow, try drawing each state on paper as a labeled circle, and each change between two states as an arrow. You can sketch out many flows this way and sort out bugs long before implementation.
>
> ![Image](/assets/responding_to_input_flow.webp)
>
> [React](https://react.dev/learn/reacting-to-input-with-state)

## Step 3: Drafting State

"you’ll need to represent the visual states of your component in memory with `useState`. Simplicity is key" ([React](https://react.dev/learn/reacting-to-input-with-state))

"you’ll need to store the `answer` for the input, and the `error` (if it exists) to store the last error . . . Then, you’ll need a state variable representing which one of the visual states that you want to display. There’s usually more than a single way to represent that in memory, so you’ll need to experiment with it. . . . start by adding enough state that you’re _definitely_ sure that all the possible visual states are covered . . . Your first idea likely won’t be the best, but that’s ok—refactoring state is a part of the process!" ([React](https://react.dev/learn/reacting-to-input-with-state))

## Step 4: Refactoring State

"Spending a little time on refactoring your state structure will make your components easier to understand, reduce duplication, and avoid unintended meanings." ([React](https://react.dev/learn/reacting-to-input-with-state))

> Here are some questions you can ask about your state variables:
>
> - **Does this state cause a paradox?** For example, `isTyping` and `isSubmitting` can’t both be `true`. A paradox usually means that the state is not constrained enough. There are four possible combinations of two booleans, but only three correspond to valid states. To remove the “impossible” state, you can combine these into a `status` that must be one of three values: `'typing'`, `'submitting'`, or `'success'`.
> - **Is the same information available in another state variable already?** . . .
> - **Can you get the same information from the inverse of another state variable?**
>
> [React](https://react.dev/learn/reacting-to-input-with-state)

"You know they are essential, because you can’t remove any of them without breaking the functionality." ([React](https://react.dev/learn/reacting-to-input-with-state))

"a non-null `error` doesn’t make sense when `status` is `'success'`. To model the state more precisely, you can extract it into a reducer. Reducers let you unify multiple state variables into a single object and consolidate all the related logic!" ([React](https://react.dev/learn/reacting-to-input-with-state))

## Step 5: Connecting State and Triggers

"create event handlers that update the state." ([React](https://react.dev/learn/reacting-to-input-with-state))

"Expressing all interactions as state changes lets you later introduce new visual states without breaking existing ones. It also lets you change what should be displayed in each state without changing the logic of the interaction itself." ([React](https://react.dev/learn/reacting-to-input-with-state))

# TypeScript Intergration

## Setup

"JSX is an embeddable XML-like syntax. It is meant to be transformed into valid JavaScript, though the semantics of that transformation are implementation-specific. JSX rose to popularity with the React framework, but has since seen other implementations as well." ([TypeScript](https://www.typescriptlang.org/docs/handbook/jsx.html))

"TypeScript supports embedding, type checking, and compiling JSX directly to JavaScript." ([TypeScript](https://www.typescriptlang.org/docs/handbook/jsx.html))

> In order to use JSX you must do two things.
>
> 1.  Name your files with a `.tsx` extension
> 2.  Enable the `jsx` option
>
> [TypeScript](https://www.typescriptlang.org/docs/handbook/jsx.html)

> TypeScript ships with three JSX modes: `preserve`, `react`, and `react-native`. These modes only affect the emit stage - type checking is unaffected. The `preserve` mode will keep the JSX as part of the output to be further consumed by another transform step (e.g. Babel). Additionally the output will have a `.jsx` file extension. The `react` mode will emit `React.createElement`, does not need to go through a JSX transformation before use, and the output will have a `.js` file extension. The react-native mode is the equivalent of `preserve` in that it keeps all JSX, but the output will instead have a `.js` file extension.
>
> | Mode           | Input     | Output                                            | Output File Extension |
> | -------------- | --------- | ------------------------------------------------- | --------------------- |
> | `preserve`     | `<div />` | `<div />`                                         | `.jsx`                |
> | `react`        | `<div />` | `React.createElement("div")`                      | `.js`                 |
> | `react-native` | `<div />` | `<div />`                                         | `.js`                 |
> | `react-jsx`    | `<div />` | `_jsx("div", {}, void 0);`                        | `.js`                 |
> | `react-jsxdev` | `<div />` | `_jsxDEV("div", {}, void 0, false, {...}, this);` | `.js`                 |
>
> [TypeScript](https://www.typescriptlang.org/docs/handbook/jsx.html)

"To use JSX with React you should use the [React typings](https://github.com/DefinitelyTyped/DefinitelyTyped/tree/master/types/react). These typings define the JSX namespace appropriately for use with React." ([TypeScript](https://www.typescriptlang.org/docs/handbook/jsx.html))

> Out of the box, TypeScript supports JSX and you can get full React Web support by adding `@types/react` and `@types/react-dom` to your project.
>
> ```bash
> npm install @types/react @types/react-dom
> ```
>
> [React](https://react.dev/learn/typescript)

> The following compiler options need to be set in your `tsconfig.json`:
>
> - `dom` must be included in `lib` (Note: If no `lib` option is specified, `dom` is included by default).
> - `jsx` must be set to one of the valid options. `preserve` should suffice for most applications. If you’re publishing a library, consult the [`jsx` documentation](https://www.typescriptlang.org/tsconfig/#jsx) on what value to choose.
>
> [React](https://react.dev/learn/typescript)

> There are multiple compiler flags which can be used to customize your JSX, which work as both a compiler flag and via inline per-file pragmas. To learn more see their tsconfig reference pages:
>
> - `jsxFactory`
> - `jsxFragmentFactory`
> - `jsxImportSource`
>
> [TypeScript](https://www.typescriptlang.org/docs/handbook/jsx.html)

## Type Checking

### Type Assertion

> Recall how to write a type assertion:
>
> ```ts
> const foo = <foo>bar;
> ```
>
> This asserts the variable `bar` to have the type `foo`. Since TypeScript also uses angle brackets for type assertions, combining it with JSX’s syntax would introduce certain parsing difficulties. As a result, TypeScript disallows angle bracket type assertions in `.tsx` files.
>
> Since the above syntax cannot be used in `.tsx` files, an alternate type assertion operator should be used: `as`. The example can easily be rewritten with the `as` operator.
>
> ```ts
> const foo = bar as foo;
> ```
>
> The `as` operator is available in both `.ts` and `.tsx` files, and is identical in behavior to the angle-bracket type assertion style.
>
> [TypeScript](https://www.typescriptlang.org/docs/handbook/jsx.html)

### Intrinsic vs. Value-Based Elements

> In order to understand type checking with JSX, you must first understand the difference between intrinsic elements and value-based elements. Given a JSX expression `<expr />`, expr may either refer to something intrinsic to the environment (e.g. a `div` or `span` in a DOM environment) or to a custom component that you’ve created. This is important for two reasons:
>
> 1.  For React, intrinsic elements are emitted as strings (`React.createElement("div")`), whereas a component you’ve created is not (`React.createElement(MyComponent)`).
> 2.  The types of the attributes being passed in the JSX element should be looked up differently. Intrinsic element attributes should be known intrinsically whereas components will likely want to specify their own set of attributes.
>
> TypeScript uses the same convention that React does for distinguishing between these. An intrinsic element always begins with a lowercase letter, and a value-based element always begins with an uppercase letter.
>
> [TypeScript](https://www.typescriptlang.org/docs/handbook/jsx.html)

> Intrinsic elements are looked up on the special interface `JSX.IntrinsicElements`. By default, if this interface is not specified, then anything goes and intrinsic elements will not be type checked. However, if this interface is present, then the name of the intrinsic element is looked up as a property on the `JSX.IntrinsicElements` interface. For example:
>
> ```ts
> declare namespace JSX {
>   interface IntrinsicElements {
>     foo: any;
>   }
> }
> <foo />; // ok
> <bar />; // error
> ```
>
> [TypeScript](https://www.typescriptlang.org/docs/handbook/jsx.html)

> Value-based elements are simply looked up by identifiers that are in scope.
>
> ```ts
> import MyComponent from "./myComponent";
>
> <MyComponent />; // ok
> <SomeOtherComponent />; // error
> ```
>
> There are two ways to define a value-based element:
>
> 1.  Function Component (FC)
> 2.  Class Component
>
> . . . [Function component] is defined as a JavaScript function where its first argument is a `props` object. TS enforces that its return type must be assignable to `JSX.Element`. . . . Because a Function Component is simply a JavaScript function, function overloads may be used here as well:
>
> ```ts
> interface ClickableProps {
>   children: JSX.Element[] | JSX.Element;
> }
>
> interface HomeProps extends ClickableProps {
>   home: JSX.Element;
> }
>
> interface SideProps extends ClickableProps {
>   side: JSX.Element | string;
> }
>
> function MainButton(prop: HomeProps): JSX.Element;
> function MainButton(prop: SideProps): JSX.Element;
> function MainButton(prop: ClickableProps): JSX.Element {
>   // ...
> }
> ```
>
> [TypeScript](https://www.typescriptlang.org/docs/handbook/jsx.html)

### Result Type

"By default the result of a JSX expression is typed as `any`. You can customize the type by specifying the `JSX.Element` interface. However, it is not possible to retrieve type information about the element, attributes or children of the JSX from this interface. It is a black box." ([TypeScript](https://www.typescriptlang.org/docs/handbook/jsx.html))

## Usage

### General

"Every file containing JSX must use the `.tsx` file extension. This is a TypeScript-specific extension that tells TypeScript that this file contains JSX." ([React](https://react.dev/learn/typescript))

Usage:

> ```ts
> function MyButton({ title }: { title: string }) {
>   return <button>{title}</button>;
> }
> ```
>
> [React](https://react.dev/learn/typescript)

Alternatively:

> ```ts
> interface MyButtonProps {
>   title: string;
>   disabled: boolean;
> }
>
> function MyButton({ title, disabled }: MyButtonProps) {
>   return <button disabled={disabled}>{title}</button>;
> }
> ```
>
> [React](https://react.dev/learn/typescript)

"The type definitions from `@types/react` include types for the built-in Hooks, so you can use them in your components without any additional setup. They are built to take into account the code you write in your component, so you will get inferred types a lot of the time and ideally do not need to handle the minutiae of providing the types." ([React](https://react.dev/learn/typescript))

### `useState`

> The `useState` Hook will re-use the value passed in as the initial state to determine what the type of the value should be. For example:
>
> ```ts
> // Infer the type as "boolean"
> const [enabled, setEnabled] = useState(false);
> ```
>
> This will assign the type of `boolean` to `enabled`, and `setEnabled` will be a function accepting either a `boolean` argument, or a function that returns a `boolean`.
>
> [React](https://react.dev/learn/typescript)

> If you want to explicitly provide a type for the state, you can do so by providing a type argument to the `useState` call . . .
>
> a common case where you may want to provide a type is when you have a union type. For example, status here can be one of a few different strings:
>
> ```ts
> type Status = "idle" | "loading" | "success" | "error";
>
> const [status, setStatus] = useState<Status>("idle");
> ```
>
> Or, as recommended . . . you can group related state as an object and describe the different possibilities via object types:
>
> ```ts
> type RequestState =
>   | { status: "idle" }
>   | { status: "loading" }
>   | { status: "success"; data: any }
>   | { status: "error"; error: Error };
>
> const [requestState, setRequestState] = useState<RequestState>({
>   status: "idle",
> });
> ```
>
> [React](https://react.dev/learn/typescript)

### `useReducer`

"The `useReducer` Hook . . . takes a reducer function and an initial state." ([React](https://react.dev/learn/typescript))

"The types for the reducer function are inferred from the initial state." ([React](https://react.dev/learn/typescript))

"You can optionally provide a type argument to the `useReducer` call to provide a type for the state, but it is often better to set the type on the initial state instead" ([React](https://react.dev/learn/typescript))

> ```ts
> import { useReducer } from "react";
>
> interface State {
>   count: number;
> }
>
> type CounterAction =
>   | { type: "reset" }
>   | { type: "setCount"; value: State["count"] };
>
> const initialState: State = { count: 0 };
>
> function stateReducer(state: State, action: CounterAction): State {
>   switch (action.type) {
>     case "reset":
>       return initialState;
>     case "setCount":
>       return { ...state, count: action.value };
>     default:
>       throw new Error("Unknown action");
>   }
> }
>
> export default function App() {
>   const [state, dispatch] = useReducer(stateReducer, initialState);
>
>   const addFive = () =>
>     dispatch({ type: "setCount", value: state.count + 5 });
>   const reset = () => dispatch({ type: "reset" });
>
>   return (
>     <div>
>       <h1>Welcome to my counter</h1>
>
>       <p>Count: {state.count}</p>
>       <button onClick={addFive}>Add 5</button>
>       <button onClick={reset}>Reset</button>
>     </div>
>   );
> }
> ```
>
> We are using TypeScript in a few key places:
>
> - `interface State` describes the shape of the reducer’s state.
> - `type CounterAction` describes the different actions which can be dispatched to the reducer.
> - `const initialState: State` provides a type for the initial state, and also the type which is used by `useReducer` by default.
> - `stateReducer(state: State, action: CounterAction): State` sets the types for the reducer function’s arguments and return value.
>
> [React](https://react.dev/learn/typescript)

> A more explicit alternative to setting the type on `initialState` is to provide a type argument to `useReducer`:
>
> ```ts
> import { stateReducer, State } from "./your-reducer-implementation";
>
> const initialState = { count: 0 };
>
> export default function App() {
>   const [state, dispatch] = useReducer<State>(stateReducer, initialState);
> }
> ```
>
> [React](https://react.dev/learn/typescript)

### `useContext`

"The type of the value provided by the context is inferred from the value passed to the createContext call" ([React](https://react.dev/learn/typescript))

> ```ts
> import { createContext, useContext, useState } from "react";
>
> type Theme = "light" | "dark" | "system";
> const ThemeContext = createContext<Theme>("system");
>
> const useGetTheme = () => useContext(ThemeContext);
>
> export default function MyApp() {
>   const [theme, setTheme] = useState<Theme>("light");
>
>   return (
>     <ThemeContext.Provider value={theme}>
>       <MyComponent />
>     </ThemeContext.Provider>
>   );
> }
>
> function MyComponent() {
>   const theme = useGetTheme();
>
>   return (
>     <div>
>       <p>Current theme: {theme}</p>
>     </div>
>   );
> }
> ```
>
> [React](https://react.dev/learn/typescript)

> This technique works when you have a default value which makes sense - but there are occasionally cases when you do not, and in those cases `null` can feel reasonable as a default value. However, to allow the type-system to understand your code, you need to explicitly set `ContextShape | null` on the `createContext`. This causes the issue that you need to eliminate the `| null` in the type for context consumers. Our recommendation is to have the Hook do a runtime check for it’s existence and throw an error when not present:
>
> ```ts
> import { createContext, useContext, useState, useMemo } from "react";
>
> // This is a simpler example, but you can imagine a more complex object here
> type ComplexObject = {
>   kind: string;
> };
>
> // The context is created with `| null` in the type, to accurately reflect the default value.
> const Context = createContext<ComplexObject | null>(null);
>
> // The `| null` will be removed via the check in the Hook.
> const useGetComplexObject = () => {
>   const object = useContext(Context);
>   if (!object) {
>     throw new Error("useGetComplexObject must be used within a Provider");
>   }
>   return object;
> };
>
> export default function MyApp() {
>   const object = useMemo(() => ({ kind: "complex" }), []);
>
>   return (
>     <Context.Provider value={object}>
>       <MyComponent />
>     </Context.Provider>
>   );
> }
>
> function MyComponent() {
>   const object = useGetComplexObject();
>
>   return (
>     <div>
>       <p>Current object: {object.kind}</p>
>     </div>
>   );
> }
> ```
>
> [React](https://react.dev/learn/typescript)

### `useMemo`

"The result of calling the Hook is inferred from the return value from the function in the first parameter. You can be more explicit by providing a type argument to the Hook." ([React](https://react.dev/learn/typescript))

> ```ts
> // The type of visibleTodos is inferred from the return value of filterTodos
> const visibleTodos = useMemo(() => filterTodos(todos, tab), [todos, tab]);
> ```
>
> [React](https://react.dev/learn/typescript)

### `useCallback`

"the function’s type is inferred from the return value of the function in the first parameter, and you can be more explicit by providing a type argument to the Hook." ([React](https://react.dev/learn/typescript))

> When working in TypeScript strict mode `useCallback` requires adding types for the parameters in your callback. This is because the type of the callback is inferred from the return value of the function, and without parameters the type cannot be fully understood. Depending on your code-style preferences, you could use the `*EventHandler` functions from the React types to provide the type for the event handler at the same time as defining the callback:

> ```ts
> import { useState, useCallback } from "react";
>
> export default function Form() {
>   const [value, setValue] = useState("Change me");
>
>   const handleChange = useCallback<
>     React.ChangeEventHandler<HTMLInputElement>
>   >(
>     (event) => {
>       setValue(event.currentTarget.value);
>     },
>     [setValue]
>   );
>
>   return (
>     <>
>       <input value={value} onChange={handleChange} />
>       <p>Value: {value}</p>
>     </>
>   );
> }
> ```
>
> [React](https://react.dev/learn/typescript)

## Built-In Types

### DOM Events

"When working with DOM events in React, the type of the event can often be inferred from the event handler. However, when you want to extract a function to be passed to an event handler, you will need to explicitly set the type of the event." ([React](https://react.dev/learn/typescript))

> ```ts
> import { useState } from "react";
>
> export default function Form() {
>   const [value, setValue] = useState("Change me");
>
>   function handleChange(event: React.ChangeEvent<HTMLInputElement>) {
>     setValue(event.currentTarget.value);
>   }
>
>   return (
>     <>
>       <input value={value} onChange={handleChange} />
>       <p>Value: {value}</p>
>     </>
>   );
> }
> ```
>
> [React](https://react.dev/learn/typescript)

"There are many types of events provided in the React types - the full list can be found [here](https://github.com/DefinitelyTyped/DefinitelyTyped/blob/b580df54c0819ec9df62b0835a315dd48b8594a9/types/react/index.d.ts#L1247C1-L1373) which is based on the most popular events from the DOM. When determining the type you are looking for you can first look at the hover information for the event handler you are using, which will show the type of the event. If you need to use an event that is not included in this list, you can use the `React.SyntheticEvent` type, which is the base type for all events." ([React](https://react.dev/learn/typescript))

### Children

> There are two common paths to describing the children of a component.
>
> The first is to use the `React.ReactNode` type, which is a union of all the possible types that can be passed as children in JSX:
>
> ```ts
> interface ModalRendererProps {
>   title: string;
>   children: React.ReactNode;
> }
> ```
>
> This is a very broad definition of children.
>
> The second is to use the `React.ReactElement` type, which is only JSX elements and not JavaScript primitives like strings or numbers:
>
> ```ts
> interface ModalRendererProps {
>   title: string;
>   children: React.ReactElement;
> }
> ```
>
> Note, that you cannot use TypeScript to describe that the children are a certain type of JSX elements, so you cannot use the type-system to describe a component which only accepts `<li>` children.
>
> [React](https://react.dev/learn/typescript)

> ```ts
> import React from "react";
>
> // React.ReactNode accepts the most inputs
> interface ReactNodeProps {
>   children: React.ReactNode;
> }
>
> const RNode = (props: ReactNodeProps) => <div>{props.children}</div>;
>
> const ReactNodeApp = () => (
>   <>
>     <RNode>
>       <p>One element</p>
>     </RNode>
>     <RNode>
>       <>
>         <p>Fragments for</p>
>         <p>More elements</p>
>       </>
>     </RNode>
>     <RNode>1</RNode>
>     <RNode>Hello</RNode>
>     <RNode>{null}</RNode>
>     <RNode>{true}</RNode>
>     // Must have children though
>     <RNode /> // Error
>   </>
> );
>
> // React.ReactElement accepts only JSX elements
> interface ReactElementProps {
>   children: React.ReactElement;
> }
>
> const RElement = (props: ReactElementProps) => <div>{props.children}</div>;
>
> const ReactElementApp = () => (
>   <>
>     <RElement>
>       <p>More elements</p>
>     </RElement>
>     <RElement>
>       <>
>         <p>More elements</p>
>         <p>More elements</p>
>       </>
>     </RElement>
>     // Must be a JSX element
>     <RElement>1</RElement> // Error
>     <RElement>Hello</RElement> // Error
>     <RElement>{null}</RElement> // Error
>     <RElement>{true}</RElement> // Error
>     {/* prettier-ignore */}
>     // Must have children though
>     <RElement /> // Error
>   </>
> );
> ```
>
> [TypeScript](https://www.typescriptlang.org/play/#code/JYWwDg9gTgLgBAJQKYEMDG8BmUIjgIilQ3wChSB6CxYmAOmXRgDkIATJOdNJMGAZzgwAFpxAR+8YADswAVwGkZMJFEzpOjDKw4AFHGEEBvUnDhphwADZsi0gFw0mDWjqQBuUgF9yaCNMlENzgAXjgACjADfkctFnYkfQhDAEpQgD44AB42YAA3dKMo5P46C2tbJGkvLIpcgt9-QLi3AEEwMFCItJDMrPTTbIQ3dKywdIB5aU4kKyQQKpha8drhhIGzLLWODbNs3b3s8YAxKBQAcwXpAThMaGWDvbH0gFloGbmrgQfBzYpd1YjQZbEYARkB6zMwO2SHSAAlZlYIBCdtCRkZpHIrFYahQYQD8UYYFA5EhcfjyGYqHAXnJAsIUHlOOUbHYhMIIHJzsI0Qk4P9SLUBuRqXEXEwAKKfRZcNA8PiCfxWACecAAUgBlAAacFm80W-CU11U6h4TgwUv11yShjgJjMLMqDnN9Dilq+nh8pD8AXgCHdMrCkWisVoAet0R6fXqhWKhjKllZVVxMcavpd4Zg7U6Qaj+2hmdG4zeRF10uu-Aeq0LBfLMEe-V+T2L7zLVu+FBWLdLeq+lc7DYFf39deFVOotMCACNOCh1dq219a+30uC8YWoZsRyuEdjkevR8uvoVMdjyTWt4WiSSydXD4NqZP4AymeZE072ZzuUeZQKheQgA)

### `style` Property

"When using inline styles in React, you can use `React.CSSProperties` to describe the object passed to the `style` prop. This type is a union of all the possible CSS properties, and is a good way to ensure you are passing valid CSS properties to the `style` prop, and to get auto-complete in your editor." ([React](https://react.dev/learn/typescript))

> ```ts
> interface MyComponentProps {
>   style: React.CSSProperties;
> }
> ```
>
> [React](https://react.dev/learn/typescript)
