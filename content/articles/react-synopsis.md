---
title: React synopsis
---


## React

<br /> 

### SPA’s

* [Mozilla](https://developer.mozilla.org/en-US/docs/Glossary/SPA)

<br />

* An SPA (Single-page application) is a web app implementation that loads only a single web document, and then updates the body content of that single document via JavaScript APIs such as XMLHttpRequest and Fetch when different content is to be shown.
* This therefore allows users to use websites without loading whole new pages from the server, which can result in performance gains and a more dynamic experience, with some tradeoff disadvantages such as SEO, more effort required to maintain state, implement navigation, and do meaningful performance monitoring.
* A benefit is that you only transfer the minimum amount of data needed to make the update changes after the main page has been loaded. The main page loads all the content (js, css, etc) that the SPA needs to run so the initial page load can take longer, but after that it is very fast.


<br />

### Declarative vs. Imperative

<br />

#### Imperative

* With imperative programming, you tell the compiler what you want to happen, step by step.

<br />

#### Declarative

* With declarative programming, on the other hand, you write code that describes what you want, but not necessarily how to get it (declare your desired results, but not the step-by-step):
* Some well-known examples of declarative domain specific languages (DSLs) include CSS, regular expressions, and a subset of SQL (SELECT queries, for example) Many markup languages such as HTML, MXML, XAML, XSLT


<br />

### File structure patterns

<br />

#### Atomic design





<br />

### Component react patterns

<br />

#### Smart & dumb

* One way we can organise our app is into smart and dumb components (a.k.a. container & presentational components). This seperates complex logic from UI.
* Dumb components are small, take an input, do minimal work and then return some UI output. They are easy to test and reason about.
* Presentational components utilize props, render, and context (stateless API’s) and can be the syntactically-pretty functional, stateless component:
* Presentational components receive data and callbacks from props only, which can be provided by its container or parent component.
* Smart components reside at the top level and do most of the heavy lifting. In PM UI we are somewhat following this pattern and have smart components at the top level that fetch data, parse the JSON response, etc. (e.g. FeedsPage.jsx)
* Containers are your data or logic layer and utilize stateful API’s. Using lifecycle events, you can connect to a state management store such as Redux or Flux, and pass down data and callbacks as props to children components. In the container’s render method is where you compose your UI consisting of presentational children components. In order to have access to all stateful API’s, a container must be a class component as opposed to a functional component.
* Tip: Most of these type of components can currently be found in the 'router' directory in PM UI whilst "dumb" components are mainly found in the 'components' directory

<br />

#### It do go down

* Data flows "down" in React. A component may choose to pass its state down as props to its child components
* Any state is always owned by some specific component, and any data or UI derived from that state can only affect components below them in the tree. Never up. This is called uniderectional data flow
* So how can components affect one another and not just their children? These componenets must share some common ancestorage. The parent component can pass a function as a property into a child component. When the function is called from the child component it can affect the parent component and so also affect a different child.

<br />

#### Conditional rendering


* Render a certain JSX code based on the state.
* Conditional rendering is very useful as it allows you to create distinct components based on your needs and then render only the ones that are required by the application.

<br />

#### Render props

* technique for sharing code between React components using a prop whose value is a function
* Render props is when you have a prop that is a function which returns JSX. This of course should work for function components because aside from life cycle methods there really isnt much that is different than class components.

<br />

#### Controlled components

* The controlled component takes the state through props. It can notify any changes by means of callbacks like onChange.
* Parent components can control it by handling the callback and managing its own state meanwhile, the new values are passed to the controlled component as props.
* `<input type = "text" value = {value} onChange = {handleChange} />`


<br />

### One Way Data Flow

* You typically pass down callbacks from parent components to child components as props. When the state changes in any of the child components, it invokes that callback and passes whatever data is appropriate in each use case. Your "controller-view" (the root component that implements the actual callback) then does whatever business logic you need based on the current state and then updates its state accordingly (causing a re-render of the component tree from that component down).
* In general this concept means that data has one, and only one, way to be transferred to other parts of the application.
  * state is passed to the view and to child components
  * actions are triggered by the view
  * actions can update the state
  * the state change is passed to the view and to child components

<br />

### React is a UI Library

<br />

## React app setup

<br />

### create-react-app

[docs](https://create-react-app.dev/docs/getting-started)

* Create React App is an officially supported way to create single-page React applications. It offers a modern build setup with no configuration.
* Features:
  * A recommended starting folder structure
  * A solid build setup with webpack and Babel (that you don't have to worry about setting up)
  * Scripts to run our React application
  * Scripts to bundle for production
  * Scrips and build for running tests
  * Ability to extend with Sass, TypeScript, and more

<br />

## JSX

<br />

### Dynamic content

```jsx
function App() {
  const [data, setData] = useState([
    { id: 1 }
  ])
  
  
  return (
    <div className="App">
      {data.map(block => <ComponentFoo  key={block.id} block={block} ></ComponentFoo> )}
    </div>
  )
}
```

<br />

### Rendering cycle

* mounting, updating and unmounting.

<br />

### React DOM

* ReactDOM is the glue between React and the DOM. Often, you will only use it for one single thing: mounting with ReactDOM.render(). Another useful feature of ReactDOM is ReactDOM.findDOMNode() which you can use to gain direct access to a DOM element. (Something you should use sparingly in React apps, but it can be necessary.)

<br />


### JSX Expressions

[Introducing JSX](https://reactjs.org/docs/introducing-jsx.html)

<br />

#### Specifying Attributes with JSX

```jsx
const element = <div tabIndex="0"></div>;
const element = <img src={user.avatarUrl}></img>;
```

<br />

#### JSX is an Expression Too

* After compilation, JSX expressions become regular JavaScript function calls and evaluate to JavaScript objects. This means that you can use JSX inside of if statements and for loops, assign it to variables, accept it as arguments, and return it from functions:

<br />

```jsx
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```

<br />

### JSX Attributes

<br />

* [JSX in depth](https://reactjs.org/docs/jsx-in-depth.html)

<br />

* JSX just provides syntactic sugar for the `React.createElement(component, props, ...children)`
* Capitalized types indicate that the JSX tag is referring to a React component. These tags get compiled into a direct reference to the named variable, so if you use the JSX <Foo /> expression, Foo must be in scope.
* User-Defined Components Must Be Capitalized
* Booleans, Null, and Undefined Are Ignored

<br />

#### Using dot notation

```jsx
import React from 'react';

const MyComponents = {
  DatePicker: function DatePicker(props) {
    return <div>Imagine a {props.color} datepicker here.</div>;
  }
}

function BlueDatePicker() {
  return <MyComponents.DatePicker color="blue" />;
}

```

<br />

### Choosing the type at runtime

```jsx
import React from 'react';
import { PhotoStory, VideoStory } from './stories';

const components = {
  photo: PhotoStory,
  video: VideoStory
};

function Story(props) {
  // Correct! JSX type can be a capitalized variable.
  const SpecificStory = components[props.storyType];
  return <SpecificStory story={props.story} />;
}
```

<br />

#### Javascript expressions as props

```jsx
<MyComponent foo={1 + 2 + 3 + 4} />

```

<br />


#### Strings

```jsx
<MyComponent message="hello world" />

<MyComponent message={'hello world'} />

```

<br />


#### Props default to true

```jsx
<MyTextBox autocomplete />

<MyTextBox autocomplete={true} />

```

<br />


#### Spread attributes

```jsx
function App1() {
  return <Greeting firstName="Ben" lastName="Hector" />;
}

function App2() {
  const props = {firstName: 'Ben', lastName: 'Hector'};
  return <Greeting {...props} />;
}
```

<br />

#### Children

```jsx


// list
function Item(props) {
  return <li>{props.message}</li>;
}

function TodoList() {
  const todos = ['finish doc', 'submit pr', 'nag dan to review'];
  return (
    <ul>
      {todos.map((message) => <Item key={message} message={message} />)}
    </ul>
  );
}

// String
<MyComponent>foo</MyComponent>

<MyComponent>{'foo'}</MyComponent>


// Function
// Calls the children callback numTimes to produce a repeated component
function Repeat(props) {
  let items = [];
  for (let i = 0; i < props.numTimes; i++) {
    items.push(props.children(i));
  }
  return <div>{items}</div>;
}

function ListOfTenThings() {
  return (
    <Repeat numTimes={10}>
      {(index) => <div key={index}>This is item {index} in the list</div>}
    </Repeat>
  );
}
```

<br />


## Class components

<br />

### Lifecycle methods

* Mounting
  * constructor()
  * static getDerivedStateFromProps()
    * getDerivedStateFromProps is invoked right before calling the render method, both on the initial mount and on subsequent updates. It should return an object to update the state, or null to update nothing.
    * This method exists for rare use cases where the state depends on changes in props over time. For example, it might be handy for implementing a <Transition> component that compares its previous and next children to decide which of them to animate in and out.
  * render()
  * componentDidMount()
* Updating
  * static getDerivedStateFromProps()
  * shouldComponentUpdate()
  * render()
  * getSnapshotBeforeUpdate()
  *  componentDidUpdate()
* Unmounting
  * componentWillUnmount()
* Error handling
  * static getDerivedStateFromError()
  * componentDidCatch()
* Other
  * setState()
  * forceUpdate()


<br />

### Props

* Props stand for Properties and they are  Read-Only
* It is an object which stores the value of attributes of a tag and work similar to the HTML attributes. It gives a way to pass data from one component to other components. It is similar to function arguments. Props are passed to the component in the same way as arguments passed in a function.
* All React components must act like pure functions with respect to their props.
* Props are immutable so we cannot modify the props from inside the component. Inside the components, we can add attributes called props. These attributes are available in the component as this.props and can be used to render dynamic data in our render method.


<br />

### State

* The state is an instance of React Component Class can be defined as an object of a set of observable properties that control the behavior of the component. In other words, the State of a component is an object that holds some information that may change over the lifetime of the component.

<br />

## Hooks

[Docs](https://reactjs.org/docs/hooks-reference.html)

<br />

* Hooks are functions that let you “hook into” React state and lifecycle features from function components. Hooks don’t work inside classes — they let you use React without classes.
* Benefits:
  * No need to learn, memorize, implement lifecycle methods
  * Complex components become hard to understand
  * Composable, Reusable Logic

<br />

### Rule of hooks

* Only Call Hooks at the Top Level
  * Don’t call Hooks inside loops, conditions, or nested functions
* Only Call Hooks from React Functions:
  * Call Hooks from React function components.
  * Call Hooks from custom Hooks (we’ll learn about them on the next page).

<br />


### useState

* Returns a stateful value, and a function to update it.
* During the initial render, the returned state (state) is the same as the value passed as the first argument (initialState).
* The setState function is used to update the state. It accepts a new state value and enqueues a re-render of the component.
* During subsequent re-renders, the first value returned by useState will always be the most recent state after applying updates.
* If your update function returns the exact same value as the current state, the subsequent rerender will be skipped completely.


<br />

#### Functional updates

* If the new state is computed using the previous state, you can pass a function to setState. The function will receive the previous value, and return an updated value. Here’s an example of a counter component that uses both forms of setState

```jsx
function Counter({initialCount}) {
  const [count, setCount] = useState(initialCount);
  return (
    <>
      Count: {count}
      <button onClick={() => setCount(initialCount)}>Reset</button>
      <button onClick={() => setCount(prevCount => prevCount - 1)}>-</button>
      <button onClick={() => setCount(prevCount => prevCount + 1)}>+</button>
    </>
  );
}
```

<br />

#### Lazy initial state

* The initialState argument is the state used during the initial render. In subsequent renders, it is disregarded. If the initial state is the result of an expensive computation, you may provide a function instead, which will be executed only on the initial render:

```jsx
const [state, setState] = useState(() => {
  const initialState = someExpensiveComputation(props);
  return initialState;
});
```


<br />

### useEffect

* Mutations, subscriptions, timers, logging, and other side effects are not allowed inside the main body of a function component (referred to as React’s render phase). Doing so will lead to confusing bugs and inconsistencies in the UI. Instead, use useEffect. The function passed to useEffect will run after the render is committed to the screen. Think of effects as an escape hatch from React’s purely functional world into the imperative world.  By default, effects run after every completed render, but you can choose to fire them only when certain values have changed.
* If you want to run an effect and clean it up only once (on mount and unmount), you can pass an empty array ([]) as a second argument. This tells React that your effect doesn’t depend on any values from props or state, so it never needs to re-run. If you pass an empty array ([]), the props and state inside the effect will always have their initial values. While passing [] as the second argument is closer to the familiar componentDidMount and componentWillUnmount mental model.


<br />

#### Cleaning up an effect
* Often, effects create resources that need to be cleaned up before the component leaves the screen, such as a subscription or timer ID. To do this, the function passed to useEffect may return a clean-up function. For example, to create a subscription:
```jsx
useEffect(() => {
  const subscription = props.source.subscribe();
  return () => {
    // Clean up the subscription
    subscription.unsubscribe();
  };
});
```
* The clean-up function runs before the component is removed from the UI to prevent memory leaks. Additionally, if a component renders multiple times (as they typically do), the previous effect is cleaned up before executing the next effect. In our example, this means a new subscription is created on every update. To avoid firing an effect on every update, refer to the next section.

<br />

#### Timing of effects

* Unlike componentDidMount and componentDidUpdate, the function passed to useEffect fires after layout and paint, during a deferred event. This makes it suitable for the many common side effects, like setting up subscriptions and event handlers, because most types of work shouldn’t block the browser from updating the screen.
* However, not all effects can be deferred. For example, a DOM mutation that is visible to the user must fire synchronously before the next paint so that the user does not perceive a visual inconsistency. (The distinction is conceptually similar to passive versus active event listeners.) For these types of effects, React provides one additional Hook called useLayoutEffect. It has the same signature as useEffect, and only differs in when it is fired.


```jsx
useEffect(
  () => {
    const subscription = props.source.subscribe();
    return () => {
      subscription.unsubscribe();
    };
  },
  [props.source],
);
```

<br />


#### Conditionally firing an effect

* The default behavior for effects is to fire the effect after every completed render. That way an effect is always recreated if one of its dependencies changes.
* pass a second argument to useEffect that is the array of values that the effect depends on.

<br />


### useContext

* Accepts a context object (the value returned from React.createContext) and returns the current context value for that context. The current context value is determined by the value prop of the nearest <MyContext.Provider> above the calling component in the tree.
* When the nearest <MyContext.Provider> above the component updates, this Hook will trigger a rerender with the latest context value passed to that MyContext provider. Even if an ancestor uses React.memo or shouldComponentUpdate, a rerender will still happen starting at the component itself using useContext.
* Don’t forget that the argument to useContext must be the context object itself:
  * Correct: useContext(MyContext)
  * Incorrect: useContext(MyContext.Consumer)
  * Incorrect: useContext(MyContext.Provider)
* A component calling useContext will always re-render when the context value changes. If re-rendering the component is expensive, you can optimize it by using memoization.

<br />

```jsx
const themes = {
  light: {
    foreground: "#000000",
    background: "#eeeeee"
  },
  dark: {
    foreground: "#ffffff",
    background: "#222222"
  }
};

const ThemeContext = React.createContext(themes.light);

function App() {
  return (
    <ThemeContext.Provider value={themes.dark}>
      <Toolbar />
    </ThemeContext.Provider>
  );
}

function Toolbar(props) {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}

function ThemedButton() {
  const theme = useContext(ThemeContext);
  return (
    <button style={{ background: theme.background, color: theme.foreground }}>
      I am styled by theme context!
    </button>
  );
}
```

<br />

### useReducer

* useReducer is usually preferable to useState when you have complex state logic that involves multiple sub-values or when the next state depends on the previous one. useReducer also lets you optimize performance for components that trigger deep updates because you can pass dispatch down instead of callbacks.

```jsx
const initialState = {count: 0};

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return {count: state.count + 1};
    case 'decrement':
      return {count: state.count - 1};
    default:
      throw new Error();
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);
  return (
    <>
      Count: {state.count}
      <button onClick={() => dispatch({type: 'decrement'})}>-</button>
      <button onClick={() => dispatch({type: 'increment'})}>+</button>
    </>
  );
}
```

<br />

#### Lazy initialization

* You can also create the initial state lazily. To do this, you can pass an init function as the third argument. The initial state will be set to init(initialArg).
* It lets you extract the logic for calculating the initial state outside the reducer. This is also handy for resetting the state later in response to an action:

```jsx
function init(initialCount) {
  return {count: initialCount};
}

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return {count: state.count + 1};
    case 'decrement':
      return {count: state.count - 1};
    case 'reset':
      return init(action.payload);
    default:
      throw new Error();
  }
}

function Counter({initialCount}) {
  const [state, dispatch] = useReducer(reducer, initialCount, init);
  return (
    <>
      Count: {state.count}
      <button
        onClick={() => dispatch({type: 'reset', payload: initialCount})}>
        Reset
      </button>
      <button onClick={() => dispatch({type: 'decrement'})}>-</button>
      <button onClick={() => dispatch({type: 'increment'})}>+</button>
    </>
  );
}
```

<br />

#### Bailing out of a dispatch
* If you return the same value from a Reducer Hook as the current state, React will bail out without rendering the children or firing effects. (React uses the Object.is comparison algorithm.)
* Note that React may still need to render that specific component again before bailing out. That shouldn’t be a concern because React won’t unnecessarily go “deeper” into the tree. If you’re doing expensive calculations while rendering, you can optimize them with useMemo.

<br />

### useCallback

```jsx
const memoizedCallback = useCallback(
  () => {
    doSomething(a, b);
  },
  [a, b],
);
```

<br />

* Returns a memoized callback.
* Pass an inline callback and an array of dependencies. useCallback will return a memoized version of the callback that only changes if one of the dependencies has changed. This is useful when passing callbacks to optimized child components that rely on reference equality to prevent unnecessary renders (e.g. shouldComponentUpdate).
* useCallback(fn, deps) is equivalent to useMemo(() => fn, deps).

<br />

#### Practical example of useCallback


<br />

### useMemo

```jsx
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

* Returns a memoized value.
* Pass a “create” function and an array of dependencies. useMemo will only recompute the memoized value when one of the dependencies has changed. This optimization helps to avoid expensive calculations on every render.
* Remember that the function passed to useMemo runs during rendering. Don’t do anything there that you wouldn’t normally do while rendering. For example, side effects belong in useEffect, not useMemo.
* If no array is provided, a new value will be computed on every render.
* You may rely on useMemo as a performance optimization, not as a semantic guarantee. In the future, React may choose to “forget” some previously memoized values and recalculate them on next render, e.g. to free memory for offscreen components. Write your code so that it still works without useMemo — and then add it to optimize performance.


<br />

#### Practical example of useMemo


<br />

### useRef

* `const refContainer = useRef(initialValue);`
* useRef returns a mutable ref object whose .current property is initialized to the passed argument (initialValue). The returned object will persist for the full lifetime of the component.
* A common use case is to access a child imperatively:
```jsx
function TextInputWithFocusButton() {
  const inputEl = useRef(null);
  const onButtonClick = () => {
    // `current` points to the mounted text input element
    inputEl.current.focus();
  };
  return (
    <>
      <input ref={inputEl} type="text" />
      <button onClick={onButtonClick}>Focus the input</button>
    </>
  );
}
```
* Essentially, useRef is like a “box” that can hold a mutable value in its .current property.
* You might be familiar with refs primarily as a way to access the DOM. If you pass a ref object to React with <div ref={myRef} />, React will set its .current property to the corresponding DOM node whenever that node changes.
* However, useRef() is useful for more than the ref attribute. It’s handy for keeping any mutable value around similar to how you’d use instance fields in classes.
* This works because useRef() creates a plain JavaScript object. The only difference between useRef() and creating a {current: ...} object yourself is that useRef will give you the same ref object on every render.
* Keep in mind that useRef doesn’t notify you when its content changes. Mutating the .current property doesn’t cause a re-render. If you want to run some code when React attaches or detaches a ref to a DOM node, you may want to use a callback ref instead.


<br />


### useImperativeHandle

* `useImperativeHandle(ref, createHandle, [deps])`
* useImperativeHandle customizes the instance value that is exposed to parent components when using ref. As always, imperative code using refs should be avoided in most cases. useImperativeHandle should be used with forwardRef:
```jsx
function FancyInput(props, ref) {
  const inputRef = useRef();
  useImperativeHandle(ref, () => ({
    focus: () => {
      inputRef.current.focus();
    }
  }));
  return <input ref={inputRef} />;
}
FancyInput = forwardRef(FancyInput);
```
* In this example, a parent component that renders <FancyInput ref={inputRef} /> would be able to call inputRef.current.focus().

<br />

### useLayoutEffect

* The signature is identical to useEffect, but it fires synchronously after all DOM mutations. Use this to read layout from the DOM and synchronously re-render. Updates scheduled inside useLayoutEffect will be flushed synchronously, before the browser has a chance to paint.
* Prefer the standard useEffect when possible to avoid blocking visual updates.
* If you use server rendering, keep in mind that neither useLayoutEffect nor useEffect can run until the JavaScript is downloaded. This is why React warns when a server-rendered component contains useLayoutEffect. To fix this, either move that logic to useEffect (if it isn’t necessary for the first render), or delay showing that component until after the client renders (if the HTML looks broken until useLayoutEffect runs).
* To exclude a component that needs layout effects from the server-rendered HTML, render it conditionally with showChild && <Child /> and defer showing it with useEffect(() => { setShowChild(true); }, []). This way, the UI doesn’t appear broken before hydration.

<br />

### useDebugValue

* `useDebugValue(value)`
* useDebugValue can be used to display a label for custom hooks in React DevTools.
* We don’t recommend adding debug values to every custom Hook. It’s most valuable for custom Hooks that are part of shared libraries.

```jsx
function useFriendStatus(friendID) {
  const [isOnline, setIsOnline] = useState(null);

  // ...

  // Show a label in DevTools next to this Hook
  // e.g. "FriendStatus: Online"
  useDebugValue(isOnline ? 'Online' : 'Offline');

  return isOnline;
}
```

<br />

#### Defer formatting debug values

* In some cases formatting a value for display might be an expensive operation. It’s also unnecessary unless a Hook is actually inspected.
* For this reason useDebugValue accepts a formatting function as an optional second parameter. This function is only called if the Hooks are inspected. It receives the debug value as a parameter and should return a formatted display value.
* For example a custom Hook that returned a Date value could avoid calling the toDateString function unnecessarily by passing the following formatter:

```jsx
useDebugValue(date, date => date.toDateString());
```

<br />

### Custom hooks

* [Custom hooks](https://reactjs.org/docs/hooks-custom.html)

<br />


* A custom Hook is a JavaScript function whose name starts with ”use” and that may call other Hooks.
* We can decide what it takes as arguments, and what, if anything, it should return. In other words, it’s just like a normal function. Its name should always start with use so that you can tell at a glance that the rules of Hooks apply to it.
* Custom Hooks are a convention that naturally follows from the design of Hooks, rather than a React feature.
* Custom Hooks are a mechanism to reuse stateful logic (such as setting up a subscription and remembering the current value), but every time you use a custom Hook, all state and effects inside of it are fully isolated.
* spotting cases where a custom Hook could hide complex logic behind a simple interface, or help untangle a messy component.
* Tip: Pass Information Between Hooks


<br />


## Events & events handling


<br />

### Synthetic events

* [Events](https://reactjs.org/docs/events.html)

<br />

* Your event handlers will be passed instances of SyntheticEvent, a cross-browser wrapper around the browser’s native event. It has the same interface as the browser’s native event, including stopPropagation() and preventDefault(), except the events work identically across all browsers.
* If you find that you need the underlying browser event for some reason, simply use the nativeEvent attribute to get it. The synthetic events are different from, and do not map directly to, the browser’s native events. For example in onMouseLeave event.nativeEvent will point to a mouseout event. The specific mapping is not part of the public API and may change at any time. Every SyntheticEvent object has the following attributes:

<br />

#### Difference between onChange and onInput

* The change event fires in most browsers when content is changed and the element loses focus. It's basically an aggregate of changes. It will not fire for every single change as in the case input event.
* The input event fires synchronously on change of the content for the element. As such, the event listener tends to fire more frequently.

<br />

### Handler Functions & Callback handlers

* [Difference between event handlers and callbacks](https://stackoverflow.com/questions/2069763/difference-between-event-handlers-and-callbacks)

<br />

* A callback is procedure you pass as an argument to another procedure. The procedure receiving the parameter can call it, or share it so some other procedures in the system can call it.
* An event handler is a procedure called when an event happens. It can be a callback.

<br />

* [Handling events](https://reactjs.org/docs/handling-events.html)

<br />

```jsx
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```

<br />


## Styling components

<br />

### CSS/SASS

<br />

#### inline styling

* The style attribute accepts a JavaScript object with camelCased properties rather than a CSS string. This is consistent with the DOM style JavaScript property, is more efficient, and prevents XSS security holes. For example:

```jsx
const divStyle = {
  color: 'blue',
  backgroundImage: 'url(' + imgUrl + ')',
};

function HelloWorldComponent() {
  return <div style={divStyle}>
    Hello World!
    <div style={{ height: 10 }}>
      Hello World!
    </div>
    <div style={{ height: '10%' }}>
      Hello World!
    </div>
  </div>;
}
```

<br />

#### ClassNames

* A simple JavaScript utility for conditionally joining classNames together.

```jsx
classNames('foo', 'bar'); // => 'foo bar'
classNames('foo', { bar: true }); // => 'foo bar'
classNames({ 'foo-bar': true }); // => 'foo-bar'
classNames({ 'foo-bar': false }); // => ''
classNames({ foo: true }, { bar: true }); // => 'foo bar'
classNames({ foo: true, bar: true }); // => 'foo bar'

// lots of arguments of various types
classNames('foo', { bar: true, duck: false }, 'baz', { quux: true }); // => 'foo bar baz quux'

// other falsy values are just ignored
classNames(null, false, 'bar', undefined, 0, 1, { baz: null }, ''); // => 'bar 1'
```

<br />

### CssInJS / JSS

* “CSS-in-JS” refers to a pattern where CSS is composed using JavaScript instead of defined in external files.
* Note that this functionality is not a part of React, but provided by third-party libraries. React does not have an opinion about how styles are defined; if in doubt, a good starting point is to define your styles in a separate *.css file as usual and refer to them using className.
* JSS is an authoring tool for CSS which allows you to use JavaScript to describe styles in a declarative, conflict-free and reusable way. It can compile in the browser, server-side or at build time in Node.

<br />

#### JSS


```jsx
import jss from 'jss'
import preset from 'jss-preset-default'
import color from 'color'

// One time setup with default plugins and settings.
jss.setup(preset())

const styles = {
  '@global': {
    body: {
      color: 'green'
    },
    a: {
      textDecoration: 'underline'
    }
  },
  withTemplates: `
    border-radius: 3px;
    background-color: green;
    color: red;
    margin: 20px 40px;
    padding: 10px;
  `,
  button: {
    fontSize: 12,
    '&:hover': {
      background: 'blue'
    }
  },
  ctaButton: {
    extend: 'button',
    '&:hover': {
      background: color('blue')
        .darken(0.3)
        .hex()
    }
  },
  '@media (min-width: 1024px)': {
    button: {
      width: 200
    }
  }
}

const {classes} = jss.createStyleSheet(styles).attach()

document.body.innerHTML = `
  <button class="${classes.button}">Button</button>
  <button class="${classes.ctaButton}">CTA Button</button>
```

<br />

#### React-JSS
```jsx
import React from 'react'
import {render} from 'react-dom'
import {createUseStyles} from 'react-jss'

// Create your Styles. Remember, since React-JSS uses the default preset,
// most plugins are available without further configuration needed.
const useStyles = createUseStyles({
  myButton: {
    color: 'green',
    margin: {
      // jss-expand gives more readable syntax
      top: 5, // jss-default-unit makes this 5px
      right: 0,
      bottom: 0,
      left: '1rem'
    },
    '& span': {
      // jss-nested applies this to a child span
      fontWeight: 'bold' // jss-camel-case turns this into 'font-weight'
    }
  },
  myLabel: {
    fontStyle: 'italic'
  }
})

const Button = ({children}) => {
  const classes = useStyles()
  return (
    <button className={classes.myButton}>
      <span className={classes.myLabel}>{children}</span>
    </button>
  )
}

const App = () => <Button>Submit</Button>

render(<App />, document.getElementById('root'))
```

<br />

### Material UI and component frameworks

*

<br />


## State management

<br />

### State design & lifting state



<br />


### Redux

* Redux is a predictable state container for JavaScript apps.
* Imagine your app’s state is described as a plain object. This object is like a “model” except that there are no setters. This is so that different parts of the code can’t change the state arbitrarily, causing hard-to-reproduce bugs.
* To change something in the state, you need to dispatch an action. An action is a plain JavaScript object (notice how we don’t introduce any magic?) that describes what happened.
* Enforcing that every change is described as an action lets us have a clear understanding of what’s going on in the app. If something changed, we know why it changed. Actions are like breadcrumbs of what has happened. Finally, to tie state and actions together, we write a function called a reducer. Again, nothing magical about it—it’s just a function that takes state and action as arguments, and returns the next state of the app. It would be hard to write such a function for a big app, so we write smaller functions managing parts of the state.

<br />

### Redux-Saga


<br />

### Redux-toolkit


<br />

## React Router

<br />

### Router

<br />

### Route

<br />

### BrowserRouter

<br />

### Link & NavLink

<br />

### Redirect

<br />


### Switch

<br />


### history, location & match


<br />


## Consuming REST APIs

<br />


### Where to execute REST requests?

* I would suggest you to make api call in life cycle method componentDidMount and when api call returns some data set it to your state and let your user interface update accordingly.

<br />

### axios

<br />

### isomorphic-fetch



<br />

### Async requests


<br />


## React performance

<br />

### Code splitting

<br />
