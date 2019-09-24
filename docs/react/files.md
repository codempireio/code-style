# Files in React Ecosystem

[Return to Table of Contents](../../README.md)

[Return to React](../react.md)

> Structuring files in react is essential point of quality code!
> We're using next rules for keeping our code more maintainable

## File Naming

> All files should be named in `kebab-case` syntax
> `.tsx` or `.jsx` will help us to understand if it's component file

```
  location-service.js
  events-filter.jsx

  home.service.ts
  home.tsx
  home-header.tsx
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
    index.ts

    home-header.tsx

    /home-events
      home-events.tsx
      home-events.constants.ts
      index.ts
```

## Folder Structure

### `src/screens`

> Feature folder that contains all files according to - components, constants, typings, api etc.
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

### `src/components`

> Components (with local constants, typings) that are shared over several screens;

```
   /components
      /spinner
        spinner.tsx
        spinner.test.tsx
        spinner.constants.ts
        index.ts
      /modal
      text.tsx
```

### `src/constants`

> Contains constants that are shared over several scenes, components, services...

```
    /constants
      date-formats.ts
      routes.ts
      config.ts
      icons.ts
```

### `src/services`

> Services - contains application services: api classes, helper functions, redux

```

    /services
      /api
        api-service.ts
        api-service.test.ts
        api-service.constants.ts
      /helpers
        time-slots.ts
      location-service.ts
```

### `src/typings`

> Typings - contains shared typings

```
    /typings
      index.d.ts
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
