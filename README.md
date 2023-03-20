# VMOTEST
# NODEJS
# Agenda

- Introduction
- Content
  - Simple rules
    - Naming convention
      - camel, pascal, snake, kebab
    - Formatting
      - Spacing
      - Line break
      - Others
    - Es6
    - Others standard rules (Airbnb)
  - Project rules
    - ExpressJS source code structure
    - Catch and throw exception
    - Logging
    - Design Pattern
- Summary

# A. Introduction

## I. Overview

## II. Why coding convention is important?

Coding Conventions ensure quality of source code:

- More readability
- Easier to maintenance
- Collaborating between developers is convenience

# B. Content

## I. Simple rule

### 1. Naming convention

#### Style case:

- camelCase
- PascalCase
- snake_case
- kebab-case

=> use for naming:

- variables, properties, functions, methods, class, folders, files
  == **Meaningful** name

- `Use camelCase for variables, properties, function and method names`

- `Use PascalCase for class name`

- `Use SNAKE_UPPER_CASE for constant`

- `kebab-case for folder and file name`

#### Example

```javascript
// Variables
let totalUsers = db.query('SELECT COUNT(*) FROM users'); // Right
let total_users = db.query('SELECT COUNT(*) FROM users'); // Wrong

// Classes - is a noun
class ShoppingCart {
  // properties
  items = [];
  // method name - is a verb
  addItem(item) {}
  getCart() {}
  deleteItem() {}

  // mark as private method
  _validateItem(item) {
    return true;
  }
}

class Order {}

class OrderItem {}

// Constants
const SECOND_IN_DAY = 86400;
const DATABASE_NAME = 'Mongodb';
const DATABASE_PORT = 27017;
// Message constants
const CREATE_ORDER_SUCCESS = 'Order is created';
const CREATE_ORDER_FAIL = 'Create order fail';

async function createUser(id, info) {
  // Inner function with underscore prefix (_)
  function _checkUserExist() {
    // something
  }

  const userExist = _checkUserExist(id);
  // something
  return true;
}
```

### 2. Formatting

#### a. `Spacing`

- 2 or 4 space is OK -> that's on u
- Never use TAB (-> use eslint to config - see below)

`HOW?`

- VsCode config:
  "editor.tabSize": 4,
  "editor.insertSpaces": true,
  "editor.detectIndentation": true
- Eslint: [link](https://eslint.org/docs/rules/indent#enforce-consistent-indentation-indent)

#### b. `Line break` (EOL- end of line)

- unix style: `\n` (LF)
- windows style: `\r\n` (CRLF)

--> should (must) choose LF

- Server deploy usually is Linux, Ubuntu (Unix system)

`HOW?`

- Eslint: [link](https://eslint.org/docs/rules/linebreak-style)
- VSC GIT:
  > git config --global core.autocrlf false

#### c. Others

- [More formatting rules](https://medium.com/swlh/node-js-coding-style-guidelines-74a20d00c40b)
  - `No trailing white space`
  - `Use semicolons`
  - `80 characters per line`
  - `Use single quotes`
  - ...

### 3. Follow new version javascript (prefer ES6 [7, 8])

[Ref](https://ehkoo.com/bai-viet/tong-hop-tinh-nang-noi-bat-es6)

- `let`, `const`
- template string
- [Promise](https://kipalog.com/posts/Promise-la-khi-gi-)

### 4. Reference to other standard rules

- Eslint default rules
- Otherwise, can follow some eslint standard from airbnb, shopify [link](https://vinasupport.com/lua-chon-chuan-coding-conventions-cho-du-an-javascript/)

  ```javascript
  /** Airbnb example rules **/

  /* No new object */
  // bad
  const item = new Object();
  // good
  const item = {};

  /* Object spread syntax */
  // very bad
  const original = { a: 1, b: 2 };
  const copy = Object.assign(original, { c: 3 }); // this mutates `original` ಠ_ಠ
  delete copy.a; // so does this
  // bad
  const original = { a: 1, b: 2 };
  const copy = Object.assign({}, original, { c: 3 }); // copy => { a: 1, b: 2, c: 3 }
  // good
  const original = { a: 1, b: 2 };
  const copy = { ...original, c: 3 }; // copy => { a: 1, b: 2, c: 3 }
  const { a, ...noA } = copy; // noA => { b: 2, c: 3 }

  /* Array constructor */
  // bad
  const items = new Array();
  // good
  const items = [];
  ```

## II. Project rules

### 1. ExpressJS source code structure

[Ref](http://anixir.com/minimal-node-express-style-guide/)

#### a. Directory Structure

- `Middleware`: (Pipelines)

  - Use for:

    - Logging request
    - Authentication/Authorization
    - Validate input (request query, param, body)

      **\-Controller**

    - Format response
    - Catch error

- `Routes`:

  - Prefix with `Router`;
  - Use for: Arrange, routing requests
  - Interact: Controller layer only (maybe middleware)

- `Controllers`

  - Prefix with `Controller`
  - Use for:
    - Validate/Formatting input data
    - Call services to get/modify data
    - Response to client
  - Interact with service layer only

- `Services`:

  - Prefix `Service`
  - Use for:
    - Handle all business logic (complex) of system
  - Interact with:

    - model (or repository) of that object (module)
    - or with service of other object (module)
    - Utils/Common

- `Repository`

  - Prefix with`Repository`
  - Use for:
    - Write queries to get/modify data from database
  - Interact with `Schema` or `Entity` (may be use join query -> can interact with others `schema`, `entity`)

- `Schema/Entity`

  - Prefix with `Schema` or `Entity`
  - Use for define object mapping with Collection or Table in Database layer

- `Utils`

```javascript
// Sample Project Structure
//==========================

SampleApp-Server/
		- .git
		- .gitignore
		- package.json
		- app.js  			// Cluster Master, our main file that runs server.js
		- server.js 		// Cluster Node file
		- config/
        - database/
          - database.config.js
				- env/					// Environment specific config files
					- development.env.js
					- production.env.js
					- test.env.js
					...
    - common
      - constant
        - index.js
    - middlewares/
				- authentication.js
				- authorization.js
        - error-handler.js
		- routes/
				- user.router.js
        - product.router.js
				- order.router.js
        - index.js
    - modules/
      - user/
        - controllers/
            - user.controller.js
            ...
        - services/
            - user.service.js
            ...
        - repositories/
            - user.repository.js
            ...
        - entities/
            - user.entity.js
            ...
      - product/
      - order/
      - shopping-cart/
        - controllers/
            - shopping-cart.controller.js
	 - utils/
				- common/
          - array.utils.js
          - string.utils.js
          - crypto.utils.js
					- datetime.utils.js
	- tests/
				...
```

#### b. The order of importing package, modules

- Built-in package of nodejs
- Third party package (install via package.json)
- Group of: constant, config
- Group of: utils, middleware
- Group of: .model, .entity
- Group of: .repository
- Group of .service
-

### 2. Catch and throw exception

#### a. Throw exception

- Throw exception with Error object (object built in of javascript)
- Can custom object (extends from Error) for customize purpose

#### b. Catch error in express pipeline

```javascript
// server.js
const bodyParser = require('body-parser');
const methodOverride = require('method-override');

const app = express();
app.use(
  bodyParser.urlencoded({
    extended: true,
  })
);
app.use(bodyParser.json());
app.use(methodOverride());
app.use(function (err, req, res, next) {
  // handle error logic
});

// Subscribe to "uncaughtException" event: Exception in synchronous
process.on('uncaughtException', (ex) => {
  console.log('Uncaught Exception');
  logger.error(ex.message, ex);
});
// Subscribe to "uncaughtException" event: Exception in asynchronous
process.on('unhandledRejection', (ex) => {
  console.log('Unhandle Rejection');
  logger.error(ex.message, ex);
});
```

### 3. Logging

[Ref](https://stackify.com/node-js-logging/)

- Why use log?
  - Debugging while coding
  - Error tracking when server is running
  - Analyzing logs—Logs are rich sources of information.
    You can analyze logs to discover usage patterns to guide troubleshooting.
- Use custom class for logging
  - simple is use console.log
  - Log package suggestion:
    - [`Winston`](https://www.npmjs.com/package/winston) is one of the most popular logging utilities.
      The cool thing is, it can take more than one output transport.

### 4. Design pattern

- Singleton:
  - nodejs:
    "awilix": "^3.0.9",
    "awilix-express": "^0.11.0"
  - NestJS (auto)
- MVC

### More. [SDC2 rules](https://docs.google.com/document/d/11-ZrUaBKZqi3oVbqRL7wutKNMBaBLffE/edit)

# C. Summary

### Config eslint

- [How to config eslint in your project?](https://viblo.asia/p/hay-su-dung-eslint-cho-du-an-cua-ban-bJzKm07O59N)

### Ref:

- https://eslint.org/
- http://anixir.com/minimal-node-express-style-guide/
- https://medium.com/swlh/node-js-coding-style-guidelines-74a20d00c40b
- https://www.perfomatix.com/nodejs-coding-standards-and-best-practices-node-js-development-company/
- https://viblo.asia/p/hay-su-dung-eslint-cho-du-an-cua-ban-bJzKm07O59N
- https://ehkoo.com/bai-viet/tong-hop-tinh-nang-noi-bat-es6
- https://expressjs.com/en/guide/error-handling.html
- https://stackify.com/node-js-logging/
