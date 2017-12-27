# Redux in a pill
written by Mariusz Szypulski

[![](https://codingthesmartway.com/wp-content/uploads/2017/05/01.png)]()

## Actions

Actions are JavaScript objects that use **type** property to inform about the data that should be sent to the store.

## Reducers

While actions only trigger changes in the app, the **reducers** specify those changes. The reducer is a function that takes two parameters (**state** and **action**) to calculate and return an updated state.

## Store

The store is a place that holds the app's state. 

## Root Component

The **App** component is the root component of the app. Only the root component should be aware of a redux.

Example:
```javascript
import React, { PropTypes } from 'react';
import { render } from 'react-dom';
import { createStore } from 'redux';
import { connect, Provider } from 'react-redux';

const reducer = (state, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return { ...state, counter: state.counter + 1 };
    case 'DECREMENT':
      return { ...state, counter: state.counter - 1 };
    default:
      return state;
    }
};

const store = createStore(reducer, { counter: 0 });

class Counter extends React.Component {
  static propTypes = {
    counter: PropTypes.Number,
    onIncrement: PropTypes.func,
    onDecrement: PropTypes.func
  };

  render() {
    const { counter, onDecrement, onIncrement } = this.props;

    return (
      <div>
        <div>{counter}</div>
        <button onClick={onDecrement}>-</button>
        <button onClick={onIncrement}>+</button>
      </div>
    );
  }
}

const mapStateToProps = (state) => {
  return { counter: state.counter };
};
const mapDispatchToProps = (dispatch) => {
  return {
    onIncrement: () => dispatch({ type: 'INCREMENT' }),
    onDecrement: () => dispatch({ type: 'DECREMENT' })
  }
};

Counter = connect(mapStateToProps, mapDispatchToProps)(Counter);

render(
  <Provider store={store}>
    <Counter />
  </Provider>
  , document.getElementById('root')
);
```
### Reducer
```javascript
const reducer = (state, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return { ...state, counter: state.counter + 1 };
    case 'DECREMENT':
      return { ...state, counter: state.counter - 1 };
    default:
      return state;
    }
};
```
The reducer function takes two parameters (state and action) and checking action type, when type is 'INCREMENT', function increments counter and returns new object of state (spread operator '...state'), because in redux state is always immutable. When type is 'DECREMENT', function decrements counter and returns new object of state. When type is other, function return a state object.

### Store
```javascript
const store = createStore(reducer, { counter: 0 });
```
The store object is created by a createStore fuction, which takes two parameters: reducer function (which is a callback function) and beginning state of application 'counter: 0'. The createStore function is imported from a redux library.
```javascript
import { createStore } from 'redux';
```
### Component
```javascript
class Counter extends React.Component {
  static propTypes = {
    counter: PropTypes.Number,
    onIncrement: PropTypes.func,
    onDecrement: PropTypes.func
  };

  render() {
    const { counter, onDecrement, onIncrement } = this.props;

    return (
      <div>
        <div>{counter}</div>
        <button onClick={onDecrement}>-</button>
        <button onClick={onIncrement}>+</button>
      </div>
    );
  }
}
```
The counter property and onDecrement, onIncrement methods are extracted from this.props object.
```javascript
const mapStateToProps = (state) => {
  return { counter: state.counter };
};
const mapDispatchToProps = (dispatch) => {
  return {
    onIncrement: () => dispatch({ type: 'INCREMENT' }),
    onDecrement: () => dispatch({ type: 'DECREMENT' })
  }
};
```
The mapStateToProps function takes state as a parameter and returns new object. The mapDispatchToProps function returns object with methods, which dispatch action objects to the store.
```javascript
Counter = connect(mapStateToProps, mapDispatchToProps)(Counter);
```
The connect function takes two functions as parameters and the result of their call joins to the object. This function injects this object to the this.props of a Counter component. The returned function returns new Counter component.
```javascript
render(
  <Provider store={store}>
    <Counter />
  </Provider>
  , document.getElementById('root')
);
```
The last thing is rendering everything by a render function. We have to include our Counter component in the Provider component from react-redux module, which takes a store object as a parameter.
