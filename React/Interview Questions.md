# React/Interview Questions


What is a pure function and how are they relevant to React?
: A function is considered pure if it contains no side effects and if, given the same input, it always returns the same output. They're relevant to React because, as a rule, React components (and more specifically, React's rendering flow) must be pure.

What is the difference between a React element and a React component?
: A React element is an object description of a DOM node. A React component is a function that optionally accepts input (via **props** or **context**), and returns a React element.

What are keys in React and why are they important?
: Keys in React are used to identify unique elements in collections, such as arrays or maps. They play a critical role in helping React keep track of changes in the list or collection, ensuring that the rendering process is efficient and that the state is maintained correctly across re-renders.

What are children in React?
: When you create an element, anything that is between the opening and closing tag of that element is considered children and is accessible in that component via props.children.

How would you handle the scenario where two components depend on the same piece of state?
: Whenever you have state that multiple components depend on, you'll want to lift that state up to the nearest parent component and then pass it down via props.

How would you handle updating state that two components depend on?
: Often times when lifting state up, you decouple where the state lives from the event handlers that update that state. To solve this, you'll create an updater function in the parent component where the state lives and, via props, invoke that function from the child component(s) where the event handler(s) live.

What are some signals that tell you you should reach for useReducer instead of useState?
: useReducer offers a bit more flexibility than useState since it allows you to decouple how the state is updated from the action that triggered the update - typically leading to more declarative state updates.
  
  If your state tends to be updated together or if updating one piece of state is based on another piece of state, that's a signal you might want to use useReducer.

What does it mean to derive state in React?
: Deriving state refers to the practice of computing values based on props or other state within a React component. It allows you to minimize the amount of React state you use in a component, often leading to more predictable state updates. i.e. deriving filtered items from an array based on a search term.

What is "rendering" in React?
: Rendering is just a fancy way of saying that React calls your function component with the intent of eventually updating the View.

What causes a React component to re-render?
: When the state of a component changes. (state, props)

Describe the process that happens when React renders
: When React renders a component, two things happen.
  
  First, React creates a snapshot of your component which captures everything React needs to update the view at that particular moment in time. props, state, event handlers, and a description of the UI (based on those props and state) are all captured in this snapshot.
  
  From there, React takes that description of the UI and uses it to update the View.

If you needed to preserve a value across renders, but that value had nothing to do with the UI, what would you do?
: You'd store the value inside of a "ref" using React.useRef.

What are the two most common scenarios for using useRef in React?
: To preserve a value across renders, without triggering a re-render when it changes.
  To get a reference to a DOM node.

What is the purpose of useEffect?
: From a philosophical perspective, to synchronize your component with some external system. From a pragmatic perspective, to remove side effects (that aren't triggered by a particular event) from React's rendering flow.

What are some common pitfalls with useEffect and how can you avoid them?
: There are a bunch.

  You should avoid using useEffect for data transformations. Instead, derive state at the top of the component.

  If a side effect is triggered by a specific user event, put that side effect in an event handler instead of useEffect
  
  Don't use useEffect as a way to react to props or state changes.
  
  Don't use useEffect to subscribe to an external store, instead use useSyncExternalStore.
  
  Don't ignore useEffect's dependency array or leave values out of it.
  
  Don't think of useEffect's dependency array as a way to re-run the effect. Instead, it's a list of all of the dependencies needed to synchronize with some outside system.

How would you add support for i18n (localized text) to your React application?
: Because i18n requires that you're able to pass locale data to any component in the component tree, regardless of how deeply nested the components are, context is a good solution for it.

What is React.memo and when would you want to use it?
: React.memo is a higher-order component (a function that takes in a React component as an argument and returns a new component).

  You'd want to use it when you have an expensive component and you want the component to opt of of React's default behavior and only re-render when its props change.

Tell me everything you know about React's useMemo hook
: useMemo lets you cache the result of a calculation between renders.
  Its first argument is a function that returns the value you want to cache. Its second argument is an array of dependencies the function depends on. If any of the dependencies change, the cached value will be recalculated.

Tell me everything you know about React's useCallback hook
: useCallback lets you cache a function (and therefore, keep it referentially stable across renders).
  Its first argument is the function you want to cache and its second argument is an array of dependencies the function depends on. If any of the dependencies change, the cached function will be updated. It's particularly helpful when you're creating a custom hook that returns a function. You'll want to make sure the function's reference stays stable so you don't invalidate any React.memos that the consumer of the hook has.

Describe a scenario where you'd want to use useLayoutEffect instead of useEffect
: React guarantees that the code inside of useLayoutEffect, as well as any state updates scheduled within it, will be processed before the browser repaints the screen.

  A scenario where you'd want to use it over useEffect is anytime your component is reliant upon layout information for rendering. useMouse, useWindowScroll, and useWindowSize are all examples of this.

How would you integrate a custom data store into your React application?
: This would be the perfect use case for React's useSyncExternalStore hook.

How would you fetch data from an external API in a React application?
: Fetching data from an external API is a side effect, so you'll need to get it out of React's rendering flow. You have two options for this. If the side effect is triggered by a specific user event, you can stick it inside of an event handler. If it's not, then you're most likely synchronizing your component with some outside system and therefore, you'll want to stick it inside of useEffect.

What would you do if you had two components that both needed to share the same non-visual logic?
: You'd encapsulate that logic into a custom hook that each component could consume.

What is lazy state initialization and when is it useful?
: It's rare, but there are times when your initial state is the result of a calculation (usually encapsulated in a function).
  If you pass a function to useState (as opposed to a function invocation), React will only invoke that function on the initial render in order to get the initial state.
  ```
  const [state, setState] = React.useState(getInitialState)
  // NOT
  const [state, setState] = React.useState(getInitialState())
  ```

Why is it necessary to include every dependency in useEffect's dependency array?
: There are two primary reasons.

  If you don't, React (and more specifically ESLint) will yell at you.
  Because of how React's snapshot phase "captures" the component's props and state at specific moments in time, there are scenarios where, if you don't include the appropriate values in the dependency array, those missing values will be "stale" inside of useEffect.

What's the purpose of React's useEffectEvent hook?
: useEffectEvent allows you to abstract any reactive but non-synchronizing values into their own event handler that you can then use inside of useEffect without needing to declare in useEffect's dependency array.

How would you approach building a React app that needed to read the browser's window size and react to its changes?
: I'd encapsulate all of the logic for computing the browser's window size into its own custom useWindowSize hook (and I'd be sure to use useLayoutEffect instead of useEffect since the hook is reliant upon layout information).

How can you set default props for a component?
: React components are just functions, so you can use JavaScript's "default parameters" feature. (i.e. myFunction(var1 = "default"))
  Using react.props can also set defaultProps.

How would you update a nested object that is stored as React state?
: When you pass a value to useState's updater function, whatever value you pass will always replace the current piece of state. What this means is that if you have a piece of state that is an object, it won't be merged with the current state.

  Instead of replacing the object, you most likely want to merge the new value with the existing state object. To do that, we can use JavaScript's spread operator to spread all of the existing key/value pairs onto a new object, then pass that new object to the updater function.
  ```
    const updatedUser = {
      ...user,
      email: "imatest@gmail.com"
    }
    setUser(updatedUser) //
  ```

Why are regular variables insufficient for reacting to user interactions in a React component?
: For a few different reasons.
  1. React has no way of knowing when regular variables change and is therefore unable to trigger a re-render when they do.
  
  2. Normal variables don't get persisted across renders.

If React.memo isn't working appropriately, what is the most likely cause?
: A prop being passed to the component wrapped in React.memo is a reference value whose reference is changing on every render.

What's the de facto way to add a new item to an array that's on state?
: To add an item to an array, use JavaScript's spread operator (...) to spread all the existing elements onto a new array with the new item.
  ```
  const newHighScores = [...highScores, session]
  setHighScores(newHighScores)
  ```

What's the de facto way to remove an item from an array that's on state?
: To remove an item from an array, use JavaScript's filter method to create a new array, filtering out the item that should be removed.
  ```
  const newHighScores = highScores.filter((session) =>
    session.id !== id
  )
  setHighScores(newHighScores)
  ```
  
What's the de facto way to update an item in an array that's on state?
:  To update an item, use JavaScript's map method to create a new array, updating the specific item where appropriate.
  ```
  const newHighScores = highScores.map((session) => {
      return session.id === updatedSession.id
        ? updatedSession
        : session
    })
    setHighScores(newHighScores)
  ``` 

What types of data can you pass as props?
: It's just JavaScript, baby. Do whatever you want.

What are the most common techniques used to conditionally render UI in React?
: The most basic example is just using a simple if/else statement.
  If you're rendering different UI based on a single condition, typically you'd use JavaScript's ternary operator.
  You can also use JavaScript's logical && operator.

What is "batching" in React?
: Batching is React's algorithm for minimizing unnecessary re-renders. It works by taking into account every updater function inside of an event handler before it updates its state and triggers a re-render.

What is the purpose of React's StrictMode component?
: StrictMode is a React component that enables the following behavior in development:

  * Your components will re-render an extra time to find bugs caused by impure rendering.
  * Your components will re-run Effects an extra time to find bugs caused by improper cleanups.
  * Your components will be checked for deprecated APIs.

In the context of React, what is a "snapshot"?
: A "snapshot" is a step in React's rendering flow that captures everything React needs to update the view at a specific moment in time. props, state, event handlers, and a description of the UI (based on those props and state) are all captured in the snapshot.

What are the 5 rules for managing side effects in React?
: * Rule #0 – When a component renders, it should do so without running into any side effects 
  * Rule #1 – If a side effect is triggered by an event, put that side effect in an event handler 
  * Rule #2 – If a side effect is synchronizing your component with some outside system, put that side effect inside useEffect 
  * Rule #3 – If a side effect is synchronizing your component with some outside system and that side effect needs to run before the browser paints the screen, put that side effect inside useLayoutEffect 
  * Rule #4 – If a side effect is subscribing to an external store, use the useSyncExternalStore hook
