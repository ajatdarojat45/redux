---
id: createstore
title: createStore
hide_title: true
description: 'API > createStore: creating a core Redux store'
---

&nbsp;

# `createStore(reducer, [preloadedState], [enhancer])`

Creates a Redux [store](Store.md) that holds the complete state tree of your app.
There should only be a single store in your app.

#### Arguments

1. `reducer` _(Function)_: A [reducing function](../understanding/thinking-in-redux/Glossary.md#reducer) that returns the next [state tree](../understanding/thinking-in-redux/Glossary.md#state), given the current state tree and an [action](../understanding/thinking-in-redux/Glossary.md#action) to handle.

2. [`preloadedState`] _(any)_: The initial state. You may optionally specify it to hydrate the state from the server in universal apps, or to restore a previously serialized user session. If you produced `reducer` with [`combineReducers`](combineReducers.md), this must be a plain object with the same shape as the keys passed to it. Otherwise, you are free to pass anything that your `reducer` can understand.

3. [`enhancer`] _(Function)_: The store enhancer. You may optionally specify it to enhance the store with third-party capabilities such as middleware, time travel, persistence, etc. The only store enhancer that ships with Redux is [`applyMiddleware()`](./applyMiddleware.md).

#### Returns

([_`Store`_](Store.md)): An object that holds the complete state of your app. The only way to change its state is by [dispatching actions](Store.md#dispatchaction). You may also [subscribe](Store.md#subscribelistener) to the changes to its state to update the UI.

#### Example

```js
import { legacy_createStore as createStore } from 'redux'

function todos(state = [], action) {
  switch (action.type) {
    case 'ADD_TODO':
      return state.concat([action.text])
    default:
      return state
  }
}

const store = createStore(todos, ['Use Redux'])

store.dispatch({
  type: 'ADD_TODO',
  text: 'Read the docs'
})

console.log(store.getState())
// [ 'Use Redux', 'Read the docs' ]
```

#### Tips

- Don't create more than one store in an application! Instead, use [`combineReducers`](combineReducers.md) to create a single root reducer out of many.

- Redux state is normally plain JS objects and arrays.

- If your state is a plain object, make sure you never mutate it! Immutable updates require making copies of each level of data, typically using the object spread operator ( `return { ...state, ...newData }` ).

- For universal apps that run on the server, create a store instance with every request so that they are isolated. Dispatch a few data fetching actions to a store instance and wait for them to complete before rendering the app on the server.

- When a store is created, Redux dispatches a dummy action to your reducer to populate the store with the initial state. You are not meant to handle the dummy action directly. Just remember that your reducer should return some kind of initial state if the state given to it as the first argument is `undefined`, and you're all set.

- To apply multiple store enhancers, you may use [`compose()`](./compose.md).
