# Objective
We want clean code and the possibility of adding new features with ease.

# Rule 1
Organize your Files Around Features, Not Roles
DONT
```
.
├── controllers
|   ├── product.js
|   └── user.js
├── models
|   ├── product.js
|   └── user.js
├── views
|   ├── product.hbs
|   └── user.hbs
```
DO
```
.
├── product
|   ├── index.js
|   ├── product.js
|   └── product.hbs
├── user
|   ├── index.js
|   ├── user.js
|   └── user.hbs
```

# Rule 2
Don't Put Logic in index.js Files. Use these files only to export functionality.

# Rule 3
Place Your Test Files Next to The Implementation. Tests are not just for checking whether a module produces the expected output, they also document your modules. Put your additional test files to a separate test folder to avoid confusion.
```
.
├── test
|   └── setup.spec.js
├── product
|   ├── index.js
|   ├── product.js
|   ├── product.spec.js
|   └── product.hbs
```

# Rule 4
Use a config directory for configuration files.

# Rule 5
Create separate directory for your additional long scripts in package.json
```
.
├── scripts
|   ├── syncDb.sh
|   └── provision.sh
```