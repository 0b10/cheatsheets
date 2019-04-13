# Config
## File Path
config file: `/var/lib/pgsql/data/pg_hba.conf`
## Options

The 'method' specified here requires you to use a password, and is hashed with md5. You can choose other, more secure methods.

Libraries like 'knex' will connect over TCP, even on the localhost. Commands like psql will connect via sockets on localhost.
#### Unix Sockets
TYPE | DATABASE | USER | ADDRESS | METHOD
--- | --- | --- | --- | ---
local | all | all | | md5

#### Local TCP Connections
TYPE | DATABASE | USER | ADDRESS | METHOD
--- | --- | --- | --- | ---
host | all | all | 127.0.0.1/32 | md5

# Database management
## Account
Environment variables for credentials (use for testing). Can be prepended to commands.
> PGUSER=foo
> PGPASSWORD=bar

## Roles
```sql
CREATE ROLE "role_name" WITH LOGIN PASSWORD 'password'; --Can log in.
CREATE ROLE "role_name" WITH CREAREDB LOGIN PASSWORD 'password'; --Multiple role attributes
```
```sql
DROP ROLE "role_name";
```
## Privileges
##### Inheritance
* LOGIN, SUPERUSER, CREATEDB, and CREATEROLE cannot be inherited. They must be granted directly.
##### Statements
This grants permissions for all database operations - like connect. There are probably more refined permissions to explore.
```sql
GRANT ALL PRIVILEGES ON DATABASE db_name TO "role_name";
```
This grants all permissions for a table. Again, there's probably more refined permissions to explore.
```sql
GRANT ALL PRIVILEGES ON TABLE table_name TO "role_name";
```
This revokes all permissions for DATABASE (can be on any object, e.g. TABLE).
```sql
REVOKE ALL PRIVILEGES ON DATABASE db_name FROM "role_name";
```
---

# CLI
Command | Result
--- | ---
\d [table_name] | Describe table; view table schema.
\dt | Describe tables; view all tables.
\du | Describe roles; view all roles.
\x | Toggle extended output.

