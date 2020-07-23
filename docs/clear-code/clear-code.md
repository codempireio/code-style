# Clear code

[Return to Table of Contents](../../README.md).

[Return to Clear code](./README.md).

## Table of Contents 🚀

- [**Variables**](#Variables)
- [**Function**](#Function)
- [**Formatting**](#Formatting)

## Variables

### Use meaningful and meaningful names

👎**Bad:**

```ts
const d = moment().format("YYYY/MM/DD");
```

👍**Good:**

```ts
const currentDate = moment().format("YYYY/MM/DD");
```

### Don't use magic number

👎**Bad:**

```ts
// What the heck is 86400000 for?
setTimeout(blastOff, 86400000);
```

👍**Good:**

```ts
// Declare them as capitalized named constants.
const MILLISECONDS_IN_A_DAY = 86400000;

setTimeout(blastOff, MILLISECONDS_IN_A_DAY);
```

### Use explanatory variables

👎**Bad:**

```ts
const address = "Some address";
const cityZipCodeRegex = /^\d{5}-\d{4}|\d{5}|[A-Z]\d[A-Z] \d[A-Z]\d$/;
saveCityZipCode(
  address.match(cityZipCodeRegex)[1],
  address.match(cityZipCodeRegex)[2]
);
```

👍**Good:**

```ts
const address = "Some address";
const cityZipCodeRegex = /^\d{5}-\d{4}|\d{5}|[A-Z]\d[A-Z] \d[A-Z]\d$/;
const [_, city, zipCode] = address.match(cityZipCodeRegex) || [];
saveCityZipCode(city, zipCode);
```

### Avoid intuitive mappings

👎**Bad:**

```ts
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach((l) => {
  doSomething(l);
});
```

👍**Good:**

```ts
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach((location) => {
  doSomething(location);
});
```

### Don't add unnecessary context

👎**Bad:**

```ts
const Job = {
  jobCompany: "codempire",
  jobStack: "Front-end developer",
  jobTime: 8,
};

const changeStack = (job) => {
  job.jobStack = "FullStack";
};
```

👍**Good:**

```ts
const Job = {
  company: "codempire",
  stack: "Front-end developer",
  time: 8,
};

const changeStack = (job) => {
  job.stack = "FullStack";
};
```

### Use default value in function

👎**Bad:**

```ts
const createFullName = (firstName?: string, lastName?: string) => {
  const fName = firstName || "Name";
  // ...
};
```

👍**Good:**

```ts
const createFullName = (firstName = "Name", lastName = "Surname") => {
  // ...
};
```

## Function

### The function should complete only one task.

👎**Bad:**

```ts
const getActiveClientEmails = (clients: IClient[]) => {
  clients.forEach((client) => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
};
```

👍**Good:**

```ts
const getActiveClientEmails = (clients: IClient[]) =>
  clients.filter(isActiveClient).forEach(email);

const isActiveClient = (client: IClient) => {
  const record = database.lookup(client);
  return record.isActive();
};
```

### Names should describe what the function does

👎**Bad:**

```ts
function addToDate(date: Date, month: number) {
  // ...
}

const date = new Date();
// It's hard to to tell from the function name what is added
addToDate(date, 1);
```

👍**Good:**

```ts
function addMonthToDate(month: number, date: Date) {
  // ...
}

const date = new Date();
addMonthToDate(1, date);
```

### Don't use the flag as a parameter

👎**Bad:**

```ts
const createFile = (name: string, temp: boolean) => {
  temp ? fs.create(`./temp/${name}`) : fs.create(name);
};
```

👍**Good:**

```ts
const createFile = (name: string) => {
  fs.create(name);
};

const createTempFile = (name: string) => {
  createFile(`./temp/${name}`);
};
```

### Avoid negative conditions

👎**Bad:**

```ts
function isDOM(node: any): node is HTMLElement {
  // ...
}

if (!isDOM(node)) {
  // ...
}
```

👍**Good:**

```ts
function isNotDOM(node: any): boolean {
  // ...
}

if (isNotDOM(node)) {
  // ...
}
```

### The ideal number of arguments for functions is two or less

👎**Bad:**

```ts
const createMenu = (
  title: string,
  body: string,
  buttonText: string,
  cancellable: string
) => {
  // ...
};
```

👍**Good:**

```ts
interface ICreateMenuParameters {
  title: string;
  body: string;
  buttonText: string;
  cancellable: string;
}

const createMenu = (parameters: ICreateMenuParameters) => {
  const { title, body, buttonText, cancellable } = parameters;
  // ...
};

createMenu({
  title: "Foo",
  body: "Bar",
  buttonText: "Baz",
  cancellable: true,
});
```

### Use currying if some variable is identical

👎**Bad:**

```ts
const createTemplate = (folderName: string, fileName: string) =>
  `/${foldername}/${fileName}`;

createTemplate("react", "index");
createTemplate("react", "state");
```

👍**Good:**

```ts
const createTemplate = (folderName: string) => (fileName: string) =>
  `/${foldername}/${fileName}`;

const createReactTemplate = createTemplate("react");

createReactTemplate("index");
createReactTemplate("state");
```

### Global functions are not overridden

👎**Bad:**

```ts
Array.prototype.diff = function diff(comparisonArray) {
  const hash = new Set(comparisonArray);
  return this.filter((elem) => !hash.has(elem));
};
```

👍**Good:**

```ts
class SuperArray extends Array {
  diff(comparisonArray) {
    const hash = new Set(comparisonArray);
    return this.filter((elem) => !hash.has(elem));
  }
}
```

### Delete unusable code

👎**Bad:**

```ts
// Bad
function oldRequestModule(url: string) {
  // ...
}

function newRequestModule(url: string) {
  // ...
}

const req = newRequestModule;
inventoryTracker("apples", req, "test-url");
```

👍**Good:**

```ts
function newRequestModule(url: string) {
  // ...
}

const req = newRequestModule;
inventoryTracker("apples", req, "test-url");
```

## Formatting

### Comment only code containing complex business logic

👎**Bad:**

```ts
function hashIt(data: string) {
  // The hash
  let hash = 0;

  // Length of string
  const length = data.length;

  // Loop through every character in data
  for (let i = 0; i < length; i++) {
    // Get character code.
    const char = data.charCodeAt(i);
    // Make the hash
    hash = (hash << 5) - hash + char;
    // Convert to 32-bit integer
    hash &= hash;
  }
}
```

👍**Good:**

```ts
function hashIt(data: string) {
  let hash = 0;
  const length = data.length;

  for (let i = 0; i < length; i++) {
    const char = data.charCodeAt(i);
    hash = (hash << 5) - hash + char;

    // Convert to 32-bit integer
    hash &= hash;
  }
}
```

### Don't comment out unnecessary code

👎**Bad:**

```ts
doStuff();
// doOtherStuff();
// doSomeMoreStuff();
// doSoMuchStuff();
```

👍**Good:**

```ts
doStuff();
```

### Never keep a comment journal

👎**Bad:**

```ts
/**
 * 2016-12-20: Removed monads, didn't understand them (RM)
 * 2016-10-01: Improved using special monads (JP)
 * 2016-02-03: Removed type-checking (LI)
 * 2015-03-14: Added combine with type-checking (JR)
 */
function combine(a: number, b: number) {
  return a + b;
}
```

👍**Good:**

```ts
function combine(a: number, b: number) {
  return a + b;
}
```

### Don't use position markers

👎**Bad:**

```ts
////////////////////////////////////////////////////////////////////////////////
// Scope Model Instantiation
////////////////////////////////////////////////////////////////////////////////
$scope.model = {
  menu: "foo",
  nav: "bar",
};

////////////////////////////////////////////////////////////////////////////////
// Action setup
////////////////////////////////////////////////////////////////////////////////
const actions = function () {
  // ...
};
```

👍**Good:**

```ts
$scope.model = {
  menu: "foo",
  nav: "bar",
};

const actions = function () {
  // ...
};
```
