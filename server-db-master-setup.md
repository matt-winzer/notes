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

## Knex: Environment Configuration

* In knexfile.js
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

* Console:
```
mkdir db
cd db
touch knex.js
```

* In knex.js
```js
const environment = process.env.NODE_ENV || 'development';
const config = require('../knexfile.js')[environment]

module.exports = require('knex')(config)
```

## Knex: Migrations

* Console:
```
knex migrate:make 01_migration_name
```

* Migration file should look something like:
```js
exports.up = function(knex, Promise) {
  return knex.schema.createTable('member', (table) =>{
  table.increments();
  table.text('username').notNullable().unique();
  table.text('email').unique().notNullable();
  table.text('password').notNullable();
  table.date('dateCreated').notNullable();
  table.boolean('isActive').notNullable().defaultTo(true);
  table.text('bio');
  })
};

// express knex 4 lyfe

exports.down = function(knex, Promise) {
  return knex.schema.dropTableIfExists('member');
};
```

* Foreign Key relationships are handled as such:
```js
table.integer('memberID').references('member.id').unsigned().onDelete('cascade')
```
* Run migrations
```
knex migrate:latest
```

* Continue making migrations for all the tables in your database


## Knex: Seeds

* Console:
```
knex seed:make 01_seed_name
```

* Seed file should look something like:
```js
var bcrypt = require('bcrypt');

exports.seed = function(knex, Promise) {
  // Deletes ALL existing entries
  return knex.raw('DELETE FROM "user"; ALTER SEQUENCE user_id_seq RESTART WITH 3;')
    .then(function () {
      var users = [{
        id: 1,
        name: 'sam',
        email: 'sam@gmail.com',
        password: bcrypt.hashSync('sammyg21', 10)
      }, {
        id: 2,
        name: 'alex',
        email: 'alex@gmail.com',
        password: bcrypt.hashSync('alexmart05', 10)
      }];
      return knex('user').insert(users);
    });
};
```

* Run seed
```
knex seed:run
```

* Keep creating seeds and running them until your entire database is seeded


























## Dependencies
