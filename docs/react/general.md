# General React rules

[Return to Table of Contents](../../README.md)

[Return to React](../react.md)

> General rules for working with react

## Don't use anonymous function in events props

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

## Use Pure Component or Stateless Component if possible

```javascript
export class Home extends PureComponent {
  /* ... */
}

export const User = props => <User {...props} />;
```

## Divide complex render function to several functions or move it to another components

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

## Extract complex logic from Component to another modules

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

## Use constructor only for assigning props to state

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

> as an excepting

```javascript
class Component extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      isOpen: props.isOpened,
    };
  }
}
```
