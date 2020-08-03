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

### - ESLint

[ESLint is an extensible static analysis tool that checks TypeScript code for readability, maintainability, and functionality errors.](https://www.npmjs.com/package/eslint)

1. Here you can see main rules from ESLint, but React Native and other libraries need additional parameters. So, copy these parameters to your .eslintrc file.

```json
{
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "project": "tsconfig.json",
    "sourceType": "module",
    "ecmaFeatures": {
      "jsx": true
    }
  },
  "plugins": ["react", "@typescript-eslint"],
  "extends": [
    "plugin:@typescript-eslint/eslint-recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:react/recommended"
  ],
  "root": true,
  "env": {
    "node": true
  },
  "rules": {
    "no-shadow": "error",
    "no-console": ["error", { "allow": ["warn", "error"] }],
    "curly": "error",
    "react/prop-types": "off",
    "react/display-name": "off",
    "@typescript-eslint/interface-name-prefix": "off",
    "@typescript-eslint/explicit-function-return-type": "off",
    "@typescript-eslint/no-explicit-any": "off",

    "default-case": "error", //require `default` cases in `switch` statements
    "no-alert": "error", //Disallow Use of  alert, confirm, and prompt
    "no-empty-function": "error" //Disallow empty functions
  },
  "settings": {
    "react": {
      "version": "detect"
    }
  }
}
```

2. In Setting found setting.json add this fields to end.

```json
  "editor.formatOnSave": true,
  "editor.rulers": [80],
  "eslint.workingDirectories": [
    {
      "mode": "auto"
    }
  ],
```

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

### Import auto sorter(Test setting version)

This tool automatically sort import to code-style rules(**Main part of import!**) after save file.

1. Install this [extension](https://marketplace.visualstudio.com/items?itemName=mike-co.import-sorter).
2. In Setting find > setting.json and add to end this JSON.

```json
  "importSorter.generalConfiguration.sortOnBeforeSave": true,
  "importSorter.importStringConfiguration.tabSize": 2,
  "importSorter.sortConfiguration.customOrderingRules.defaultNumberOfEmptyLinesAfterGroup": 1,
  "importSorter.importStringConfiguration.maximumNumberOfImportExpressionsPerLine.count": 80,
  "importSorter.sortConfiguration.customOrderingRules.rules": [
    {
      "regex": "^.?(screens|components).*(?<!.api|.utils|.state|.reducer|.actions|.constants|.strings|.typings|.styles|.controller|.service|.module)$",
      "orderLevel": 20
    },
    {
      "regex": "^.?services|.*.api$|.utils$|.state$|.reducer$|.actions$",
      "orderLevel": 30
    },
    {
      "regex": "^.?constants|.constants$|.strings$",
      "orderLevel": 40
    },
    {
      "regex": "^.?typings|.typings$",
      "orderLevel": 50
    },
    {
      "regex": "^.?styles|.?styles$|^themes$",
      "orderLevel": 60
    },
    {
      "regex": "^((?!(/|themes|theme)).)*$|(^@nestjs)",
      "orderLevel": 10
    }
  ],
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
