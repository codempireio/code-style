# Javascript

[Return to Table of Contents](../README.md)

## Table of Contents 🚀

  - [**Modules**](#modules)
  - [**Naming**](#naming)
  - [**Code Structure**](#code-structure)

## **Modules**

- ### **Follow next import order**

> This spaces allow you quicker understand what files use your module

```javascript
// node_modules
import * as React from "react";
import { Link } from "react-router-dom";
// components or classes
import { Home } from "./scenes/Home";
import { Collapse } from "../shared/components";
// helper or services
import { sendMessage } from "../services/api";
import { createId } from "../services/helper";
// constants
import { USER_ROLE } from "../constants/user";
import { APP_NAME } from "./constants";
// typings
import { IHome } from "./scenes/Home/typings";
import { ISendMessageResponse } from "../services/api/typings";
// files
import "./scenes/Home.css";
```

- ### **Don't use default export**

> Default export allows confusions in codebase
> To avoid this don't use it

```javascript
import { HomeApi } from "./home.api";
```

- ### **Use `index.js` for export module parts to another**

**Instead of** 🤨

```javascript
import { homeActions } from "../../home/home-utils/home.actions";
import { routeUtils } from "../../../utils/route.utils";

import { homeTypes } from "../../home/home.typings";
```

**You will have** 🥰

```javascript
import { homeActions, homeTypes } from "../../home";
import { routeUtils } from "../../../utils";
```

> Put everything that you want to share to index.js

```javascript
export * from "./home";
export { homeActions } from "./home-utils/home-actions";
```

- ### **Combine utils/api functions under one object**

**Instead of** 🤨

```javascript
// utils.js
const formatTimeSlots = () => {};
const updateTimeSlots = () => {};
const createTimeSlots = () => {};

//file.js

import { formatTimeSlots, createTimeSlots, updateTimeSlots } from "./utils";
```

**You will have** 🥰

```javascript
// utils.js
const utils = {
  formatTimeSlots: () => {},
  updateTimeSlots: () => {},
  createTimeSlots: () => {},
};

//file.js

import { utils } from "./utils";
```

**Alternative** 😎

```javascript
import * as utils from "./utils";
```

- ### **Use module name wrapper enhance your import from other modules**

**Instead of** 🤨

```javascript
import { User } from "../../../scenes/Account/User";
import { THEME_COLOR } from "../../constants/theme";
import { getUser } from "../services/userServices";
```

**You will have** 🥰

```javascript
import { User } from "@scenes/Account/User";
import { THEME_COLOR } from "@constants/theme";
import { getUser } from "@services/userServices";
```

## **Naming**

- ### **Boolean variables or methods with boolean return value should start from `is`**

```javascript
const isCompleted = true;
const isValidUser = (user) => !!user;
```

- ### **Constants should always be written in uppercase**

```javascript
const NEW_MODE = "New Mode";
const GOOGLE_KEY = "1da541ac298";
const IS_PRODUCTION = process.env.NODE_ENV === "production";
```

- ### **Creating several variables will make your code more readable**

**Instead of** 🤨

```javascript
const result = (income - expenses) - income > expenses * 2 ? income * 0,25: 0;
```

**You will have** 🥰

```javascript
const value = income - expenses;
const isUseTaxes = income > expenses * 2;
const taxes = isUseTaxes ? income * 0,25 : 0;
const result = value - taxes;
```

- ### **Using `object destruction` will readability a lot**

**Instead of** 🤨

```javascript
setState({
  account: `${response.user.name} ${response.user.surname}`,
  photo: response.user.photo,
  options: response.options,
});
```

**You will have** 🥰

```javascript
const { user, options } = response;
// two times if needed
const { name, surname, photo } = user;

setState({
  account: `${name} ${surname}`,
  photo,
  options,
});
```

- ### **Try to name arguments and use correct destructions to use object shortcuts**

**Instead of** 🤨

```javascript
createUser = (params) => {
  const { userName, photo } = api.request({ options: params });

  return {
    name: userName,
    photo: photo,
  };
};
```

**You will have** 🥰

```javascript
createUser = (options) => {
  const { userName: name, photo } = api.request({ options });

  return {
    name,
    photo,
  };
};
```

## **Code Structure**

- ### **Avoid code nesting and make `if` statement small**

**Instead of** 🤨

```javascript
method = (isCompleted, options) => {
  if (isCompleted) {
    const params = format(options);
    if (params.isCreated) {
      return params.date;
    }
    /* ... */
  }

  return null;
  /* ... */
};
```

**You will have** 🥰

```javascript
method = (isCompleted, options) => {
  if (!isCompleted) {
    return null;
  }

  /* ... */
};
```

- ### **Ternary helps you to reduce code lines**

**Instead of** 🤨

```javascript
if (isCompleted) {
  return method();
}
return null;
```

**You will have** 🥰

```javascript
return isCompleted ? method() : null;
```

- ### **Use object mapping instead of nesting `if`**

**Instead of** 🤨

```javascript
const response = await api.fetchData();

if (response.message === "saved") {
  /* ... */
}
if (response.message === "updated") {
  /* ... */
}
if (response.message === "error") {
  /* ... */
}
```

**You will have** 🥰

```javascript
  const responseHandleMap = {
   saved: () => /* ... */,
   updated: () => /* ... */,
   error: () => /* ... */
  }
  /* ... */
  const response = await api.fetchData();
  responseHandleMap[response.message]();

```

- ### **Always extract string and number to constants (local or global)**

```javascript
export const strings = {
  modal: {
    name: "Save Data",
    info: "Do you ...?",
  },
};

/* ... */
import { strings } from "@constants/strings";

const { name, info } = strings.modal;
createModal(name, info);
```

- ### **Use `async/await` instead of `then/catch`**

**Instead of** 🤨

```javascript
getUserInfo()
  .then(({ user, options }) => {
    /* ... */
  })
  .catch((e) => {
    /* ... */
  });
```

**You will have** 🥰

```javascript
request = async () => {
  try {
    const { user, options } = await getUserInfo();
    /* ... */
  } catch (e) {
    /* ... */
  }
};
```
