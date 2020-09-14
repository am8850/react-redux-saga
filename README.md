# React with Redux (JavaScript version)

## Redux

- What is Redux: Redux is a predictable state container for JavaScript apps.

## Initial Configuration

Steps:

- Create a a Redux store and add reducers
  - If there's more than one reducers, they need to be combined
  - In this example, there are two reducers, counter and signin
- Import a React-Redux Provider
  - This will allow the application access to the global store
- Use the provider and the store

> Note: if the Redux extension is installed, it will be activated in the developer tool by the command ```window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()```

File: Index.js

```javascript
import React from "react";
import ReactDOM from "react-dom";
import "./index.css";
import App from "./App";
import * as serviceWorker from "./serviceWorker";
import { createStore } from "redux";
import rootReducer from "./reducers";
import { Provider } from "react-redux";
import Counter from "./Counter";

const store = createStore(
  // Combined reducer
  rootReducer,
  // Devtools
  window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
);

ReactDOM.render(
  <Provider store={store}>
    <Counter />
  </Provider>,
  // </React.StrictMode>,
  document.getElementById("root")
);

// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls.
// Learn more about service workers: https://bit.ly/CRA-PWA
serviceWorker.unregister();
```

## Reducers

- A reducer is a function that handles an action via a dispatch. In the code below there a three reducers: counterReducer, loginReducer, and rootReducer.
- rootReducer is a comibination of counterReducer and loginReducer

File: Reducer\counterReducer.js

```javascript
const counterReducer = (state=0,action) => {
    switch(action.type) {
        case 'INCREMENT': 
                return state + action.payload;
        case 'DECREMENT':
            return state - 1;
        default:
            return state;
    }
}

export default counterReducer;
```

File: Reducer\loginReducer.js
```javascript
const loginReducer = (state=false, action) => {
    switch(action.type) {
        case 'SIGN_IN':
            return !state;
        default:
            return state;
    }
};

export default loggedReducer;
```

File: Reducer\index.js

```javascript
import { combineReducers } from "redux";
import counterReducer from "./counterReducer";
import loggedReducer from "./loginReducer";

const rootReducer = combineReducers({
    counter: counterReducer,
    isLogged: loginReducer
})

export default rootReducer;
```

## Actions

- An action returns an object that includes the action type and a optionally a payload. When an action is dispached it is handled by the reducer that is associated to the action.

File: Actions\index.js

```javascript
export const increment = (nbr) => {
    return {
        type: 'INCREMENT',
        payload: nbr
    };
}

export const decrement = () => {
    return {
        type: 'DECREMENT'
    };
}
```

## Sample Component:

File: Counter.js

Steps:

- The components access the global Redux store via the useSelector
- The component dispaches actions via the useDispatch
  - In this example the two actions are: increment and decrement functions
  - Notice that the increment function receives a parameter called the payload

```javascript
import React from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { increment, decrement } from "./actions";
function Counter() {
    const counter = useSelector(state=>state.counter);
    const isLogged = useSelector(state=>state.isLogged);
    const dispatch = useDispatch();
    return (
        <div>
            <h1>Counter Demo</h1>
            <p>Current Count: {counter}</p>
            { isLogged ? <p>Sensitive information</p> : ''}
            <button onClick={()=>{dispatch(increment(5))}}>+</button>
            <button onClick={()=>{dispatch(decrement())}}>-</button>
        </div>
    )
};
export default Counter;
```

## Reference

- This content is based on the following YouTube tutorial:
  - https://www.youtube.com/watch?v=CVpUuw9XSjY

 
