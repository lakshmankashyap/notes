# Code Analysis

## Installing Postgresql

[Tutorial](https://www.codementor.io/engineerapart/getting-started-with-postgresql-on-mac-osx-are8jcopb)

Then I can launch `psql` with this command : `brew services start postgresql`, create the database : `createdb smart-brain` and finally the user : `createuser thomas`

Access the database with the `psql` command : `psql postgres://@localhost:5432/smart-brain`

We can then use commands like `\d` or `SELECT * FROM users

## Tips and tricks on how to analyze code

- Look at API endpoints : it tells a lot about the code.
- When you clone a web project, before `npm install` or `yarn install`, it's good to take a look at the `package.json` to know what you're talking about at a glance.
- Don't criticize code when we land on a project! We have to look at the code and be completely neutral, integrate onto the project and work as a team to make it better.