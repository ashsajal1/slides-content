# 10 slides about useReducer :

1. **useReducer is a React Hook that lets you add a reducer to your component to manage state**
```javascript
const [state, dispatch] = useReducer(reducer, initialArg);
```

2. **Returns an array with exactly two values: current state and a dispatch function**
```javascript
const [state, dispatch] = useReducer(reducer, initialArg);
// state: current state value
// dispatch: function to trigger updates
```

3. **The dispatch function updates state to a different value and triggers a re-render**
```javascript
dispatch({ type: 'incremented_age' });
```

4. **Reducer functions take current state and action, then calculate and return the next state**
```javascript
function reducer(state, action) {
  // calculate and return next state
  return nextState;
}
```

5. **Actions are objects representing what the user did, commonly with a 'type' property**
```javascript
switch (action.type) {
  case 'incremented_age':
    return { ...state, age: state.age + 1 };
}
```

6. **The dispatch function only updates state for the next render, not immediately in running code**
```javascript
dispatch({ type: 'incremented_age' });
console.log(state.age); // Still shows old value
```

7. **State is read-only - never modify objects or arrays directly, always return new ones**
```javascript
// Wrong: state.age = 42;
// Correct: return { ...state, age: 42 };
```

8. **React batches state updates and updates the screen after all event handlers have run**
```javascript
// Multiple dispatches in one event handler
// are batched into a single re-render
```

9. **Pass an initializer function as the third argument to avoid recreating initial state on every render**
```javascript
const [state, dispatch] = useReducer(reducer, username, createInitialState);
```

10. **React skips re-rendering if new state equals current state, determined by Object.is comparison**
```javascript
// If Object.is(newState, currentState) === true
// React skips re-rendering the component
```
