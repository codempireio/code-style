# Files in React Ecosystem

[Return to Table of Contents](../../README.md)

[Return to React](README.md)

> Structuring files in react is essential point of quality code!
> We're using next rules for keeping our code more maintainable

## Table of Contents

1. [File Naming](#file-naming)
2. [Folder Structure](#folder-structure)
3. [Constants folder](#constants-folder)

## File Naming

> All files should be named in `kebab-case` syntax
> `.tsx` or `.jsx` will help us to understand if it's component file

```
  users-panel.api.js
  events-filter.jsx

  home.tsx
  home-header.state.tsx
```

> Secondary files (helpers, constants, services) in same folder should name with `.`

```
 /home
    index.ts
    home.ts
    home.utils.ts
    home.constants.tsx
    home.state.ts
```

> Secondary components should be kept near main component as file or keep in separate folder if it has secondary files also

```
  /home
    home.state.ts
    home.tsx
    home.constants.ts
    home.actions.ts
    home.reducer.ts
    home.saga.ts
    index.ts

    home-header.tsx

    /home-events
      home-events.tsx
      home-events.constants.ts
      index.ts
```

## Folder Structure

### `src/screens`

> Feature folder that contains all files according to - components, constants, typings, api, Redux part (reducers, actions, saga) etc.
> `index.ts` file inside each folder is using as an export point. We should import files only from it.

```
    /screens
      /home
        /header
          header.tsx
          index.ts
        /footer
          footer.tsx
          footer.styles.ts
          footer.test.tsx
          index.ts
        home.tsx
        home.actions.ts
        home.api.ts
        home.constants.ts
        home.reducer.ts
        home.state.ts
        home.styles.ts
        home.test.tsx
        home.typing.ts
        index.ts
      /auth
      /user
```

### `src/components`

> Components (with local constants, typings) that are shared over several screens, always without redux or api;

```
   /components
      /modal
        modal.tsx
        modal.test.tsx
        modal.constants.ts
        index.ts
      /spinner
      text.tsx
```

### `src/constants`

> Contains constants that are shared over several scenes, components, services...

```
    /constants
      date.ts
      routes.ts
      env.ts
      config.ts
      images.ts
```

### `src/services`

> Services - contains application services: api classes, helper functions, redux setup

```

    /services
      /api
        api-service.ts
        api-service.test.ts
        api-service.constants.ts
        index.ts
      /helpers
        time-slots.ts
      store.ts
      storage-service.ts
      location-service.ts
      /redux
        store.ts
        utils.ts
        index.ts
```

### `src/typings`

> Typings - contains shared typings

```
    /typings
      index.d.ts // global typings
      api.ts
      items.ts
```

### `src/assets`

> Assets - contains images, fonts, icons

```
    /assets
      /img
        background.png
        user.png
        table.png
      /fonts
        Arial.woff
        Roboto.ttf
      /icons
        avatar.svg
        settings.svg
```

### `./`

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

## Constants folder

> It's a good place to keep your constants that you're using in project

### `config.ts`

> your application config data: base api url, host name, google credentials, etc

```typescript
export const BASE_URL = "api.com";
export const AUTH_URL = "identity.api.com";

export const Env = {
  isProduction: process.env.NODE_ENV === "production",
};
```

### `date-formats.ts`

> moment formats that you're using across application

```typescript
export const HOURS_FORMAT = "h a";
export const DATE_FORMAT = "dddd Do MMMM";
export const DATE_YEAR_FORMAT = "dddd Do MMMM, YYYY";
```


### `images.ts`

> here you can export your images from assets

```typescript
export const images = {
  logo: require("../assets/images/logo.png"),
  background: require("../assets/images/background.png"),
};

export const icons = {
  user: require("../assets/icons/user.svg"),
}
```

### `routes.ts`

> routes of our application

```typescript
export enum ROUTES {
  Home = "Home",
  Auth = "Auth",
  Settings = "Settings",
}
```
