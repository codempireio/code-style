# Typings

[Return to Table of Contents](../README.md)

### Types `any`, `array` and `object` not allowed

#### Always use interface for object and types for one-line

```typescript
interface IUser {
  name: string;
  email: string;
  phone: number;
}

type TAnswer = "Yes" | "No";
```

### Class or Component interface should contain its name

```typescript
interface IApiService {
  getUser(): IUser;
}

class ApiService implements IApiService {
  /* ... */
}
```

### Always use interface `extends` if possible

```typescript
interface IData {
  name: string;
  surname: string;
}

interface IAdmin extends IData {
  /* ... */
}
```

### Specify type values if it's predefined

> Instead of

```typescript
interface IData {
  level: number;
}
```

> Use

```typescript
interface IData {
  level: 1 | 2;
}
```

### Use `?` when declaring non-certain types

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

### Create `typings` folder for storing common types and `typing.ts` file for extraction local types

```typescript
import { ICommonData } from "@typings/data";
import { ILocalInfo } from "./typings";
```

### Create `global.d.ts` file if you need to override library types or create types for an untyped module

```typescript
declare module "data" {
  export interface IPayload {
    name: string;
    password: string;
  }

  export function parseData(options: IPayload): void;
}

// for fonts
declare module "*.ttf";
```

### Type annotation should be on the top of the file

```typescript
interface IService {
  getInfo: (token: string) => Promise<IData>;
}
```

### Don't forget about `Record` and `Partial`

> `Record` works good with object typings

```typescript
const hashMap: Record<string, string> = {
  name: "John",
  surname: "Dou",
  photoUrl: "...",
};
```

> Also could be used for strict object validation

```typescript
const hashMap: Record<'name' | 'surname' | 'photoUrl', string> = {
  name: "John",
  surname: "Dou",
  photoUrl: "...",
};
```

> `Partial` will help you with typization of big objects

```typescript
interface BigObject {
  getName(): string;
  getInfo(): Promise<Data>;
  name: string;
  photo: string;
}

const smallPart: Partial<BigObject> = {
  name: "John",
  photo: "...",
};
```
