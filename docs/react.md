# React Ecosystem

Return to content [Table of Contents](../README.md)

## 1. Folder Structure

## 2. Don't use anonymus function in events props

> Instead of

```javascript
<Button onClick={() => this.props.callback()} />
```

> Use

```javascript
<Button onClick={this.method} />
```

## 3. Use Pure Component or Stateless Component if possible

```javascript
export class Home extends PureComponent {
  /* ... */
}

export const User = props => <User {...props} />;
```

## 4. Divide complex render function to several rener functions

> Instead of

```javascript
render () {
  <Component>
    {data.map((d) => <User {...d} />)}
    /* ... */
    /* ... */
    /* ... */
  </Component>
}
```

> Use

```javascript
render () {
  <Component>
    {this.renderUsers()}
    {this.renderList()}
  </Component>
}
```

## 5. Extract complex logic from Component to another modules

> Instead of

```javascript
componentDidMount () {
  fetch('url').then( () => {
    /* ... */
  });
  /* ... */
}
```

> Use

```javascript
componentDidMount () {
  this.getData()
}
getData = async () => {
  const data = await api.getData
  /* ... */
}
```

## 6. Extract complex logic from reducer to helper or middlewares

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

## 7. When using Redux use create reducer mapper

```javascript
export function createReducer(initialState, handlers) {
  return (state = initialState, action) => {
    const handler = handlers[action.type];
    if (!handler) return state;
    return { ...state, ...handler(state, action) };
  };
}
```

## 8. Always declare type of props via Typescript

```typescript
interface IHome {
  user: IUser;
  route: string;
}

export class Home extends React.Component<IHome> {
  /* ... */
}
```
