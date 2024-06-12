# React Reference

## TOC

### Overview

### Components

- **General**
- **Internals**
- **Built-In Components**
  - HTML Elements
  - `<Fragment>`
- **Props**
- **Rendering**
- **Purity**
  - General
  - `StrictMode`
  - Side Effects
- **Returned Value**
- **The Render Tree**
- **The Module Dependency Tree**
- **Organization**

### Styling

### JSX

- **Definition**
- **Escaping Into JavaScript**
  - General
  - HTML Attributes
  - Control Flow
    - Conditionals
    - Loops
- **Requirements**

### Hooks

- **General**
- **`useRef`**

### Events

### State

- **Defining State**
- **Sharing State**
- **Data Immutability and Rendering**

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

### State Structuring

- **Overview**
- **Grouping Related State Variables**
- **Avoiding State Contradictions**
- **Avoiding State Redundancies**
- **Avoiding State Duplications**
- **Avoiding Deep State Nesting**

### State Management

- **Reducers**
- **Context**
- **Reducers With Context**

### Effects

# Overview

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

# Components

## General

- "React components are JavaScript functions that return markup" ([React](https://react.dev/learn))
  - "They return JSX markup" ([React](https://react.dev/learn/your-first-component))
- "Components are used to render, manage, and update the UI elements in your application" ([React](https://react.dev/learn/tutorial-tic-tac-toe))

## Internals

- "JSX looks like HTML, but under the hood it is transformed into plain JavaScript objects. You can’t return two objects from a function without wrapping them into an array. This explains why you also can’t return two JSX tags without wrapping them into another tag or a Fragment." ([React](https://react.dev/learn/writing-markup-with-jsx))

## Built-In Components

### HTML Elements

- "`className="square"` is a button property or *prop* . . . The DOM `<button>` element’s `onClick` attribute has a special meaning to React because it is a built-in component. For custom components like `Square`, the naming is up to you. You could give any name to the `Square`’s `onSquareClick` prop or `Board`’s `handleClick` function, and the code would work the same." ([React](https://react.dev/learn/tutorial-tic-tac-toe))
- "`<img />` is written like HTML, but it is actually JavaScript under the hood!" ([React](https://react.dev/learn/your-first-component))
- "JavaScript has limitations on variable names. For example, their names can’t contain dashes or be reserved words like `class`. Since `class` is a reserved word, in React you write `className` instead, named after the corresponding DOM property" ([React](https://react.dev/learn/writing-markup-with-jsx))
- "For historical reasons, `aria-*` and `data-*` attributes are written as in HTML with dashes." ([React](https://react.dev/learn/writing-markup-with-jsx))
- "JSX elements aren’t “instances” because they don’t hold any internal state and aren’t real DOM nodes." ([React](https://react.dev/learn/conditional-rendering))

### `<Fragment>`

- "empty tag is called a *Fragment*. Fragments let you group things without leaving any trace in the browser HTML tree." ([React](https://react.dev/learn/writing-markup-with-jsx))
- "What do you do when each item needs to render not one, but several DOM nodes? The short `<>...</>` Fragment syntax won’t let you pass a key, so you need to either group them into a single `<div>`, or use the slightly longer and more explicit `<Fragment>` syntax . .  . Fragments disappear from the DOM, so this will produce a flat list" ([React](https://react.dev/learn/rendering-lists))

## Props
  
- "React component functions accept a single argument, a `props` object . . . Usually you don’t need the whole `props` object itself, so you destructure it into individual props." ([React](https://react.dev/learn/passing-props-to-a-component))
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
- "If you want to give a prop a default value to fall back on when no value is specified, you can do it with the destructuring by putting `=` and the default value right after the parameter" ([React](https://react.dev/learn/passing-props-to-a-component))
- "props are immutable . . . When a component needs to change its props (for example, in response to a user interaction or new data), it will have to “ask” its parent component to pass it different props—a new object! Its old props will then be cast aside, and eventually the JavaScript engine will reclaim the memory taken by them." ([React](https://react.dev/learn/passing-props-to-a-component))
- > Forwarding props with the JSX spread syntax . . .
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
- "When you nest content inside a JSX tag, the parent component will receive that content in a prop called `children`. . . . You can think of a component with a `children` prop as having a “hole” that can be “filled in” by its parent components with arbitrary JSX. You will often use the `children` prop for visual wrappers: panels, grids, etc." ([React](https://react.dev/learn/passing-props-to-a-component))
  - ```jsx
    import MyComponent from './components/MyComponent.jsx';
    import NestedComponent from './components/NestedComponent.jsx';

    export default function App() {
      return (
        <>
          <MyComponent>
            <NestedComponent />
          </MyComponent>
        </>
      )
    }
    ```
  - ```jsx
    export default function MyComponent({ children }) {
      return (
        <>
          <h1>MyComponent</h1>
          <section>
            {children}
          </section>
        </>
      );
    }
    ```
- "In React, it’s conventional to use `onSomething` names for props which represent events and `handleSomething` for the function definitions which handle those events." ([React](https://react.dev/learn/tutorial-tic-tac-toe))
- "It is common to call a component with some local state “uncontrolled”. . . . In contrast, you might say a component is “controlled” when the important information in it is driven by props rather than its own local state. This lets the parent component fully specify its behavior. . . . Uncontrolled components are easier to use within their parents because they require less configuration. But they’re less flexible when you want to coordinate them together. Controlled components are maximally flexible, but they require the parent components to fully configure them with props. In practice, “controlled” and “uncontrolled” aren’t strict technical terms—each component usually has some mix of both local state and props. However, this is a useful way to talk about how components are designed and what capabilities they offer." ([React](https://react.dev/learn/sharing-state-between-components))

## Rendering

- "Local variables don’t persist between renders." ([React](https://react.dev/learn/state-a-components-memory))
- "Changes to local variables won’t trigger renders." ([React](https://react.dev/learn/state-a-components-memory))
- > To update a component with new data, two things need to happen:
  > 1. **Retain** the data between renders.
  > 2. **Trigger** React to render the component with new data (re-rendering).
  >
  > The useState Hook provides those two things:
  > 1. A **state variable** to retain the data between renders.
  > 2. A **state setter function** to update the variable and trigger React to render the component again.
  >
  > [React](https://react.dev/learn/state-a-components-memory)
- > process of requesting and serving UI has three steps:
  > 1. Triggering a render . . .
  > 2. Rendering the component . . .
  > 3. Committing to the DOM
  >
  > [React](https://react.dev/learn/render-and-commit)
- > There are two reasons for a component to render:
  > 1. It’s the component’s **initial render**.
  > 2. The component’s (or one of its ancestors’) **state has been updated**.
  >
  > [React](https://react.dev/learn/render-and-commit)
- > After you trigger a render, React calls your components to figure out what to display on screen. “Rendering” is React calling your components.
  > - On initial render, React will call the root component.
  > - For subsequent renders, React will call the function component whose state update triggered the render.
  >
  > This process is recursive: if the updated component returns some other component, React will render that component next, and if that component also returns something, it will render that component next, and so on. The process will continue until there are no more nested components and React knows exactly what should be displayed on screen.
  >
  > [React](https://react.dev/learn/render-and-commit)
- > After rendering (calling) your components, React will modify the DOM.
  > - For the initial render, React will use the appendChild() DOM API to put all the DOM nodes it has created on screen.
  > - For re-renders, React will apply the minimal necessary operations (calculated while rendering!) to make the DOM match the latest rendering output.
  >
  > [React](https://react.dev/learn/render-and-commit)
- "React only changes the DOM nodes if there’s a difference between renders. . . . you can add some text into the `<input>`, updating its `value`, but the text doesn’t disappear when the component re-renders" ([React](https://react.dev/learn/render-and-commit))
- "After rendering is done and React updated the DOM, the browser will repaint the screen. Although this process is known as “browser rendering”, we’ll refer to it as “painting” to avoid confusion throughout the docs." ([React](https://react.dev/learn/render-and-commit))
- "state behaves more like a snapshot. Setting it does not change the state variable you already have, but instead triggers a re-render." ([React](https://react.dev/learn/state-as-a-snapshot))
- "Setting state only changes it for the next render. . . . The state stored in React may have changed by the time the `alert` runs, but it was scheduled using a snapshot of the state at the time the user interacted with it! A state variable’s value never changes within a render, even if its event handler’s code is asynchronous. . . . the value of `number` continues to be `0` even after `setNumber(number + 5)` was called . . . React keeps the state values “fixed” within one render’s event handlers. . . . Event handlers created in the past have the state values from the render in which they were created." ([React](https://react.dev/learn/state-as-a-snapshot))
- "React waits until all code in the event handlers has run before processing your state updates. . . . This behavior, also known as batching, makes your React app run much faster. . . . React does not batch across *multiple* intentional events like clicks—each click is handled separately. Rest assured that React only does batching when it’s generally safe to do." ([React](https://react.dev/learn/queueing-a-series-of-state-updates))
- > It is an uncommon use case, but if you would like to update the same state variable multiple times before the next render, instead of passing the next state value like `setNumber(number + 1)`, you can pass a function that calculates the next state based on the previous one in the queue, like `setNumber(n => n + 1)`. . . . `n => n + 1` is called an updater function. When you pass it to a state setter:
  > 1. React queues this function to be processed after all the other code in the event handler has run.
  > 2. During the next render, React goes through the queue and gives you the final updated state.
  >
  > . . . React takes the return value of your previous updater function and passes it to the next updater
  >
  > [React](https://react.dev/learn/queueing-a-series-of-state-updates)
- `setNumber(number + 5); setNumber(n => n + 1);` (for `number === 0` initially) will result in `number === 6` in the next render (see [React](https://react.dev/learn/queueing-a-series-of-state-updates))
- `setNumber(number + 5); setNumber(n => n + 1); setNumber(number + 4);` (for `number === 0` initially) will result in `number === 4` in the next render (see [React](https://react.dev/learn/queueing-a-series-of-state-updates))
- "Updater functions run during rendering, so **updater functions must be pure** and only *return* the result. Don’t try to set state from inside of them or run other side effects. In Strict Mode, React will run each updater function twice (but discard the second result) to help you find mistakes." ([React](https://react.dev/learn/queueing-a-series-of-state-updates))
- > It’s common to name the updater function argument by the first letters of the corresponding state variable:
  >
  > ```jsx
  > setEnabled(e => !e);
  > setLastName(ln => ln.reverse());
  > setFriendCount(fc => fc * 2);
  > ```
  >
  > If you prefer more verbose code, another common convention is to repeat the full state variable name, like `setEnabled(enabled => !enabled)`, or to use a prefix like `setEnabled(prevEnabled => !prevEnabled)`.
  >
  > [React](https://react.dev/learn/queueing-a-series-of-state-updates)
- "`setState(5)` actually works like `setState(n => 5)`, but `n` is unused!" ([React](https://react.dev/learn/queueing-a-series-of-state-updates))
## Purity

### General

- > a pure function is a function with the following characteristics:
  > - **It minds its own business.** It does not change any objects or variables that existed before it was called.
  > - **Same inputs, same output.** Given the same inputs, a pure function should always return the same result.
  >
  > [React](https://react.dev/learn/keeping-components-pure)
- "React assumes that every component you write is a pure function. This means that React components you write must always return the same JSX given the same inputs" ([React](https://react.dev/learn/keeping-components-pure))
- "Components should only *return* their JSX, and not *change* any objects or variables that existed before rendering—that would make them impure!" ([React](https://react.dev/learn/keeping-components-pure))
- "Pure functions don’t mutate variables outside of the function’s scope or objects that were created before the call—that makes them impure!  However, it’s completely fine to change variables and objects that you’ve *just* created while rendering. . . . it’s fine because you’ve created them during the same render . . . No code outside of [it] . . . will ever know that this happened. This is called “local mutation”—it’s like your component’s little secret." ([React](https://react.dev/learn/keeping-components-pure))

### `StrictMode`

- "You should never change preexisting variables or objects while your component is rendering. React offers a “Strict Mode” in which it calls each component’s function twice during development. By calling the component functions twice, Strict Mode helps find components that break these rules. . . . Pure functions only calculate, so calling them twice won’t change anything" ([React](https://react.dev/learn/keeping-components-pure))
- "Strict Mode has no effect in production, so it won’t slow down the app for your users." ([React](https://react.dev/learn/keeping-components-pure))
- "To opt into Strict Mode, you can wrap your root component into `<React.StrictMode>`" ([React](https://react.dev/learn/keeping-components-pure))

### Side Effects

- "changes—updating the screen, starting an animation, changing the data—are called **side effects**. They’re things that happen *“on the side”*, not during rendering." ([React](https://react.dev/learn/keeping-components-pure))
- "In React, side effects usually belong inside event handlers. . . . Even though event handlers are defined *inside* your component, they don’t run *during* rendering! So event handlers don’t need to be pure." ([React](https://react.dev/learn/keeping-components-pure))
- "If you’ve exhausted all other options and can’t find the right event handler for your side effect, you can still attach it to your returned JSX with a `useEffect` call in your component. This tells React to execute it later, after rendering, when side effects are allowed. However, this approach should be your last resort. When possible, try to express your logic with rendering alone." ([React](https://react.dev/learn/keeping-components-pure))

## Returned Value

  - "In some situations, you won’t want to render anything at all. . . . A component must return something. In this case, you can return `null` . . . In practice, returning `null` from a component isn’t common . . . More often, you would conditionally include or exclude the component in the parent component’s JSX." ([React](https://react.dev/learn/conditional-rendering))

## The Render Tree

- "As we nest components, we have the concept of parent and child components, where each parent component may itself be a child of another component.  When we render a React app, we can model this relationship in a tree, known as the render tree." ([React](https://react.dev/learn/understanding-your-ui-as-a-tree))
- > The root node in a React render tree is the root component of the app. In this case, the root component is `App` and it is the first component React renders. . . .
  >
  > ![Image](/assets/conditional_render_tree.webp)
  >
  > With conditional rendering, across different renders, the render tree may render different components.
  >
  > [React](https://react.dev/learn/understanding-your-ui-as-a-tree)
- "the render tree is only composed of React components. React, as a UI framework, is platform agnostic. On react.dev, we showcase examples that render to the web, which uses HTML markup as its UI primitives. But a React app could just as likely render to a mobile or desktop platform, which may use different UI primitives like UIView or FrameworkElement. These platform UI primitives are not a part of React. React render trees can provide insight to our React app regardless of what platform your app renders to." ([React](https://react.dev/learn/understanding-your-ui-as-a-tree))
- "Although render trees may differ across render passes, these trees are generally helpful for identifying what the `top-level` and `leaf components` are in a React app. Top-level components are the components nearest to the root component and affect the rendering performance of all the components beneath them and often contain the most complexity. Leaf components are near the bottom of the tree and have no child components and are often frequently re-rendered." ([React](https://react.dev/learn/understanding-your-ui-as-a-tree))
- "Top-level components affect the rendering performance of all components beneath them and leaf components are often re-rendered frequently. Identifying them is useful for understanding and debugging rendering performance." ([React](https://react.dev/learn/understanding-your-ui-as-a-tree))
- "when React removes a component, it destroys its state. . . . React preserves a component’s state for as long as it’s being rendered at its position in the UI tree." ([React](https://react.dev/learn/preserving-and-resetting-state))
- > In this example, there are two different `<Counter />` tags:
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
- "Different components at the same position reset state . . . you switch between different component types at the same position. Initially, the first child of the `<div>` contained a `Counter`. But when you swapped in a `p`, React removed the `Counter` from the UI tree and destroyed its state." ([React](https://react.dev/learn/preserving-and-resetting-state))
- "Also, when you render a different component in the same position, it resets the state of its entire subtree." ([React](https://react.dev/learn/preserving-and-resetting-state))
- "As a rule of thumb, if you want to preserve the state between re-renders, the structure of your tree needs to “match up” from one render to another. If the structure is different, the state gets destroyed because React destroys state when it removes a component from the tree." ([React](https://react.dev/learn/preserving-and-resetting-state))
- > you should not nest component function definitions.
  >
  > ```jsx
  > import { useState } from 'react';
  >
  > export default function MyComponent() {
  >   const [counter, setCounter] = useState(0);
  >
  >   function MyTextField() {
  >     const [text, setText] = useState('');
  >
  >     return (
  >       <input
  >         value={text}
  >         onChange={e => setText(e.target.value)}
  >       />
  >     );
  >   }
  >
  >   return (
  >     <>
  >       <MyTextField />
  >       <button onClick={() => {
  >         setCounter(counter + 1)
  >       }}>Clicked {counter} times</button>
  >     </>
  >   );
  > }
  > ```
  >
  > Every time you click the button, the input state disappears! This is because a *different* `MyTextField` function is created for every render of `MyComponent`. You’re rendering a *different* component in the same position, so React resets all state below. This leads to bugs and performance problems. To avoid this problem, always declare component functions at the top level, and don’t nest their definitions.
  >
  > [React](https://react.dev/learn/preserving-and-resetting-state)
- > By default, React preserves state of a component while it stays at the same position.
  >
  > ```jsx
  > /* ... */
  > {isPlayerA ? (
  >   <Counter person="Taylor" />
  > ) : (
  >   <Counter person="Sarah" />
  > )}
  > /* ... */
  > ```
  >
  > Currently, when you change the player, the score is preserved. The two `Counter`s appear in the same position, so React sees them as *the same* `Counter` whose `person` prop has changed. . . . If you want these two `Counter`s to be independent, you can render them in two different positions:
  >
  > ```jsx
  > /* ... */
  > {isPlayerA &&
  >   <Counter person="Taylor" />
  > }
  > {!isPlayerA &&
  >   <Counter person="Sarah" />
  > }
  > /* ... */
  > ```
  >
  > Each Counter’s state gets destroyed each time it’s removed from the DOM. . . .  This solution is convenient when you only have a few independent components rendered in the same place.
  >
  > [React](https://react.dev/learn/preserving-and-resetting-state)
- > There is also another, more generic, way to reset a component’s state. . . . You can use keys to make React distinguish between any components. By default, React uses order within the parent (“first counter”, “second counter”) to discern between components. But keys let you tell React that this is not just a *first* counter, or a *second* counter, but a specific counter . . . In this example, the two `<Counter />`s don’t share state even though they appear in the same place in JSX:
  >
  > ```jsx
  > {isPlayerA ? (
  >   <Counter key="Taylor" person="Taylor" />
  > ) : (
  >   <Counter key="Sarah" person="Sarah" />
  > )}
  > ```
  >
  > [React](https://react.dev/learn/preserving-and-resetting-state)
    - "keys are not globally unique. They only specify the position *within* the parent." ([React](https://react.dev/learn/preserving-and-resetting-state))
- "Resetting state with a key is particularly useful when dealing with forms." ([React](https://react.dev/learn/preserving-and-resetting-state))
- > There are a few ways to keep the state “alive” for a component that’s no longer visible:
  > - You could render *all* chats instead of just the current one, but hide all the others with CSS. The chats would not get removed from the tree, so their local state would be preserved. This solution works great for simple UIs. But it can get very slow if the hidden trees are large and contain a lot of DOM nodes.
  > - You could lift the state up and hold the pending message for each recipient in the parent component. This way, when the child components get removed, it doesn’t matter, because it’s the parent that keeps the important information. This is the most common solution.
  > - You might also use a different source in addition to React state. For example, you probably want a message draft to persist even if the user accidentally closes the page. To implement this, you could have the `Chat` component initialize its state by reading from the `localStorage`, and save the drafts there too.
  >
  > [React](https://react.dev/learn/preserving-and-resetting-state)

## The Module Dependency Tree

- > As we break up our components and logic into separate files, we create JS modules where we may export components, functions, or constants. . . .
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
- "bundlers will use the dependency tree to determine what modules should be included." ([React](https://react.dev/learn/understanding-your-ui-as-a-tree))
- "Large bundle sizes can delay the time for your UI to get drawn. Getting a sense of your app’s dependency tree may help with debugging these issues. . . . Dependency trees are useful for debugging large bundle sizes that slow time to paint" ([React](https://react.dev/learn/understanding-your-ui-as-a-tree))

## Organization

- "many websites only use React to add interactivity to existing HTML pages. They have many root components instead of a single one for the entire page." ([React](https://react.dev/learn/your-first-component))
- "a root component file, named `App.js` . . . Depending on your setup, your root component could be in another file, though. If you use a framework with file-based routing, such as Next.js, your root component will be different for every page." ([React](https://react.dev/learn/importing-and-exporting-components))
- "Components are regular JavaScript functions, so you can keep multiple components in the same file. This is convenient when components are relatively small or tightly related to each other. If this file gets crowded, you can always move . . . to a separate file." ([React](https://react.dev/learn/your-first-component))

# Styling

> React does not prescribe how you add CSS files. In the simplest case, you’ll add a `<link>` tag to your HTML.
>
> [React](https://react.dev/learn)

# JSX

## Definition

- "The markup syntax . . . is called *JSX*" ([React](https://react.dev/learn))

## Escaping Into JavaScript

### General

- "JSX lets you put markup into JavaScript. Curly braces let you “escape back” into JavaScript" ([React](https://react.dev/learn))
- "JavaScript inside the JSX `{` and `}` executes right away" ([React](https://react.dev/learn/responding-to-events))

### HTML Attributes

- "You can also “escape into JavaScript” from JSX attributes, but you have to use curly braces *instead of quotes*" ([React](https://react.dev/learn))
- "`style={{}}` is not a special syntax, but a regular `{}` object inside the `style={ }` JSX curly braces. You can use the `style` attribute when your styles depend on JavaScript variables." ([React](https://react.dev/learn))

### Control Flow

#### Conditionals

- "you can use the conditional `?` operator. Unlike `if`, it works inside JSX . . . you can also use a shorter logical `&&` syntax" ([React](https://react.dev/learn))
  - If the first operand of the `&&` operator after the automatic `Boolean` conversion becomes `false` but itself is not strictly `false` nor `null`/`undefined`, it will be rendered by React - examples include `0`, `NaN`, or `""` (see [React](https://react.dev/learn/conditional-rendering))

#### Loops

- "You will rely on JavaScript features like `for` loop and the array `map()` function to render lists of components." ([React](https://react.dev/learn))
- The `key` attribute:
  - "JSX elements directly inside a `map()` call always need keys!" ([React](https://react.dev/learn/rendering-lists))
  - "`<li>` has a `key` attribute. For each item in a list, you should pass a string or a number that uniquely identifies that item among its siblings." ([React](https://react.dev/learn))
  - Key generation rules:
    - "**Keys must be unique among siblings.** However, it’s okay to use the same keys for JSX nodes in different arrays." ([React](https://react.dev/learn/rendering-lists))
    - **Keys must not change** or that defeats their purpose! Don’t generate them while rendering. . . . do not generate keys on the fly, e.g. with `key={Math.random()}`. This will cause keys to never match up between renders, leading to all your components and DOM being recreated every time. Not only is this slow, but it will also lose any user input inside the list items. Instead, use a stable ID based on the data." ([React](https://react.dev/learn/rendering-lists))
    - "It’s strongly recommended that you assign proper keys whenever you build dynamic lists. If you don’t have an appropriate key, you may want to consider restructuring your data so that you do." ([React](https://react.dev/learn/tutorial-tic-tac-toe))
    - "If your data is coming from a database, you can use the database keys/IDs, which are unique by nature. . . . If your data is generated and persisted locally (e.g. notes in a note-taking app), use an incrementing counter, `crypto.randomUUID()` or a package like `uuid` when creating items." ([React](https://react.dev/learn/rendering-lists))
    - "You might be tempted to use an item’s index in the array as its key. In fact, that’s what React will use if you don’t specify a `key` at all. But the order in which you render items will change over time if an item is inserted, deleted, or if the array gets reordered. Index as a key often leads to subtle and confusing bugs." ([React](https://react.dev/learn/rendering-lists))

## Requirements

- "You have to close tags like `<br />`." ([React](https://react.dev/learn))
- "Your component also can’t return multiple JSX tags. You have to wrap them into a shared parent, like a `<div>...</div>` or an empty `<>...</>` wrapper" ([React](https://react.dev/learn))

# Hooks

## General

- "Functions starting with `use` are called Hooks." ([React](https://react.dev/learn))
- "*Hooks* are special functions that are only available while React is rendering . . . They let you “hook into” different React features." ([React](https://react.dev/learn/state-a-components-memory))
- "Hooks—functions starting with use—can only be called at the top level of your components or your own Hooks. You can’t call Hooks inside conditions, loops, or other nested functions. Hooks are functions, but it’s helpful to think of them as unconditional declarations about your component’s needs. You “use” React features at the top of your component similar to how you “import” modules at the top of your file." ([React](https://react.dev/learn/state-a-components-memory))
- "If you want to use `useState` in a condition or a loop, extract a new component and put it there." ([React](https://react.dev/learn))

## `useRef`

Definition:
- "Refs are an escape hatch to hold onto values that aren’t used for rendering. . . . Like state, refs let you retain information between re-renders of a component. Unlike state, setting the ref’s `current` value does not trigger a re-render." ([React](https://react.dev/learn/referencing-values-with-refs))
- "the `ref.current` property . . . value is intentionally mutable, meaning you can both read and write to it. . . . As long as the object you’re mutating isn’t used for rendering, React doesn’t care what you do with the ref or its contents." ([React](https://react.dev/learn/referencing-values-with-refs))
- > when you mutate the current value of a ref, it changes immediately:
  > ```jsx
  > ref.current = 5;
  > console.log(ref.current); // 5
  > ```
  >
  > [React](https://react.dev/learn/referencing-values-with-refs)
- > You can imagine that inside of React, useRef is implemented like this:
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

Rules:
- "Don’t read or write `ref.current` during rendering. If some information is needed during rendering, use state instead. Since React doesn’t know when `ref.current` changes, even reading it while rendering makes your component’s behavior difficult to predict." ([React](https://react.dev/learn/referencing-values-with-refs))
- "During the first render, the DOM nodes have not yet been created, so `ref.current` will be `null`. And during the rendering of updates, the DOM nodes haven’t been updated yet. So it’s too early to read them. React sets `ref.current` during the commit. Before updating the DOM, React sets the affected `ref.current` values to `null`. After updating the DOM, React immediately sets them to the corresponding DOM nodes.  Usually, you will access refs from event handlers. If you want to do something with a ref, but there is no particular event to do it in, you might need an Effect." ([React](https://react.dev/learn/manipulating-the-dom-with-refs))
- "If you stick to non-destructive actions like focusing and scrolling, you shouldn’t encounter any problems. However, if you try to modify the DOM manually, you can risk conflicting with the changes React is making. . . . Avoid changing DOM nodes managed by React. Modifying, adding children to, or removing children from elements that are managed by React can lead to inconsistent visual results or crashes . . . You can safely modify parts of the DOM that React has *no reason* to update. For example, if some `<div>` is always empty in the JSX, React won’t have a reason to touch its children list. Therefore, it is safe to manually add or remove elements there." ([React](https://react.dev/learn/manipulating-the-dom-with-refs))

Usage:
- > ```jsx
  > import { useRef } from 'react';
  >
  > /* ... */
  > const ref = useRef(0);
  > ```
  >
  > `useRef` returns an object like this:
  > ```jsx
  > {
  >   current: 0 // The value you passed to useRef
  > }
  > ```
  >
  > [React](https://react.dev/learn/referencing-values-with-refs)
- > To access a DOM node managed by React . . . pass your ref [(`const myRef = useRef(null);`)] as the `ref` attribute to the JSX tag for which you want to get the DOM node:
  >
  > ```html
  > <div ref={myRef}>
  > ```
  >
  > [React](https://react.dev/learn/manipulating-the-dom-with-refs)
- > sometimes you might need a ref to each item in the list, and you don’t know how many you will have. . . . You can’t call `useRef` in a loop, in a condition, or inside a `map()` call. . . . solution is to pass a function to the `ref` attribute. This is called a `ref` callback. React will call your `ref` callback with the DOM node when it’s time to set the ref, and with `null` when it’s time to clear it. This lets you maintain your own array or a Map, and access any ref by its index or some kind of ID. . . .
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
  > </ul>
  > /* ... */
  > ```
  >
  > [React](https://react.dev/learn/manipulating-the-dom-with-refs)

Example (from [React](https://react.dev/learn/manipulating-the-dom-with-refs)):

```jsx
import { useRef } from 'react';

export default function Form() {
  const inputRef = useRef(null);

  function handleClick() {
    inputRef.current.focus();
  }

  return (
    <>
      <input ref={inputRef} />
      <button onClick={handleClick}>
        Focus the input
      </button>
    </>
  );
}
```

Use cases:
- "If your component needs to store some value, but it doesn’t impact the rendering logic, choose refs." ([React](https://react.dev/learn/referencing-values-with-refs))
- "sometimes you might need access to the DOM elements managed by React—for example, to focus a node, scroll to it, or measure its size and position. There is no built-in way to do those things in React, so you will need a *ref* to the DOM node." ([React](https://react.dev/learn/manipulating-the-dom-with-refs))
- > Typically, you will use a ref when your component needs to “step outside” React and communicate with external APIs—often a browser API that won’t impact the appearance of the component. Here are a few of these rare situations:
  > - Storing timeout IDs
  > - Storing and manipulating DOM elements, which we cover on the next page
  > - Storing other objects that aren’t necessary to calculate the JSX.
  >
  > [React](https://react.dev/learn/referencing-values-with-refs)

Custom components' refs:
- "When you put a ref on a built-in component that outputs a browser element like `<input />`, React will set that ref’s `current` property to the corresponding DOM node (such as the actual `<input />` in the browser). However, if you try to put a ref on your own component, like `<MyInput />`, by default you will get `null`. . . . This happens because by default React does not let a component access the DOM nodes of other components. Not even for its own children! This is intentional. Refs are an escape hatch that should be used sparingly. Manually manipulating *another* component’s DOM nodes makes your code even more fragile." ([React](https://react.dev/learn/manipulating-the-dom-with-refs))
- "In design systems, it is a common pattern for low-level components like buttons, inputs, and so on, to forward their refs to their DOM nodes. On the other hand, high-level components like forms, lists, or page sections usually won’t expose their DOM nodes to avoid accidental dependencies on the DOM structure." ([React](https://react.dev/learn/manipulating-the-dom-with-refs))
- > components that want to expose their DOM nodes have to opt in to that behavior. A component can specify that it “forwards” its ref to one of its children . . . can use the `forwardRef` API . . .
  > ```jsx
  > import { forwardRef, useRef } from 'react';
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
  >       <button onClick={handleClick}>
  >         Focus the input
  >       </button>
  >     </>
  >   )
  > }
  > ```
  >
  > [React](https://react.dev/learn/manipulating-the-dom-with-refs)
- > Exposing a subset of the API with an imperative handle . . . In uncommon cases, you may want to restrict the exposed functionality. You can do that with `useImperativeHandle`:
  >
  > ```jsx
  > import { forwardRef, useRef, useImperativeHandle } from 'react';
  >
  > const MyInput = forwardRef((props, ref) => {
  >   const restrictedRef = useRef(null);
  >
  >   useImperativeHandle(ref, () => ({
  >     // Only expose focus and nothing else
  >     focus() { restrictedRef.current.focus(); },
  >   }))
  >
  >   return <input {...props} ref={restrictedRef} />;
  > })
  > ```
  >
  > . . . `inputRef.current` inside the `Form` component will only have the `focus` method. In this case, the ref “handle” is not the DOM node, but the custom object you create inside `useImperativeHandle` call.
  >
  > [React](https://react.dev/learn/manipulating-the-dom-with-refs) (with a slightly modified code)

Forcing the DOM to update synchronously:
- > Consider code like this, which adds a new todo and scrolls the screen down to the last child of the list. Notice how, for some reason, it always scrolls to the todo that was just before the last added one . . . The issue is with these two lines:
  > ```jsx
  > setTodos([ ...todos, newTodo]);
  > listRef.current.lastChild.scrollIntoView();
  > ```
  >
  > In React, state updates are queued. Usually, this is what you want. However, here it causes a problem because `setTodos` does not immediately update the DOM. So the time you scroll the list to its last element, the todo has not yet been added. This is why scrolling always “lags behind” by one item. To fix this issue, you can force React to update (“flush”) the DOM synchronously. To do this, import `flushSync` from `react-dom` and wrap the state update into a `flushSync` call:
  >
  > ```jsx
  > flushSync(() => {
  >   setTodos([ ...todos, newTodo]);
  > });
  > listRef.current.lastChild.scrollIntoView();
  > ```
  >
  > This will instruct React to update the DOM synchronously right after the code wrapped in `flushSync` executes. As a result, the last todo will already be in the DOM by the time you try to scroll to it
  >
  > [React](https://react.dev/learn/manipulating-the-dom-with-refs)

# Events

- "By convention, it is common to name event handlers as `handle` followed by the event name. You’ll often see `onClick={handleClick}`, `onMouseEnter={handleMouseEnter}`, and so on." ([React](https://react.dev/learn/responding-to-events))
- "If you use a design system, it’s common for components like buttons to contain styling but not specify behavior. Instead, components like `PlayButton` and `UploadButton` will pass event handlers down." ([React](https://react.dev/learn/responding-to-events))
- "Event handlers will also catch events from any children your component might have. We say that an event “bubbles” or “propagates” up the tree" ([React](https://react.dev/learn/responding-to-events))
- "All events propagate in React except `onScroll`, which only works on the JSX tag you attach it to." ([React](https://react.dev/learn/responding-to-events))
- > In rare cases, you might need to catch all events on child elements, even if they stopped propagation. For example, maybe you want to log every click to analytics, regardless of the propagation logic. You can do this by adding `Capture` at the end of the event name:
  >
  > ```jsx
  > <div onClickCapture={() => { /* this runs first */ }}>
  >   <button onClick={e => e.stopPropagation()} />
  >   <button onClick={e => e.stopPropagation()} />
  > </div>
  > ```
  >
  > . . . Capture events are useful for code like routers or analytics, but you probably won’t use them in app code.
  >
  > [React](https://react.dev/learn/responding-to-events)
- *Passing handlers as alternative to propagation* pattern:
  - "Explicitly calling an event handler prop from a child handler is a good alternative to propagation." ([React](https://react.dev/learn/responding-to-events))
  - > this click handler runs a line of code and then calls the onClick prop passed by the parent:
    >
    > ```jsx
    > function Button({ onClick, children }) {
    >   return (
    >     <button onClick={e => {
    >       e.stopPropagation();
    >       onClick();
    >     }}>
    >       {children}
    >     </button>
    >   );
    > }
    > ```
    >
    > You could add more code to this handler before calling the parent `onClick` event handler, too. This pattern provides an *alternative* to propagation. It lets the child component handle the event, while also letting the parent component specify some additional behavior. Unlike propagation, it’s not automatic. But the benefit of this pattern is that you can clearly follow the whole chain of code that executes as a result of some event.
    >
    > If you rely on propagation and it’s difficult to trace which handlers execute and why, try this approach instead.
    >
    > [React](https://react.dev/learn/responding-to-events)

# State

## Defining State

- "you may notice that `xIsNext === true` when `currentMove` is even and `xIsNext === false` when `currentMove` is odd. In other words, if you know the value of `currentMove`, then you can always figure out what `xIsNext` should be. There’s no reason for you to store both of these in state. In fact, always try to avoid redundant state. Simplifying what you store in state reduces bugs and makes your code easier to understand. Change `Game` so that it doesn’t store `xIsNext` as a separate state variable and instead figures it out based on the `currentMove` . . . You no longer need the `xIsNext` state declaration or the calls to `setXIsNext`. Now, there’s no chance for `xIsNext` to get out of sync with `currentMove`, even if you make a mistake while coding the components." ([React](https://react.dev/learn/tutorial-tic-tac-toe))
- "It is a good idea to have multiple state variables if their state is unrelated . . . But if you find that you often change two state variables together, it might be easier to combine them into one. For example, if you have a form with many fields, it’s more convenient to have a single state variable that holds an object than state variable per field." ([React](https://react.dev/learn/state-a-components-memory))

## Sharing State

- "State is local to a component instance . . . if you render the same component twice, each copy will have completely isolated state" ([React](https://react.dev/learn/state-a-components-memory))
- "state is fully private to the component declaring it. The parent component can’t change it." ([React](https://react.dev/learn/state-a-components-memory))
- "To collect data from multiple children, or to have two child components communicate with each other, declare the shared state in their parent component instead. The parent component can pass that state back down to the children via props. This keeps the child components in sync with each other and with their parent." ([React](https://react.dev/learn/tutorial-tic-tac-toe))
- "Lifting state into a parent component is common when React components are refactored." ([React](https://react.dev/learn/tutorial-tic-tac-toe))

## Data Immutability and Rendering

- "Calling the `setSquares` function lets React know the state of the component has changed. This will trigger a re-render of the components that use the `squares` state (`Board`) as well as its child components (the `Square` components that make up the board)." ([React](https://react.dev/learn/tutorial-tic-tac-toe))
- "Avoiding direct data mutation lets you keep previous versions of the data intact, and reuse them later." ([React](https://react.dev/learn/tutorial-tic-tac-toe))
- "By default, all child components re-render automatically when the state of a parent component changes. . . . Immutability makes it very cheap for components to compare whether their data has changed or not. You can learn more about how React chooses when to re-render a component in the `memo` API reference." ([React](https://react.dev/learn/tutorial-tic-tac-toe))
- "without using the state setting function, React has no idea that object has changed. So React does not do anything in response." ([React](https://react.dev/learn/updating-objects-in-state))
- "When you store objects in state, mutating them will not trigger renders and will change the state in previous render “snapshots”." ([React](https://react.dev/learn/updating-objects-in-state))
- "you shouldn’t change objects that you hold in the React state directly. Instead, when you want to update an object, you need to create a new one (or make a copy of an existing one), and then set the state to use that copy. . . . Mutation is only a problem when you change *existing* objects that are already in state. Mutating an object you’ve just created is okay because *no other code references it yet*. Changing it isn’t going to accidentally impact something that depends on it. This is called a “local mutation”. You can even do local mutation while rendering. Very convenient and completely okay!" ([React](https://react.dev/learn/updating-objects-in-state))
  - > Updating a nested object . . .
    >
    > ```jsx
    > setPerson({
    >   ...person, // Copy other fields
    >   artwork: { // but replace the artwork
    >     ...person.artwork, // with the same one
    >     city: 'New Delhi' // but in New Delhi!
    >   }
    > });
    > ```
    >
    > [React](https://react.dev/learn/updating-objects-in-state)
- "In general, you should only mutate objects that you have just created." ([React](https://react.dev/learn/updating-arrays-in-state))
- "Arrays are mutable in JavaScript, but you should treat them as immutable when you store them in state. Just like with objects, when you want to update an array stored in state, you need to create a new one (or make a copy of an existing one), and then set state to use the new array. . . . In JavaScript, arrays are just another kind of object." ([React](https://react.dev/learn/updating-arrays-in-state))
- "even if you copy an array, you can’t mutate existing items inside of it directly. This is because copying is shallow—the new array will contain the same items as the original one. So if you modify an object inside the copied array, you are mutating the existing state. . . . When updating nested state, you need to create copies from the point where you want to update, and all the way up to the top level." ([React](https://react.dev/learn/updating-arrays-in-state))
- > you should treat arrays in React state as read-only. This means that you shouldn’t reassign items inside an array like `arr[0] = 'bird'`, and you also shouldn’t use methods that mutate the array, such as `push()` and `pop()`. Instead, every time you want to update an array, you’ll want to pass a new array to your state setting function. . . .
  >
  > |         |avoid (mutates the array) |prefer (returns a new array)|
  > |---------|--------------------------|----------------------------|
  > |adding   |`push`, `unshift`         |`concat`, `[...arr]` . . .  |
  > |removing |`pop`, `shift`, `splice`  |`filter`, `slice` . . .     |
  > |replacing|`splice`, `arr[i] =` . . .|`map` . . .                 |
  > |sorting  |`reverse`, `sort`         |copy the array first . . .  |
  >
  > [React](https://react.dev/learn/updating-arrays-in-state)
  - "to remove an item from an array is to *filter it out*. In other words, you will produce a new array that will not contain that item. To do this, use the `filter` method" ([React](https://react.dev/learn/updating-arrays-in-state))
  - "To replace an item, create a new array with `map`. Inside your `map` call, you will receive the item index as the second argument. Use it to decide whether to return the original item (the first argument) or something else" ([React](https://react.dev/learn/updating-arrays-in-state))
  - > to insert an item at a particular position that’s neither at the beginning nor at the end. To do this, you can use the `...` array spread syntax together with the `slice()` method. . . .
    >
    > ```jsx
    > const nextArtists = [
    >   // Items before the insertion point:
    >   ...artists.slice(0, insertAt),
    >   // New item:
    >   { id: nextId++, name: name },
    >   // Items after the insertion point:
    >   ...artists.slice(insertAt)
    > ];
    > ```
    >
    > [React](https://react.dev/learn/updating-arrays-in-state)

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
> let statuses = [
>   'empty',
>   'typing',
>   'submitting',
>   'success',
>   'error',
> ];
>
> export default function App() {
>   return (
>     <>
>       {statuses.map(status => (
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
> 1. **Human inputs**, like clicking a button, typing in a field, navigating a link.
> 2. **Computer inputs**, like a network response arriving, a timeout completing, an image loading.
>
> [React](https://react.dev/learn/reacting-to-input-with-state)

> For the form you’re developing, you will need to change state in response to a few different inputs:
> - **Changing the text input** (human) should switch it from the *Empty* state to the *Typing* state or back, depending on whether the text box is empty or not.
> - **Clicking the Submit button** (human) should switch it to the *Submitting* state.
> - **Successful network response** (computer) should switch it to the *Success* state.
> - **Failed network response** (computer) should switch it to the *Error* state with the matching error message.
>
> [React](https://react.dev/learn/reacting-to-input-with-state)

> To help visualize . . . [the] flow, try drawing each state on paper as a labeled circle, and each change between two states as an arrow. You can sketch out many flows this way and sort out bugs long before implementation.
>
> ![Image](/assets/responding_to_input_flow.webp)
>
> [React](https://react.dev/learn/reacting-to-input-with-state)

## Step 3: Drafting State

"you’ll need to represent the visual states of your component in memory with `useState`. Simplicity is key" ([React](https://react.dev/learn/reacting-to-input-with-state))

"you’ll need to store the `answer` for the input, and the `error` (if it exists) to store the last error . . . Then, you’ll need a state variable representing which one of the visual states that you want to display. There’s usually more than a single way to represent that in memory, so you’ll need to experiment with it. . . . start by adding enough state that you’re *definitely* sure that all the possible visual states are covered . . . Your first idea likely won’t be the best, but that’s ok—refactoring state is a part of the process!" ([React](https://react.dev/learn/reacting-to-input-with-state))

## Step 4: Refactoring State

"Spending a little time on refactoring your state structure will make your components easier to understand, reduce duplication, and avoid unintended meanings." ([React](https://react.dev/learn/reacting-to-input-with-state))

> Here are some questions you can ask about your state variables:
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

# State Structuring

## Overview

> When you write a component that holds some state, you’ll have to make choices about how many state variables to use and what the shape of their data should be. . . . there are a few principles that can guide you to make better choices:
> 1. **Group related state.** If you always update two or more state variables at the same time, consider merging them into a single state variable.
> 2. **Avoid contradictions in state.** When the state is structured in a way that several pieces of state may contradict and “disagree” with each other, you leave room for mistakes. Try to avoid this.
> 3. **Avoid redundant state.** If you can calculate some information from the component’s props or its existing state variables during rendering, you should not put that information into that component’s state.
> 4. **Avoid duplication in state.** When the same data is duplicated between multiple state variables, or within nested objects, it is difficult to keep them in sync. Reduce duplication when you can.
> 5. **Avoid deeply nested state.** Deeply hierarchical state is not very convenient to update. When possible, prefer to structure state in a flat way.
>
> The goal behind these principles is *to make state easy to update without introducing mistakes*.
>
> [React](https://react.dev/learn/choosing-the-state-structure)

## Grouping Related State Variables

- "if some two state variables always change together, it might be a good idea to unify them into a single state variable." ([React](https://react.dev/learn/choosing-the-state-structure))
- "Another case where you’ll group data into an object or an array is when you don’t know how many pieces of state you’ll need. For example, it’s helpful when you have a form where the user can add custom fields." ([React](https://react.dev/learn/choosing-the-state-structure))

## Avoiding State Contradictions

"For example, if you forget to call `setIsSent` and `setIsSending` together, you may end up in a situation where both `isSending` and `isSent` are `true` at the same time. The more complex your component is, the harder it is to understand what happened. Since `isSending` and `isSent` should never be `true` at the same time, it is better to replace them with one `status` state variable" ([React](https://react.dev/learn/choosing-the-state-structure))

## Avoiding State Redundancies

"If you can calculate some information from the component’s props or its existing state variables during rendering, you should not put that information into that component’s state." ([React](https://react.dev/learn/choosing-the-state-structure))

"Don’t mirror props in state . . . if the parent component passes a different value . . . state variable would not be updated! The state is only initialized during the first render. . . . ”Mirroring” props into state only makes sense when you want to ignore all updates for a specific prop. By convention, start the prop name with `initial` or `default` to clarify that its new values are ignored" ([React](https://react.dev/learn/choosing-the-state-structure))

## Avoiding State Duplications

"instead of a `selectedItem` object (which creates a duplication with objects inside `items`), you hold the `selectedId` in state, and then get the `selectedItem` by searching the `items` array for an item with that ID . . . You didn’t need to hold *the selected item* in state, because only the *selected ID* is essential. The rest could be calculated during render." ([React](https://react.dev/learn/choosing-the-state-structure))

## Avoiding Deep State Nesting

"Updating nested state involves making copies of objects all the way up from the part that changed." ([React](https://react.dev/learn/choosing-the-state-structure))

"If the state is too nested to update easily, consider making it “flat” . . . (also known as “normalized”)" ([React](https://react.dev/learn/choosing-the-state-structure))

"Sometimes, you can also reduce state nesting by moving some of the nested state into the child components. This works well for ephemeral UI state that doesn’t need to be stored, like whether an item is hovered." ([React](https://react.dev/learn/choosing-the-state-structure))

# State Management

## Reducers

Definition:
- "named after the `reduce()` operation that you can perform on arrays. . . . The function you pass to `reduce` is known as a “reducer”. It takes the *result so far* and the *current item*, then it returns the *next result*. React reducers are an example of the same idea: they take the *state so far* and the *action*, and return the *next state*. In this way, they accumulate actions over time into state." ([React](https://react.dev/learn/extracting-state-logic-into-a-reducer))
- "Managing state with reducers is slightly different from directly setting state. Instead of telling React “what to do” by setting state, you specify “what the user just did” by dispatching “actions” from your event handlers. (The state update logic will live elsewhere!)" ([React](https://react.dev/learn/extracting-state-logic-into-a-reducer))

Usage:

> To convert from `useState` to `useReducer`:
> 1. Dispatch actions from event handlers.
> 2. Write a reducer function that returns the next state for a given state and action.
> 3. Replace `useState` with `useReducer`.
>
> [React](https://react.dev/learn/extracting-state-logic-into-a-reducer)

Rules:
- Actions:
  - "it should contain the minimal information about *what happened*." ([React](https://react.dev/learn/extracting-state-logic-into-a-reducer))
  - "An action object can have any shape. By convention, it is common to give it a string `type` that describes what happened, and pass any additional information in other fields. The `type` is specific to a component, so in this example either `'added'` or `'added_task'` would be fine. Choose a name that says what happened!" ([React](https://react.dev/learn/extracting-state-logic-into-a-reducer))
  - "**Each action describes a single user interaction, even if that leads to multiple changes in the data.** For example, if a user presses “Reset” on a form with five fields managed by a reducer, it makes more sense to dispatch one reset_form action rather than five separate set_field actions." ([React](https://react.dev/learn/extracting-state-logic-into-a-reducer))
- Reducer:
  - "Because the reducer function takes state . . . as an argument, you can declare it outside of your component. This decreases the indentation level and can make your code easier to read." ([React](https://react.dev/learn/extracting-state-logic-into-a-reducer))
  - "it’s a convention to use `switch` statements inside reducers. . . . We recommend wrapping each case block into the `{` and `}` curly braces so that variables declared inside of different `case`s don’t clash with each other." ([React](https://react.dev/learn/extracting-state-logic-into-a-reducer))
  - "**Reducers must be pure.** Similar to state updater functions, reducers run during rendering! (Actions are queued until the next render.) This means that reducers must be pure—same inputs always result in the same output. They should not send requests, schedule timeouts, or perform any side effects (operations that impact things outside the component). They should update objects and arrays without mutations." ([React](https://react.dev/learn/extracting-state-logic-into-a-reducer))

Example (taken from [React](https://react.dev/learn/extracting-state-logic-into-a-reducer)):

```jsx
import { useReducer } from 'react';
import AddTask from './AddTask.js';
import TaskList from './TaskList.js';

export default function TaskApp() {
  const [tasks, dispatch] = useReducer(tasksReducer, initialTasks);

  function handleAddTask(text) {
    dispatch({
      type: 'added',
      id: nextId++,
      text: text,
    });
  }

  function handleChangeTask(task) {
    dispatch({
      type: 'changed',
      task: task,
    });
  }

  function handleDeleteTask(taskId) {
    dispatch({
      type: 'deleted',
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
    case 'added': {
      return [
        ...tasks,
        {
          id: action.id,
          text: action.text,
          done: false,
        },
      ];
    }
    case 'changed': {
      return tasks.map((t) => {
        if (t.id === action.task.id) {
          return action.task;
        } else {
          return t;
        }
      });
    }
    case 'deleted': {
      return tasks.filter((t) => t.id !== action.id);
    }
    default: {
      throw Error('Unknown action: ' + action.type);
    }
  }
}

let nextId = 3;
const initialTasks = [
  {id: 0, text: 'Visit Kafka Museum', done: true},
  {id: 1, text: 'Watch a puppet show', done: false},
  {id: 2, text: 'Lennon Wall pic', done: false},
];
```

Use cases:
- General:
  - "It’s a matter of preference. You can always convert between `useState` and `useReducer` back and forth: they are equivalent!" ([React](https://react.dev/learn/extracting-state-logic-into-a-reducer))
  - "We recommend using a reducer if you often encounter bugs due to incorrect state updates in some component, and want to introduce more structure to its code." ([React](https://react.dev/learn/extracting-state-logic-into-a-reducer))
  - "You don’t have to use reducers for everything: feel free to mix and match! You can even `useState` and `useReducer` in the same component." ([React](https://react.dev/learn/extracting-state-logic-into-a-reducer))
- Code maintainability:
  - "`useReducer` can help cut down on the code if many event handlers modify state in a similar way." ([React](https://react.dev/learn/extracting-state-logic-into-a-reducer))
  - "when the state updates . . . get more complex, they can bloat your component’s code and make it difficult to scan. In this case, `useReducer` lets you cleanly separate the `how` of update logic from the `what happened` of event handlers." ([React](https://react.dev/learn/extracting-state-logic-into-a-reducer))
  - "Component logic can be easier to read when you separate concerns like this. Now the event handlers only specify *what happened* by dispatching actions, and the reducer function determines *how the state updates* in response to them." ([React](https://react.dev/learn/extracting-state-logic-into-a-reducer))
- Debugging:
  - "When you have a bug with `useState`, it can be difficult to tell *where* the state was set incorrectly, and *why*. With `useReducer`, you can add a console log into your reducer to see every state update, and *why* it happened (due to which `action`). If each `action` is correct, you’ll know that the mistake is in the reducer logic itself." ([React](https://react.dev/learn/extracting-state-logic-into-a-reducer))
- Testing:
  - "A reducer is a pure function that doesn’t depend on your component. This means that you can export and test it separately in isolation. While generally it’s best to test components in a more realistic environment, for complex state update logic it can be useful to assert that your reducer returns a particular state for a particular initial state and action." ([React](https://react.dev/learn/extracting-state-logic-into-a-reducer))

## Context

Definition:
- "Context lets the parent component make some information available to any component in the tree below it—no matter how deep—without passing it explicitly through props." ([React](https://react.dev/learn/passing-data-deeply-with-context))
- "Context lets you write components that “adapt to their surroundings” and display themselves differently depending on *where* (or, in other words, *in which context*) they are being rendered." ([React](https://react.dev/learn/passing-data-deeply-with-context))

- Operation:
  - "How context works might remind you of CSS property inheritance. In CSS, you can specify `color: blue` for a `<div>`, and any DOM node inside of it, no matter how deep, will inherit that color unless some other DOM node in the middle overrides it with `color: green`. Similarly, in React, the only way to override some context coming from above is to wrap children into a context provider with a different value." ([React](https://react.dev/learn/passing-data-deeply-with-context))
  - "different React contexts don’t override each other. Each context that you make with `createContext()` is completely separate from other ones . . . One component may use or provide many different contexts without a problem." ([React](https://react.dev/learn/passing-data-deeply-with-context))
  - "Context is not limited to static values. If you pass a different value on the next render, React will update all the components reading it below! This is why context is often used in combination with state." ([React](https://react.dev/learn/passing-data-deeply-with-context))

Example (slightly modified version of the code from [React](https://react.dev/learn/passing-data-deeply-with-context)):

```jsx
/* LevelContext.js */

import { createContext } from 'react';

export const LevelContext = createContext(0);
```

```jsx
/* Heading.js */

import { useContext } from 'react';
import { LevelContext } from './LevelContext.js';

export default function Heading({ children }) {
  const level = useContext(LevelContext);

  switch(level) {
    case 0:
      throw Error('Heading must be inside a Section!');
    case 1:
      return <h1>{children}</h1>
    case 2:
      return <h2>{children}</h2>
    case 3:
      return <h3>{children}</h3>
    default:
      throw Error('Unknown level');
  }
}
```

```jsx
/* Section.js */

import { useContext } from 'react';
import { LevelContext } from './LevelContext.js';

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
/* App.js */

import Heading from './Heading.js';
import Section from './Section.js';

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

Use cases:
- General:
  - "The nearest common ancestor could be far removed from the components that need data, and lifting state up that high can lead to a situation called “prop drilling”." ([React](https://react.dev/learn/passing-data-deeply-with-context))
  - "In general, if some information is needed by distant components in different parts of the tree, it’s a good indication that context will help you." ([React](https://react.dev/learn/passing-data-deeply-with-context))
- "**Theming:** If your app lets the user change its appearance (e.g. dark mode), you can put a context provider at the top of your app, and use that context in components that need to adjust their visual look." ([React](https://react.dev/learn/passing-data-deeply-with-context))
- "**Current account:** Many components might need to know the currently logged in user. Putting it in context makes it convenient to read it anywhere in the tree. Some apps also let you operate multiple accounts at the same time (e.g. to leave a comment as a different user). In those cases, it can be convenient to wrap a part of the UI into a nested provider with a different current account value." ([React](https://react.dev/learn/passing-data-deeply-with-context))
- "**Routing:** Most routing solutions use context internally to hold the current route. This is how every link “knows” whether it’s active or not. If you build your own router, you might want to do it too." ([React](https://react.dev/learn/passing-data-deeply-with-context))
- "**Managing state:** As your app grows, you might end up with a lot of state closer to the top of your app. Many distant components below may want to change it. It is common to use a reducer together with context to manage complex state and pass it down to distant components without too much hassle." ([React](https://react.dev/learn/passing-data-deeply-with-context))

Alternatives:

> Just because you need to pass some props several levels deep doesn’t mean you should put that information into context.  Here’s a few alternatives you should consider before using context:
> 1. **Start by passing props.** If your components are not trivial, it’s not unusual to pass a dozen props down through a dozen components. It may feel like a slog, but it makes it very clear which components use which data! The person maintaining your code will be glad you’ve made the data flow explicit with props.
> 2. **Extract components and pass JSX as children to them.** If you pass some data through many layers of intermediate components that don’t use that data (and only pass it further down), this often means that you forgot to extract some components along the way. For example, maybe you pass data props like `posts` to visual components that don’t use them directly, like `<Layout posts={posts} />`. Instead, make `Layout` take `children` as a prop, and render `<Layout><Posts posts={posts} /></Layout>`. This reduces the number of layers between the component specifying the data and the one that needs it.
>
> If neither of these approaches works well for you, consider context.
>
> [React](https://react.dev/learn/passing-data-deeply-with-context)

## Reducers With Context

"You can combine reducer with context to let any component read and update state above it . . . to manage state of a complex screen." ([React](https://react.dev/learn/scaling-up-with-reducer-and-context))

"You can have many context-reducer pairs like this in your app." ([React](https://react.dev/learn/scaling-up-with-reducer-and-context))

> To provide state and the dispatch function to components below:
> 1. Create two contexts (for state and for dispatch functions).
> 2. Provide both contexts from the component that uses the reducer.
> 3. Use either context from components that need to read them.
>
> [React](https://react.dev/learn/scaling-up-with-reducer-and-context)

> You can further declutter the components by moving all wiring into one file.
> - You can export a component like `TasksProvider` that provides context.
> - You can also export custom Hooks like `useTasks` and `useTasksDispatch` to read it.
>
> [React](https://react.dev/learn/scaling-up-with-reducer-and-context)

Example (slightly modified version of the code from [React](https://react.dev/learn/scaling-up-with-reducer-and-context)):

```jsx
/* TasksContext.js */

import { createContext, useContext, useReducer } from 'react';

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
  switch(action.type) {
    case 'added': {
      return [...tasks, {
        id: action.id,
        text: action.text,
        done: false
      }];
    }
    case 'changed': {
      return tasks.map(t => {
        if (t.id === action.task.id) {
          return action.task;
        } else {
          return t;
        }
      });
    }
    case 'deleted': {
      return tasks.filter(t => t.id !== action.id);
    }
    default: {
      throw Error('Unknown action: ' + action.type);
    }
  }
}

const initialTasks = [
  { id: 0, text: 'Philosopher’s Path', done: true },
  { id: 1, text: 'Visit the temple', done: false },
  { id: 2, text: 'Drink matcha', done: false }
];
```

```jsx
/* AddTask.js */

/* ... */
import { useTasksDispatch } from './TasksContext.js';

export default function AddTask() {
  /* ... */
  const dispatch = useTasksDispatch();
  /* ... */
}
```

```jsx
/* App.js */

import AddTask from './AddTask.js';
import TaskList from './TaskList.js';
import { TasksProvider } from './TasksContext.js';

export default function TaskApp() {
  return (
    <TasksProvider>
      <AddTask />
      <TaskList />
    </TasksProvider>
  );
}
```

# Effects

Definition:
- "in this text, capitalized “Effect” refers to the React-specific definition . . . i.e. a side effect caused by rendering. To refer to the broader programming concept, we’ll say “side effect”." ([React](https://react.dev/learn/synchronizing-with-effects))
- "Effects let you specify side effects that are caused by rendering itself, rather than by a particular event. Sending a message in the chat is an *event* because it is directly caused by the user clicking a specific button. However, setting up a server connection is an *Effect* because it should happen no matter which interaction caused the component to appear." ([React](https://react.dev/learn/synchronizing-with-effects))
- "Effects let you run some code after rendering so that you can synchronize your component with some system outside of React. . . . Effects run at the end of a commit after the screen updates. This is a good time to synchronize the React components with some external system (like network or a third-party library)." ([React](https://react.dev/learn/synchronizing-with-effects))
- "Every time your component renders, React will update the screen *and then* run the code inside `useEffect`. In other words, `useEffect` “delays” a piece of code from running until that render is reflected on the screen. . . . By wrapping the DOM update in an Effect, you let React update the screen first. Then your Effect runs." ([React](https://react.dev/learn/synchronizing-with-effects))

Usage (based on [React](https://react.dev/learn/synchronizing-with-effects)):
- ```jsx
  import { useEffect } from 'react';

  export default function Component() {
    useEffect(() => {
      /* ... */
    });

    return <section>...</section>;
  }
  ```
- ```jsx
  useEffect(() => {
    // This runs after every render
  });

  useEffect(() => {
    // This runs only on mount (when the component appears)
  }, []);

  useEffect(() => {
    // This runs on mount *and also* if either a or b have changed since the last render
  }, [a, b]);
  ```
  - "React compares the dependency values using the `Object.is` comparison." ([React](https://react.dev/learn/synchronizing-with-effects))
  - "Notice that you can’t “choose” your dependencies. You will get a lint error if the dependencies you specified don’t match what React expects based on the code inside your Effect. This helps catch many bugs in your code. If you don’t want some code to re-run, edit the Effect code itself to not “need” that dependency." ([React](https://react.dev/learn/synchronizing-with-effects))
    - > ```jsx
      > Lint Error
      > 14:6 - React Hook useEffect has a missing dependency: 'isPlaying'. Either include it or remove the dependency array.
      > ```
      >
      > [React](https://react.dev/learn/synchronizing-with-effects)
  - "the `ref` object has a stable identity: React guarantees you’ll always get the same object from the same `useRef` call on every render. It never changes, so it will never by itself cause the Effect to re-run. Therefore, it does not matter whether you include it or not. Including it is fine too . . . The `set` functions returned by `useState` also have stable identity, so you will often see them omitted from the dependencies too. If the linter lets you omit a dependency without errors, it is safe to do. Omitting always-stable dependencies only works when the linter can “see” that the object is stable. For example, if `ref` was passed from a parent component, you would have to specify it in the dependency array." ([React](https://react.dev/learn/synchronizing-with-effects))
  - Cleanup function:
    - "Imagine the `ChatRoom` component is a part of a larger app with many different screens. The user starts their journey on the `ChatRoom` page. The component mounts and calls `connection.connect()`. Then imagine the user navigates to another screen—for example, to the `Settings` page. The `ChatRoom` component unmounts. Finally, the user clicks Back and `ChatRoom` mounts again. This would set up a second connection—but the first connection was never destroyed! As the user navigates across the app, the connections would keep piling up. Bugs like this are easy to miss without extensive manual testing. To help you spot them quickly, in development React remounts every component once immediately after its initial mount. Seeing the `"✅ Connecting..."` log twice helps you notice the real issue: your code doesn’t close the connection when the component unmounts." ([React](https://react.dev/learn/synchronizing-with-effects))
    - > ```jsx
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
    - "React will call your cleanup function each time before the Effect runs again, and one final time when the component unmounts" ([React](https://react.dev/learn/synchronizing-with-effects))
    - > Now you get three console logs in development:
      > ```
      > "✅ Connecting..."
      > "❌ Disconnected."
      > "✅ Connecting..."
      > ```
      >
      > This is the correct behavior in development. By remounting your component, React verifies that navigating away and back would not break your code. . . . There’s an extra connect/disconnect call pair because React is probing your code for bugs in development. This is normal—don’t try to make it go away! In production, you would only see `"✅ Connecting..."` printed once. Remounting components only happens in development to help you find Effects that need cleanup. You can turn off Strict Mode to opt out of the development behavior, but we recommend keeping it on.
      >
      > [React](https://react.dev/learn/synchronizing-with-effects)
    - > Sometimes you need to add UI widgets that aren’t written to React. For example, let’s say you’re adding a map component to your page. It has a `setZoomLevel()` method, and you’d like to keep the zoom level in sync with a `zoomLevel` state variable in your React code. Your Effect would look similar to this:
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

Use cases:
- "Keep in mind that Effects are typically used to “step out” of your React code and synchronize with some *external* system. This includes browser APIs, third-party widgets, network, and so on. If your Effect only adjusts some state based on other state, you might not need an Effect." ([React](https://react.dev/learn/synchronizing-with-effects))
- "You can use a similar approach to wrap legacy non-React code (like jQuery plugins) into declarative React components." ([React](https://react.dev/learn/synchronizing-with-effects))
