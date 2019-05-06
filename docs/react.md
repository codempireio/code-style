# React Ecosystem

[Return to Table of Contents](../README.md)

## 1. Folder Structure

> Assets - contains images, fonts, icons

```
    /assets
      /img
        bg.png
      /fonts
        Arial.woff
      /icons
        avatar.svg
```

> Component - contains components (with local constants, typings) that are shared over several scenes;

```
   /components
      /spinner
        spinner.tsx
        spinner.test.tsx
        spinner.constants.ts
        index.ts
      /modal
```

> Constants - contains constants that are shared over several scenes, components, services...

```
    /constants
      colors.ts
      routes.ts
      config.ts
```

> Screens - contains all files according to - components, constants, typings, api etc.

```
    /screens
      /home
        /header
          header.tsx
          index.ts
        /footer
          footer.tsx
          footer.styles.ts
          index.ts
        home.tsx
        home.api.ts
        home.constants.ts
        home.state.ts
        home.typing.ts
        index.ts
      /auth
      /user
```

> Services - contains application services: api classes, helper functions, redux

```

    /services
      /api
        api-service.ts
        api-service.test.ts
        api-service.constants.ts
      /helpers
        time-slots.ts
      /redux
        middleware.ts
        store.ts
```

> Typings - contains shared typings

```
    /typings
      api.ts
      items.ts
```

> So, globally it will looks like

```
  /node_modules
  /public
  /src
    /assets
    /constants
    /components
    /screens
    /typings
    index.ts
```

> `index.ts` file serves as an export point. We should import files only from it.

## 2. Don't use anonymous function in events props

### It influence badly on app performance

> Instead of

```javascript
<Button onClick={() => this.props.onAction(this.props.id)} />
```

> Use

```javascript
  method () {
    this.props.onAction(this.props.id)
  }

  /* ... */

  <Button onClick={this.method} />
```

## 3. Use Pure Component or Stateless Component if possible

```javascript
export class Home extends PureComponent {
  /* ... */
}

export const User = props => <User {...props} />;
```

## 4. Divide complex render function to several functions or move it to another components

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
    <UserList users={data.users} />
    <Info currentUser={this.currentUser()} />
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
import { api } from '@services/api';

/* ... */

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

## 8. Always add typing to component props

```typescript
interface IHome {
  user: IUser;
  route: string;
}

export class Home extends React.Component<IHome> {
  /* ... */
}
```

## 9. Don't use constructor in React Component

> Instead of

```javascript
class Component extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      isOpen: false
    };

    this.onClick = this.onClick.bind(this);
  }
}
```

> Use

```javascript
class Component extends React.Component {
  state = {
    isOpen: false
  };

  onClick = () => {
    /* ... */
  };
}
```

## 10. All files should be named in `kebab-case` syntax

`tsx` or `jsx` will help us to understand if it's component file

```
  /
    home-button.tsx
    home.tsx
    home.api.ts
    home.constants.ts
    index.ts

```
