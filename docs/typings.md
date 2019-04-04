# Typings

[Return to Table of Contents](../README.md)

## 1. Types `any`, `array` and `object` not allowed

## 2. All interface should start from I and types from T

```typescript
  interface IData {
    /* ... */
  }

  type TField = /* ... */
```

## 3. Always use interface for object and types for one-line

```typescript
interface IUser {
  name: string;
  email: string;
  phone: number;
}

type TAnswer = "Yes" | "No";
```

## 4. Class or Component interface should contain it name

```typescript
interface IApiService {
  getUser(): IUser;
}

class ApiService implements IApiService {
  /* ... */
}
```

## 5. Always use interface `extends` if possible

```typescript
interface IData {
  name: string;
  surname: string;
}

interface IAdmin extends IData {
  /* ... */
}
```

## 6. Specify type values if it's predefined

> Instead of

```typescript
interface IData {
  level: number;
}
```

> Use

```typescript
type TLevel = 1 | 2;

interface IData {
  level: TLevel;
}
```

## 7. Use `?` when declaring non certain types

> Instead of

```typescript
interface IData {
  level: number | undefined;
}
```

> Use

```typescript
interface IData {
  level?: number;
}
```

## 8. Always use `private`, `public` and `protected` in class or component

```typescript
class Api {
  public getUser = () => {
    const token = this.getToken();
    /* ... */
  };

  private getToken = () => {
    /* ... */
  };
}
```

## 7. Create `typings` folder for storing common types and `typing.ts` file for extraction local types

```typescript
import { ICommonData } from "@typings/data";
import { ILocalInfo } from "./typings";
```

## 8. Create `global.d.ts` file if you need to override library types or create types for untyped module

```typescript
declare module "data" {
  export interface IPayload {
    name: string;
    password: string;
  }

  export function parseData(options: IPayload): void;
}
```

## 9. Type annotation should be on the top of the file

```typescript
  interface IService {
    getInfo: (token: string) => Promise<IData>;
  }
```

## 10. When using React always add typing for React Children

```typescript
  interface IComponent {
    children: ReactNode | ReactChild | ReactElement
  }

  Also it could be primitive `string | number | boolean`, array `Array<T>` or `null | never | undefined`

```
