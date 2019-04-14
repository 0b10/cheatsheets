# CLI
Command | Result
--- | ---
knex migrate:make [migration_name] | Create migration scripts under ./migrations.
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
### Timestamps
Create timestamp fields: '*created\_at*' and '*updated\_at*' - which define when the records were *created* and *updated*. [See docs](https://knexjs.org/#Schema-timestamps).

"**A**" (bool): When **false** - use datetime format; when **true** - use timestamp format.

"**B**" (bool): When **true** - both columns are NOT NULL, and use the current time.

```javascript
table.timestamps(A, B)
```
There is also *timestamp()* for single timestamp fields: `table.timestamp(...)`. [See docs](https://knexjs.org/#Schema-timestamp).