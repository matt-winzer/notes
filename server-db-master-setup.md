# Server & Database Master Setup (express-generator)

## Initial Setup

* Create a GitHub repo
* Clone the repo and cd into it

* Console:
```
express --view=hbs --git
yarn add pg knex dotenv
knex init
createdb *database-name*
```

* In .gitignore file:
```
node_modules
.env
```

In knexfile.js
```js
require('dotenv').config();

module.exports = {
  development: {
    client: 'pg',
    connection: 'postgres://localhost/*database-name*'
  },
  
  production: {
    client: 'pg',
    connection: process.env.DATABASE_URL + '?ssl=true'
  }
};
```

##



## Dependencies
