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
    - "JSX elements aren’t “instances” because they don’t hold any internal state and aren’t real DOM nodes." ([React](https://react.dev/learn/conditional-rendering))
  - `<Fragment>`:
    - "empty tag is called a *Fragment*. Fragments let you group things without leaving any trace in the browser HTML tree." ([React](https://react.dev/learn/writing-markup-with-jsx))
    - "What do you do when each item needs to render not one, but several DOM nodes? The short `<>...</>` Fragment syntax won’t let you pass a key, so you need to either group them into a single `<div>`, or use the slightly longer and more explicit `<Fragment>` syntax . .  . Fragments disappear from the DOM, so this will produce a flat list" ([React](https://react.dev/learn/rendering-lists))
- Props:
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
- Purity:
  - > a pure function is a function with the following characteristics:
    > - **It minds its own business.** It does not change any objects or variables that existed before it was called.
    > - **Same inputs, same output.** Given the same inputs, a pure function should always return the same result.
    >
    > [React](https://react.dev/learn/keeping-components-pure)
  - "React assumes that every component you write is a pure function. This means that React components you write must always return the same JSX given the same inputs" ([React](https://react.dev/learn/keeping-components-pure))
  - "Components should only *return* their JSX, and not *change* any objects or variables that existed before rendering—that would make them impure!" ([React](https://react.dev/learn/keeping-components-pure))
  - "Pure functions don’t mutate variables outside of the function’s scope or objects that were created before the call—that makes them impure!  However, it’s completely fine to change variables and objects that you’ve *just* created while rendering. . . . it’s fine because you’ve created them during the same render . . . No code outside of [it] . . . will ever know that this happened. This is called “local mutation”—it’s like your component’s little secret." ([React](https://react.dev/learn/keeping-components-pure))
  - `StrictMode`:
    - "You should never change preexisting variables or objects while your component is rendering. React offers a “Strict Mode” in which it calls each component’s function twice during development. By calling the component functions twice, Strict Mode helps find components that break these rules. . . . Pure functions only calculate, so calling them twice won’t change anything" ([React](https://react.dev/learn/keeping-components-pure))
    - "Strict Mode has no effect in production, so it won’t slow down the app for your users." ([React](https://react.dev/learn/keeping-components-pure))
    - "To opt into Strict Mode, you can wrap your root component into `<React.StrictMode>`" ([React](https://react.dev/learn/keeping-components-pure))
  - Side effects:
    - "changes—updating the screen, starting an animation, changing the data—are called **side effects**. They’re things that happen *“on the side”*, not during rendering." ([React](https://react.dev/learn/keeping-components-pure))
    - "In React, side effects usually belong inside event handlers. . . . Even though event handlers are defined *inside* your component, they don’t run *during* rendering! So event handlers don’t need to be pure." ([React](https://react.dev/learn/keeping-components-pure))
    - "If you’ve exhausted all other options and can’t find the right event handler for your side effect, you can still attach it to your returned JSX with a `useEffect` call in your component. This tells React to execute it later, after rendering, when side effects are allowed. However, this approach should be your last resort. When possible, try to express your logic with rendering alone." ([React](https://react.dev/learn/keeping-components-pure))
- Returned value:
  - "In some situations, you won’t want to render anything at all. . . . A component must return something. In this case, you can return `null` . . . In practice, returning `null` from a component isn’t common . . . More often, you would conditionally include or exclude the component in the parent component’s JSX." ([React](https://react.dev/learn/conditional-rendering))
- The render tree:
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
- The module dependency tree:
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
      - If the first operand of the `&&` operator after the automatic `Boolean` conversion becomes `false` but itself is not strictly `false` nor `null`/`undefined`, it will be rendered by React - examples include `0`, `NaN`, or `""` (see [React](https://react.dev/learn/conditional-rendering))
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
