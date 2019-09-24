# Using React Hooks

[Return to Table of Contents](../../README.md)

[Return to React](README.md)

> We're using React hooks for managing react state. Usually we separate hooks file to separate `.state.ts` file

## Table of Contents

1. [State Hook](#state-hook)
2. [Context Hook](#context-hook)

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

## Context hook

> When you need to share data between several screens you need to use React.Context with useContext hook

### user-context.tsx

> Will use this provider inside app than will consume it with `useContext` hook in child components

```typescript
import * as React from "react";

import { useUserContextState, IUserContext } from "./user-context.state";

export const UserContext = React.createContext<IUserContext>({
  user: null,
  setUser: () => {
    return;
  },
  clearUser: () => {
    return;
  }
});

interface IUserProvider {
  children?: React.ReactNode;
}

export const UserProvider = (props: IUserProvider) => {
  const { user, setUser, clearUser } = useUserContextState();

  return (
    <UserContext.Provider
      value={{
        user,
        setUser,
        clearUser
      }}
    >
      {props.children}
    </UserContext.Provider>
  );
};
```

### user-context.state.tsx

> Logic of our context

```typescript
import { useState } from "react";

import { IUser } from "@typings/items";

export interface IUserContextData {
  user: IUser | null;
}

export interface IUserContext extends IUserContextData {
  setUser(user: IUser | null): void;
  clearUser(): void;
}

const initialState: IUserContextData = {
  user: null
};

export const useUserContextState = () => {
  const [state, setState] = useState<IUserContextData>(initialState);

  const setUser = (user: IUser | null) => {
    setState(() => ({
      user
    }));
  };

  const clearUser = () => {
    setState(() => ({
      user: null
    }));
  };

  return {
    ...state,
    setUser,
    clearUser
  };
};
```
