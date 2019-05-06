# Tools

[Return to Table of Contents](../README.md)

## 1. Prettier

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

## 2. TSLint

[TSLint is an extensible static analysis tool that checks TypeScript code for readability, maintainability, and functionality errors.](https://www.npmjs.com/package/tslint)

## 3. Precommit tools

[Run linters against staged git files and don't let 💩 slip into your code base!](https://www.npmjs.com/package/lint-staged)

```sh
  npm i -D lint-staged prettier tslint
```

### package.json

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