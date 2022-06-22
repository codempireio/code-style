# Using React Hooks

[Return to Table of Contents](../../README.md)

[Return to React](README.md)

> We're using React hooks for managing react state. Usually we separate hooks file to separate `.state.ts` file

## State hook

> As useState hook use `Object.is` to compare, it needs to pass new data everytime
>
> So each hook should contain one state. If state uses as object, every change of the field, will trigger re-render of all components that depends on that state, so only if it used by one component at the time, then it make sense to use object.
>
> Regular useState approach will have next look

### `component.state.ts`

```typescript
import { useState, useEffect } from 'react';

import { fetchDate } from './component.api';
import { formatUser, handleError } from './component.utils';

import { IUser } from './component.typings';

// Typing for our hook state data
interface IComponentStateData {
  users: IUser[];
  isLoading: boolean
}

// Typing for our hook return values
interface IComponentState extends IComponentStateData {
  error: string;
  selectedUser: IUser;
  onSelectUser: (user: IUser) => void;
}

const initialState = {
  users: [];
  isLoading: true,
}

// Custom hook
export const useComponentState = (id: string): IComponentState => {
  const [state, setState] = useState<IComponentStateData>(initialState);
  const [selectedUser, setSelectedUser] = useState<IUser | null>(null);
  const [error, setError] = useState<string>('');

  // Using instead componentDidMount
  useEffect(() => {
    const fetchUsers = async () => {
      try {
        const users = await fetchData(id);

        // Every setState we pass new object
        setState(currentState => ({
          ...currentState, // Just as example how to save previous state
          users: formatUsers(users),
          isLoading: false,
        }))
      } catch (e) {
        // As it update every field on state, no need to pass previous one
        setState(currentState => ({
          error: handleError(e),
          isLoading: false,
        }))
      }
    }

    fetchUsers();
  }, []);


  // Regular method that we can use for onClick
  const onSelectUser = (selectedUser: IUser) => {
    setSelectedUser(selectedUser)
  }

  // Returning data and needed method from hook
  return {
    ...state,
    error,
    selectedUser,
    onSelectUser
  }
}

```

> And will use it in next way

### `component.tsx`

```typescript
import * as React from "react";

import { Spinner } from '@components/spinner";

import { useComponentState } from "./component.state";

import { IUser } from "./component.typings";

export interface IComponent {
  id: string;
}

export const Component = (props: IComponent) => {
  const { users, isLoading, users, selectedUser, onSelectUser } = useComponentState(props.id);

  if (isLoading) {
    return (<Spinner />)
  }
  /* ... */
};
```
