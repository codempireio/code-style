# Using Redux

[Return to Table of Contents](../../README.md)

[Return to React](README.md)

## Extract complex logic from reducer to helper or middleware

> Instead of

```javascript
[ACTION_TYPE]: (state, action) => {
  const newData = action.payload.data.map(() => {
    /* ... */
  }).filter(() => {
    /* ... */
  })

   /* ... */

  return {
    ...state,
      /* ... */
  }
}
```

> Use

```javascript
import { formatData } from './helpers'

/* ... */

[ACTION_TYPE]: (state, action) => {

  return {
    ...state,
    data: formatData(action.payload)
  }
}
```

## When using Redux use create reducer mapper

```javascript
export function createReducer(initialState, handlers) {
  return (state = initialState, action) => {
    const handler = handlers[action.type];
    if (!handler) return state;
    return { ...state, ...handler(state, action) };
  };
}
```
