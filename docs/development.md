# Git Flow

[Return to Table of Contents](../README.md)

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
