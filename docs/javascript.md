# Javascript

Return to content [Table of Contents](../README.md)

## 1. All `.js` files should not exceed 200 lines of code

## 2. Import Order

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
import Logo from "../assets/icons/logo.svg";
```

## 3. Default export not allowed

> ApiService.js

```javascript
  export const class ApiService {
    /* ... */
  }
```

> Home.js

```javascript
import { ApiService } from "../ApiService";
```

## 4. Use `index.js` with only export declaration to ease import

> Home/Home.js

```javascript
 export const Component = () => /* ... */
```

> Home/index.js

```javascript
export * from "./Component";
```

> User/User.js

```javascript
import { Component } from "../Home";
```

## 5. Setup module name mapper in webpack, tsconfig, jest to ease import

> Instead of

```javascript
  import { User } from '../../scenes/Account/User';
  import { THEME_COLOR } '../../constants/theme';
  import { getUser } '../../services/userServices';
```

> Will be

```javascript
  import { User } from '@scenes/Account/User';
  import { THEME_COLOR } '@constants/theme';
  import { getUser } '@services/userServices';
```

## 6. Boolean variables should begin from `is`

```javascript
const isCompleted = method();
```

## 7. If statement should be as small as possible

```javascript
method = (isCompleted, options) => {
  if (!isCompleted) {
    return null;
  }

  /* ... */
};
```

## 8. Use ternary if possible to do it in one line

```javascript
return isCompleted ? method() : null;
```

## 9. Move complex boolean calculation to variable

```javascript
const isCompleted = status === 500 && !isLoading && !isError;
```

## 10. Use hashmap instead of nesting `if`

> Instead of:

```javascript
if (isOk) {
  if (status === 200) {
    /* ... */
  } else if (status === 201) {
    /* ... */
  }
  /* ... */
}
```

> Use:

```javascript
  const object = {
   isOk: () => /* ... */,
   isCreated: () => /* ... */,
   isUpdated: () => /* ... */
  }
```

## 11. Always use object desctruction

```javascript
const { user, options } = response;
// two times if needed
const { name, surname, photo } = user;
```

## 12. Name variables to fit object shorcut

```javascript
createUser = options => {
  const { name, photo } = api.request({ options });

  return {
    name,
    photo
  };
};
```

## 13. Constant should always be written in uppercase

```javascript
const NEW_MODE = "New Mode";
const GOOGLE_KEY = "123123123123";
```

## 14. Always extract string and number to constants (local or global)

```javascript
const MODAL_NAME = "Open Modal";
const MODAL_TEXT = "Do you ...?";

/* ... */

createModal(MODAL_NAME, MODAL_TEXT);
```

## 15. Class method should be defined only in one syntax

```javascript
class Api {
  request = options => {
    /* ... */
  };

  getUser = () => {
    return this.user;
  };
}
```

## 16. For helper functions prefer to use classes instead of several functions

```javascript
  class TimeSlotHelper {
    formatTimeSlots = () => /* ... */
    updateTimeSlots = () => /* ... */
    createTimeSlots = () => /* ... */
  }
```

## 17. Function and method should have correct name

> Only return untouched something

```javascript
  getSomething = () => /* ... */
```

> Formatting something

```javascript
  formatSomething = () => /* ... */
```

## 18. Use `async/await` instead of `.then`

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
