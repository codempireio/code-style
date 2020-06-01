# Tools

[Return to Table of Contents](../README.md)

## Must-Have Tools

> Useful tools that we commonly to using when developing projects in Codempire!

### - Prettier

[Prettier is an opinionated code formatter. It enforces a consistent style by parsing your code and re-printing it with its own rules that take the maximum line length into account, wrapping code when necessary.](https://www.npmjs.com/package/prettier)

#### .prettierrc

```json
{
  "tabWidth": 2,
  "useTabs": false,
  "semi": true,
  "singleQuote": true,
  "jsxSingleQuote": false,
  "bracketSpacing": true,
  "jsxBracketSameLine": false,
  "arrowParens": "always",
  "trailingComma": "es5",
  "endOfLine": "lf",
  "printWidth": 80
}
```

### - TSLint

[TSLint is an extensible static analysis tool that checks TypeScript code for readability, maintainability, and functionality errors.](https://www.npmjs.com/package/tslint)

### - Pre-commit hook tools

[Run linters against staged git files and don't let 💩 slip into your codebase!](https://www.npmjs.com/package/lint-staged)

```sh
  npm i -D lint-staged prettier tslint
```

#### package.json

```json
  "scripts": {
    "lint": "tslint 'src/**/*.{ts,tsx}'",
    "precommit": "lint-staged",
    "prettier": "prettier --config ./.prettierrc --write  \"./src/**/*.{ts,tsx}\""
  },
  "lint-staged": {
    "*.{ts,tsx}": [
      "yarn prettier",
      "yarn lint",
      "git add"
    ]
  }
```

### - axios

[Promise based HTTP client for the browser and node.js](https://www.npmjs.com/package/axios)

> It's the best feature is a possibility to modify XHR with interceptors

```typescript
const request = (url: string, method: string, body: {}) => {
    const instance = axios.create({
        baseURL: 'https://api.com',
        headers: {
          ['Content-Type']: 'application/json',
        },
      });
    }
    instance.interceptors.request.use((config: AxiosRequestConfig) => {
      return {
            ...config,
            headers: {
              ...config.headers,
              Authorization: `Bearer Token`,
            },
          };
    }));

    return instance.request(url, method, body);
}
```

### - moment

[A lightweight JavaScript date library for parsing, validating, manipulating, and formatting dates.](https://www.npmjs.com/package/moment)

### - styled-components

[Use the best bits of ES6 and CSS to style your apps without stress](https://www.npmjs.com/package/styled-components)

### - react-select

[Best Select component for React](https://www.npmjs.com/package/react-select)

## React Native Tools

### - react-native-vector-icons

[Customizable Icons for React Native with support for NavBar/TabBar/ToolbarAndroid, image source, and full styling.](https://www.npmjs.com/package/react-native-vector-icons)

### - react-native-dropdownalert

[A simple alert to notify users about new chat messages, something went wrong or everything is ok](https://www.npmjs.com/package/react-native-dropdownalert)

### - react-navigation

[Routing and navigation for your React Native apps](https://www.npmjs.com/package/react-navigation)

### - react-native-typescript-transformer

[Helps to create React Native project with typescript and module mapper feature](https://www.npmjs.com/package/react-native-typescript-transformer)
