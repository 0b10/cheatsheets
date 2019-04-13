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

