# Redux 4 Reacters
This post assumes you already know the basics of React. If so, you will learn Redux in 15 minutes with this post. 

Redux is a big step when having to develop and maintain a very rich web application at the front-end, like any "one single page" app of the modern days.

##The Redux Formulas: 

__Redux = State + Actions + Reducers (Predefined Flow)__

The state is changed by predifined flows called reducers which receive a given state and a given action to perform from that state.

So, in Redux:


```js
 newState = reducer(currentState, action)
```


# Redux Intro

Redux is a __Predictable State Container__ library. What da heck does this mean? This means that state of your app is gonna be managed by an [state pattern](https://en.wikipedia.org/wiki/State_pattern), so basically there will be an object commonly called "store" and that all state handled before by the components will be now handled by redux.

Lets see a couple of examples of plain states:

```js
	// Most basic example ever:
	stateExample_01 = 0; // A simple counter
	
	// A simple state for a todo App
	stateExample_02: {
		filter: 'ALL',
		todos: [
			{ title: 'Todo 1', done: false },
			{ title: 'Todo 2', done: false }
		]
	};
```

## Actions
In redux changes on state need to be triggered by a kind of object called __actions__. 

Actions are plain JavaScript objects a `type` property which is, well, the identifier of the action.

Example of an action:

```js
{
	type: 'INCREMENT_COUNTER'
}
```

## Reducers
The _predictable_ here means that the possible flow between states is predefined in what we call __reducers__. What means that there must be said somewhere that to change from state A to state B is allowed, or that Action B can be performed only from states a and z (for example). __To do so the reducer (a pure function) takes the previous state and an action, and returns the next state.__

Here is a super simple example, where the state is just an integer that stores a counter!.

```js
export default (state = 0, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return state + 1
    case 'DECREMENT':
      return state - 1
    default:
      return state
  }
}
```
### Remember!
Reducers __must return always a NEW state__, not modify the old one. It must create a new one and return it. This is very important for avoiding collisions. 

# How to use it?

Basically there are 7 steps:

1. Think and define your state object.
1. Define your actions.
1. Create the reducer function that returns the new objects for every action.
1. Init a redux _`store`_ object and configure it with the reducer function.
1. Events that could change state are handled by the redux store object method: __`store.dispatch`__, which accepts an action and internally will call the reducer
1. Bind component props to redux state using __`store.getState()`__.
1. Subscribe the redux `store` object to the render function, so every time there's a change in the state, redux calls `render`.

## Example

I took the code from this official redux example [https://github.com/reactjs/redux/tree/master/examples/counter](https://github.com/reactjs/redux/tree/master/examples/counter). Here the state object is simply an integer, and the actions are 'INCREMENT' and 'DECREMENT'.

### reducer.js

```js
export default (state = 0, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return state + 1
    case 'DECREMENT':
      return state - 1
    default:
      return state
  }
}
```
### index.js

```js

import React from 'react'
import ReactDOM from 'react-dom'
import { createStore } from 'redux'
import Counter from './Counter'
import counter from './reducer' // Step 3

const store = createStore(counter)
const rootEl = document.getElementById('root')

const render = () => ReactDOM.render(
  <Counter
    value={store.getState()}
    onIncrement={() => store.dispatch({ type: 'INCREMENT' })}
    onDecrement={() => store.dispatch({ type: 'DECREMENT' })}
  />,
  document.getElementById('root')
)

render()
store.subscribe(render)
```




### Counter.js
```js
import React, { Component, PropTypes } from 'react'

class Counter extends Component {
  static propTypes = {
    value: PropTypes.number.isRequired,
    onIncrement: PropTypes.func.isRequired,
    onDecrement: PropTypes.func.isRequired
  }

  render() {
    const { value, onIncrement, onDecrement } = this.props
    return (
      <p>
        Clicked: {value} times
        {' '}
        <button onClick={onIncrement}>
          +
        </button>
        {' '}
        <button onClick={onDecrement}>
          -
        </button>
      </p>
    )
  }
}

export default Counter
```

# From React to Redux

So basically the main thing to introduce redux into our React App is moving all component state management to Redux and change it through actions and reducers.
