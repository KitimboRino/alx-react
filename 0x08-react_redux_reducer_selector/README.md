# 0x08. React Redux reducer+selector

# Understanding Reducers in Redux

## Introduction

This guide explores the purpose of reducers in Redux, their importance in maintaining application state, and best practices like immutability and normalization. It also covers the use of selectors to efficiently access state within your Redux store.

## Table of Contents

1. [The Purpose of a Reducer and Its Role in Your Application](#the-purpose-of-a-reducer-and-its-role-in-your-application)
2. [Why a Reducer Should Stay as Pure as Possible](#why-a-reducer-should-stay-as-pure-as-possible)
3. [Why Mutations Should Not Happen Within a Reducer](#why-mutations-should-not-happen-within-a-reducer)
4. [The Use of Immutable.js Within the Reducer](#the-use-of-immutable-js-within-the-reducer)
5. [The Use of Normalizr Within the App](#the-use-of-normalizr-within-the-app)
6. [Selectors: What They Are and When to Use Them](#selectors-what-they-are-and-when-to-use-them)

## The Purpose of a Reducer and Its Role in Your Application

### What is a Reducer?

A reducer is a pure function in Redux that takes the current state and an action as arguments and returns a new state. The reducer's role is to determine how the application's state should change in response to actions. It's a crucial part of the Redux architecture because it controls how state is updated and maintained.

### Example

```javascript
const initialState = { count: 0 };

function counterReducer(state = initialState, action) {
  switch (action.type) {
    case 'INCREMENT':
      return { ...state, count: state.count + 1 };
    case 'DECREMENT':
      return { ...state, count: state.count - 1 };
    default:
      return state;
  }
}
```

### Role of Reducers

Reducers are the primary mechanism for managing state transitions in your Redux store. They ensure that state changes are predictable and can be easily tested.

## Why a Reducer Should Stay as Pure as Possible

### What is a Pure Function?

A pure function is one that, given the same input, always returns the same output and has no side effects (like modifying external variables or performing I/O operations). In Redux, reducers should be pure to ensure that state updates are predictable and that time-travel debugging (like with Redux DevTools) works correctly.

### Importance of Purity

Keeping reducers pure allows for easier testing, debugging, and reasoning about state changes. It ensures that reducers only rely on their input parameters (state and action) to produce the next state.

### Example of a Pure Reducer

```javascript
function pureReducer(state = initialState, action) {
  switch (action.type) {
    case 'INCREMENT':
      return { ...state, count: state.count + 1 };
    default:
      return state;
  }
}
```

## Why Mutations Should Not Happen Within a Reducer

### Avoiding Mutations

Reducers should never mutate the state directly. Instead, they should always return a new state object. This immutability ensures that previous states are preserved, which is essential for debugging, undo/redo functionality, and React’s efficient rendering process.

### Consequences of Mutations

Mutating state within a reducer can lead to unpredictable behavior, difficult-to-track bugs, and issues with performance optimizations in React.

### Example of Avoiding Mutation

```javascript
// Bad: Mutating the state directly
function badReducer(state = initialState, action) {
  switch (action.type) {
    case 'INCREMENT':
      state.count += 1;
      return state;
    default:
      return state;
  }
}

// Good: Returning a new state object
function goodReducer(state = initialState, action) {
  switch (action.type) {
    case 'INCREMENT':
      return { ...state, count: state.count + 1 };
    default:
      return state;
  }
}
```

## The Use of Immutable.js Within the Reducer

### Why Use Immutable.js?

Immutable.js is a library that provides immutable data structures for JavaScript. Using it in reducers helps enforce immutability, ensuring that state updates always result in new objects, preventing accidental mutations.

### Example of Immutable.js in a Reducer

```javascript
import { Map } from 'immutable';

const initialState = Map({ count: 0 });

function counterReducer(state = initialState, action) {
  switch (action.type) {
    case 'INCREMENT':
      return state.set('count', state.get('count') + 1);
    default:
      return state;
  }
}
```

### Benefits

Immutable.js provides efficient structural sharing, which can improve performance when managing large state objects, and it makes state updates more predictable and reliable.

## The Use of Normalizr Within the App

### Why Use Normalizr?

Normalizr is used to normalize nested JSON data into a flat structure. This makes it easier to manage and access data within a Redux store, particularly when dealing with complex, nested responses from APIs.

### Example Integration

You can use Normalizr in your Redux actions or reducers to normalize data before storing it in the state.

```javascript
import { normalize, schema } from 'normalizr';

// Define schemas
const user = new schema.Entity('users');
const article = new schema.Entity('articles', { author: user });

function articleReducer(state = {}, action) {
  switch (action.type) {
    case 'RECEIVE_ARTICLE':
      return {
        ...state,
        ...normalize(action.payload, article).entities
      };
    default:
      return state;
  }
}
```

### Benefits

Normalization simplifies state management, reduces redundancy, and improves the performance of state updates and access.

## Selectors: What They Are and When to Use Them

### What is a Selector?

A selector is a function that extracts specific pieces of state from the Redux store. They help you avoid repetitive code and can be used to compute derived data from the state.

### Example of a Selector

```javascript
const getCount = (state) => state.counter.count;

function mapStateToProps(state) {
  return {
    count: getCount(state)
  };
}
```

### When to Use Selectors

Use selectors whenever you need to access or compute specific pieces of state in your application. They help keep your code organized and maintainable, especially as your application grows.

### Advanced Selectors with Reselect

For more complex state computations, you can use a library like `reselect` to create memoized selectors that improve performance by avoiding unnecessary recalculations.

```javascript
import { createSelector } from 'reselect';

const getItems = (state) => state.items;

const getVisibleItems = createSelector(
  [getItems, (state, filter) => filter],
  (items, filter) => items.filter(item => item.type === filter)
);
```

## Conclusion

Reducers play a vital role in maintaining the state of your Redux application. By adhering to best practices like immutability and using tools like Immutable.js and Normalizr, you can create a more predictable and maintainable state management system. Selectors further enhance your ability to work with state efficiently, leading to cleaner and more scalable code.