# Security

<img src="star.png" width="500px">

## Injections

Injections means injecting code in another piece of code.

### SQL Injection

<img src="sql_injection.png" width="500px">

#### Normal statement

`INSERT INTO sqlinjection (email) VALUES ('user@example.com')`

#### Injection

`INSERT INTO sqlinjection (email) VALUES (; DROP TABLE users; --)` 

- This statement drop the `users` table
- `--` means comment everything after it

`' or 1=1--`

- Can be used on password fields
- Leave an empty string and verify that 1 equals 1 which is obviously true and can return the full user's information.

Anytime a user has to **input** something, we're vulnerable to injections !

### JS Injection

We can not inject inline scripts because they are executed when the original page is parsed.

What we can do instead is the following : `<img src="/" onerror="alert('boom')">`

An alert will be triggered on submit. This is possible with `innerHTML` ! A way to fix this is to use `createTextNode`.

### What to remember

- `Sanitize input` : validating and cleaning input. We're making sure our inputs have the type that we want (a phone number has to have 10 digits, etc)
- `Parametrize queries` : also called prepared statements. Prevent SQL Injections. It precompiles SQL statements so that we can only provide the parameters to the statement.
- `Knex.js or other Object Relational Mappers` : these are libraries that parametrize queries. Knex is quite popular.

[Exercice](https://www.hacksplaining.com/exercises/sql-injection#/start)

## 3rd party libraries

Always be careful with 3rd party libraries because anybody can load up his package on NPM. We have to use packages are well-known, well-respected and constantly updated.

### `nsp` and `snyk`

#### Installation

* `npm install -g nsp`
* `npm install -g snyk`

#### Usage

`nsp` checks the `package.json` file. We can do it by running `nsp check`

`snyk` : audits the `node_modules` directory. We can do it by running `snyk test`

### `npm audit`

Since NPM 6, we can use `npm audit`.

It allows us to recursively analyze your dependency trees to identify specifically what’s insecure — so we can swap in a new version or find a safer alternate dependency.

