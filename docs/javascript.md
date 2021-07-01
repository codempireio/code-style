# Javascript ⚡

[Return to Table of Contents](../README.md)

## Table of Contents 🚀

  - [**Modules**](#modules)
  - [**Naming**](#naming)
  - [**Code Structure**](#code-structure)

## **Modules**

- ### **Don't use default export**

> Default export allows confusions in codebase
> To avoid this don't use it

```javascript
import { HomeApi } from "./home.api";
```

- ### **Use `index` for export module parts to another**

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

- ### **Use `import as` for utils/api functions to have one import item**

```javascript
// utils.js
const privateFunction = () => {};
export const formatTimeSlots = () => {};
export const updateTimeSlots = () => {};
export const createTimeSlots = () => {};
```

**Instead of** 🤨

```javascript
//file.js
import { formatTimeSlots, createTimeSlots, updateTimeSlots } from "./utils";
```

**You will have** 🥰

```javascript
//file.js
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
import { User } from "scenes/Account/User";
import { THEME_COLOR } from "constants/theme";
import { getUser } from "services/userServices";

// or with special import symbol

import { User } from "@scenes/Account/User";
import { THEME_COLOR } from "@constants/theme";
import { getUser } from "@services/userServices";
```

- ### **Try to keep import order**

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

## **Naming**

- ### **Don't use primitive naming**

**Instead of** 🤨

```javascript
members.map((el, k) => {
  el.friends.forEach((el2) => {
    /* ... */
  });
});
```

**You will have** 🥰

```javascript
members.map((member, index) => {
  member.friends.forEach((user) => {
    /* ... */
  });
});
```

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

- ### **Avoid tricky ternaries**

**Instead of** 🤨

```javascript
method = (count) => {
  count === 2 ? someFunction() : null;
  /* ... */
};

// and

const code =
  count === 3 ? (getPreviousCode() === 2 ? (!isOversize ? null : 4) : 5) : 3;
```

**You will have** 🥰

```javascript
method = (count) => {
  if (count === 2) {
    someFunction();
  }
  /* ... */
};

// and

const oversizeValue = !isOversize ? null : 4;
const previousValue = getPreviousCode() === 2 ? oversizeValue : 5;

const code = count === 3 ? previousValue : 3;
```

- ### **Use object mapping or switch/case instead of nesting `if`**

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

**Good Alternative** 😏

```javascript
const response = await api.fetchData();

switch (response) {
  case "saved":
  /* ... */
  case "updated":
  /* ... */
  case "error":
  /* ... */
}
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
