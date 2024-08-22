# Redux and Normalization with Normalizr

## Introduction

This guide covers the purpose and usage of the Normalizr library for normalizing nested JSON data, its integration with Redux, and key Redux concepts like actions, action creators, async actions, and testing.

## Table of Contents

1. [Normalizr’s Purpose and How to Use It](#normalizr-purpose-and-how-to-use-it)
2. [Schemas and Normalization of Nested JSON](#schemas-and-normalization-of-nested-json)
3. [Core Concepts of Redux](#core-concepts-of-redux)
4. [Redux Actions](#redux-actions)
5. [Redux Action Creators](#redux-action-creators)
6. [Async Actions in Redux](#async-actions-in-redux)
7. [How to Write Tests for Redux](#how-to-write-tests-for-redux)

## Normalizr’s Purpose and How to Use It

### Purpose of Normalizr

Normalizr is a library designed to simplify the management of deeply nested JSON data by normalizing it into a flat structure. This approach makes it easier to store and manage data within a Redux store, enabling efficient access and updates.

### Installation

Install Normalizr using npm or yarn:

```bash
npm install normalizr
```

```bash
yarn add normalizr
```

### Basic Usage

To use Normalizr, define schemas that represent the structure of your data. Here’s an example:

```javascript
import { normalize, schema } from 'normalizr';

// Define user and article schemas
const user = new schema.Entity('users');
const article = new schema.Entity('articles', {
  author: user,
  comments: [user]
});

const originalData = {
  id: '123',
  title: 'My Article',
  author: { id: '1', name: 'John Doe' },
  comments: [{ id: '2', name: 'Jane Smith' }, { id: '3', name: 'Jack White' }]
};

// Normalize the data
const normalizedData = normalize(originalData, article);

console.log(normalizedData);
```

The output is a normalized structure that is easier to manage within a Redux store.

## Schemas and Normalization of Nested JSON

### Defining Schemas

Schemas in Normalizr represent entities within your JSON data, such as users or articles. By defining schemas, you can specify the relationships between these entities.

### Normalization Process

Normalization involves converting nested JSON into a normalized format:

1. **Define the schema**: Identify the entities and their relationships.
2. **Normalize the data**: Use the `normalize` function with the defined schema.

### Example

```javascript
import { normalize, schema } from 'normalizr';

// Define schemas for user, comment, and article
const user = new schema.Entity('users');
const comment = new schema.Entity('comments', { commenter: user });
const article = new schema.Entity('articles', {
  author: user,
  comments: [comment]
});

// Original nested data
const originalData = {
  id: '123',
  author: { id: '1', name: 'Paul' },
  title: 'My Article',
  comments: [{ id: '324', commenter: { id: '2', name: 'Nicole' } }]
};

// Normalize the data
const normalizedData = normalize(originalData, article);

console.log(normalizedData);
```

Normalized data is easier to store and manage within a Redux store.

## Core Concepts of Redux

### Redux Overview

Redux is a state management library that provides a single source of truth for your application's state. It works by centralizing the state in a store, which can be accessed and modified through actions and reducers.

### Core Concepts

1. **Store**: The centralized state container.
2. **Actions**: Objects that describe changes to the state.
3. **Reducers**: Functions that specify how the state changes in response to actions.
4. **Dispatch**: A method to send actions to the store.
5. **Selectors**: Functions that retrieve specific data from the store.

## Redux Actions

### What are Actions?

Actions are plain JavaScript objects that describe events or changes in your application. Each action must have a `type` property that indicates the action's purpose.

### Example

```javascript
const addTodo = (text) => ({
  type: 'ADD_TODO',
  payload: text
});
```

## Redux Action Creators

### What are Action Creators?

Action creators are functions that return action objects. They simplify the creation of actions and make the code more maintainable.

### Example

```javascript
function addTodoActionCreator(text) {
  return {
    type: 'ADD_TODO',
    payload: text
  };
}

dispatch(addTodoActionCreator('Learn Redux'));
```

## Async Actions in Redux

### Handling Asynchronous Operations

To handle async operations in Redux, you can use middleware like `redux-thunk` or `redux-saga`. These tools allow you to manage side effects and dispatch actions based on async operations.

### Example with Redux Thunk

```javascript
function fetchTodos() {
  return function(dispatch) {
    dispatch({ type: 'FETCH_TODOS_REQUEST' });
    return fetch('/todos')
      .then(response => response.json())
      .then(data => dispatch({ type: 'FETCH_TODOS_SUCCESS', payload: data }))
      .catch(error => dispatch({ type: 'FETCH_TODOS_FAILURE', error }));
  };
}
```

## How to Write Tests for Redux

### Testing Actions

You can test actions by comparing the object returned by the action creator with the expected action object.

```javascript
import { addTodoActionCreator } from './actions';

test('addTodoActionCreator creates the correct action', () => {
  const text = 'Learn Redux';
  const expectedAction = {
    type: 'ADD_TODO',
    payload: text
  };
  expect(addTodoActionCreator(text)).toEqual(expectedAction);
});
```

### Testing Reducers

Test reducers by providing different actions and verifying the state updates correctly.

```javascript
import todoReducer from './reducers';

test('todoReducer handles ADD_TODO', () => {
  const initialState = [];
  const action = { type: 'ADD_TODO', payload: 'Learn Redux' };
  const expectedState = ['Learn Redux'];
  expect(todoReducer(initialState, action)).toEqual(expectedState);
});
```

### Testing Async Actions

For testing async actions, you can use `redux-mock-store` and `fetch-mock` to simulate the store and mock API responses.

```javascript
import configureMockStore from 'redux-mock-store';
import thunk from 'redux-thunk';
import fetchMock from 'fetch-mock';
import { fetchTodos } from './actions';

const middlewares = [thunk];
const mockStore = configureMockStore(middlewares);

afterEach(() => {
  fetchMock.restore();
});

test('fetchTodos success', () => {
  fetchMock.getOnce('/todos', {
    body: { todos: ['Task 1'] },
    headers: { 'content-type': 'application/json' }
  });

  const expectedActions = [
    { type: 'FETCH_TODOS_REQUEST' },
    { type: 'FETCH_TODOS_SUCCESS', payload: { todos: ['Task 1'] } }
  ];

  const store = mockStore({ todos: [] });

  return store.dispatch(fetchTodos()).then(() => {
    expect(store.getActions()).toEqual(expectedActions);
  });
});
```

## Conclusion

Normalizr and Redux together provide a powerful combination for managing complex application state. By understanding how to normalize data, handle Redux actions, and write comprehensive tests, you can create scalable and maintainable applications.