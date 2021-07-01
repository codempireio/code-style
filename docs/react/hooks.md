# Using React Hooks

[Return to Table of Contents](../../README.md)

[Return to React](README.md)

> We're using React hooks for managing react state. Usually we separate hooks file to separate `.state.ts` file

## State hook

> Regular useState approach will have next look

### `component.state.ts`

```typescript
import { useState, useEffect } from 'react';

import { fetchDate } from './component.api';
import { formatUser, handleError } from './component.utils';

import { IUser } from './component.typings';

// typing for our hook state data
interface IComponentStateData {
  users: IUser[];
  selectedUser: IUser | null;
  error: string
  isLoading: boolean
}

// typing for all our hook
interface IComponentState extends IComponentStateData {
  onSelectUser: (user: IUser) => void;
}

const initialState = {
  users: [];
  selectedUser: null;
  error: '',
  isLoading: true,
}

export const useComponentState = (id: string): IComponentState => {
  const [state, setState] = useState<IComponentStateData>(initialState);

  // Using instead componentDidMount
  useEffect(() => {
    const fetchUsers = async () => {
      try {
        const users = await fetchData(id);

        setState(currentState => ({
          ...currentState,
          users: formatUsers(users),
          isLoading: false,
        }))
      } catch (e) {
        setState(currentState => ({
          ...currentState,
          error: handleError(e),
          isLoading: false,
        }))
      }
    }

    fetchUsers();
  }, []);


  // regular method that we can use for onClick
  const onSelectUser = (selectedUser: IUser) => {
    setState(currentState => ({
      ...currentState,
      selectedUser,
    }))
  }

  // returning data and needed method from hook
  return {
    ...state,
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