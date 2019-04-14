# Approaches
## Fresh Migration
Typically you want to rollback the migration as a *batch*, migrate to latest, then seed the database. Doing so will result in all auto incrementing values in the database starting from 0 or 1.

```sh
knex migrate:rollback
knex migrate:latest
knex seed:run
```

# CLI
Command | Result
--- | ---
knex migrate:make [migration_name] | Create migration scripts under ./migrations.
knex migrate:rollback | Rollback a single migration batch. Batches can be multiple relations, or singular. Each 'up' script is a single relation. A batch is a group of multiple 'up' scripts in a single job.
knex seed:make [num]_[name] | Seed (populate) the database with values. Seeds are run in alphanumerical order, so number them.

# Migrations
##### Command

`knex migrate:make [migration_name]`

##### Example
These scripts are generated in ./migrations.

```javascript
exports.up = function(knex, Promise) {
    return knex.schema.createTable('user', table => {
        table.increments();
        table.text('username');
        table.text('email');
        table.text('password');
    });
};

exports.down = function(knex, Promise) {
    return knex.schema.dropTable('user');
};
```


# Schema
## Building
### Column Defaults

A default value for a column:

```javascript
table.integer('column_name').defaultsTo('value');
```

### Events

[onDelete()](https://knexjs.org/#Schema-onDelete) and [onUpdate()](https://knexjs.org/#Schema-onUpdate) tigger when those events take place - e.g. setting onDelete() to an id column will trigger when a row with that column is deleted (i.e. any row). The argument is an SQL command.

```javascript
table.integer('id').onDelete('CASCADE'); // When an 'id' (row) is deleted, CASCADE changes.
```

### Foreign Key

```javascript
table.integer('column')
  .references('foreign_column')
  .inTable('foreign_table')
  .index(); // SQL indexing. Use for foreign keys.
```

### NOT NULL

When a column must never be null:

```javascript
table.integer('column_name').notNullable();
```

### Timestamps
Create timestamp fields: '*created\_at*' and '*updated\_at*' - which define when the records were *created* and *updated*. [See docs](https://knexjs.org/#Schema-timestamps).

"**A**" (bool): When **false** - use datetime format; when **true** - use timestamp format.

"**B**" (bool): When **true** - both columns are NOT NULL, and use the current time.

```javascript
table.timestamps(A, B)
```
There is also *timestamp()* for single timestamp fields: `table.timestamp(...)`. [See docs](https://knexjs.org/#Schema-timestamp).