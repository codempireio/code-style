# Development

[Return to Table of Contents](../README.md)

## Table of Contents 🏆

- [**Environment Setup**](#environment)
- [**JavaScript Tools**](#javascript-tools)
- [**React Native Tools**](#react-native-tools)

## Environment

> We have special rules to setup env variables on our javascript projects
> For providing variables we have `.env` file in the root of the project
> For sharing this variables through a project we creating `config.ts` file
> For launch in different env we're creating special npm script
> Also we're using third party libraries which helps us to create correct environment [cross-env](https://www.npmjs.com/package/cross-env) and [dotenv](https://www.npmjs.com/package/dotenv)

### .env

> File with all env variables

```
API_URL=http://server.com/api
API_URL_DEV=http://localhost:6000

MONGO_NAME=prod
MONGO_PASSWORD=prod-password

MONGO_NAME_DEV=test
MONGO_PASSWORD_DEV=password

JWT_SECRET=secret

```

### config.ts

> How we handle different env in code using Single Responsibility Principle

```typescript
interface IConfig {
  apiUrl: string;
  zoomId: string;
  zoomRedirectUrl: string;
}

const mode: string = process.env.NODE_ENV || "development";

const configMap: IHashMap<IConfig> = {
  development: {
    apiUrl: process.env.API_URL_DEV,
    mongoName: process.env.MONGO_NAME_DEV,
    mongoPassword: process.env.MONGO_PASSWORD_DEV,
    jwtSecret: process.env.JWT_SECRET,
  },
  production: {
    apiUrl: process.env.API_URL,
    mongoName: process.env.MONGO_NAME,
    mongoPassword: process.env.MONGO_PASSWORD,
    jwtSecret: process.env.JWT_SECRET,
  },
};

export const CONFIG = configMap[mode];
```

### package.json

> Usual npm start should launch with dev environments to prevent random production launch

```json
 "scripts": {
    "start": "react-scripts start",
    "start:prod": "cross-env NODE_ENV=production react-scripts start",
    "build": "react-scripts build",
  },
```

## Javascript Tools

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

[TSLint is an extensible static analysis tool that checks TypeScript code for readability, maintainability, and functionality errors.](https://www.npmjs.com/package/eslint)

### - ESLint

[ESLint is a tool for identifying and reporting on patterns found in ECMAScript/JavaScript code.](https://www.npmjs.com/package/tslint)

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
