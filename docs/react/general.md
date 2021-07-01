# General React rules

[Return to Table of Contents](../../README.md)

[Return to React](README.md)

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

## Use import * as for styled-components

> component.styles.ts

```javascript
import styled from "styled-components";

export const Wrapper = styled.div`
    display: flex;
  `;

  export const Modal = styled.div`
    width: 240px;
    height: 250px;
  `;
  export const Text=  styled.p`
    font-size: 16px;
  `,
};
```

> component.ts

```javascript
  import * as Styled from './component.styles.ts';


  /* ... */

  <Styled.Wrapper>
    <Styled.Modal>
      <Styled.Text/>
    </Styled.Modal>
  </Styled.Wrapper>
```

## Always prefer function component

```javascript
export const User = (props) => <div {...props} />;
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

## Update state data with option function

> in state file

```typescript
const onUpdateUser = (value: string, option: string) => {
  setState((s) => ({
    ...s,
    user: {
      ...s.user,
      [option]: value,
    },
  }));
};
```

> in input component

```typescript
interface IProps {
  option: string;
  value: string;
  onUpdateUser: (value: string, option: string) => void;
}

/* ... */

const onChangeText = (text: string) => {
  props.onUpdateUser(text, props.option);
};
```
