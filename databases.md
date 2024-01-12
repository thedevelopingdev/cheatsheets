# Databases

## PostgreSQL

Connect

```bash
# connect using psql
psql -h localhost -d mydatabase -U myuser [-p port] --set sslmode=disable
```

### General info

```sql
# list all databases
\list

# switch database
\connect <dbname>

# list all tables
\dt
```

### Permissions

```sql
# create user
# note the single quotes
CREATE USER new_user WITH PASSWORD 'strong_password';

# view roles
\du

# view database access privileges:
# https://www.postgresql.org/docs/12/ddl-priv.html#PRIVILEGE-ABBREVS-TABLE
\l

# view all grants on public schema
# use this!
WITH users AS (
  SELECT rolname, oid
  FROM pg_roles
  UNION
  SELECT 'PUBLIC', 0
)
SELECT r.rolname AS grantor, e.rolname AS grantee,
       nspname AS schema, privilege_type, is_grantable
FROM pg_namespace, aclexplode(nspacl) AS a
JOIN users AS e ON a.grantee = e.oid
JOIN users AS r ON a.grantor = r.oid
WHERE nspname = 'public';

# view permissions
SELECT table_catalog, table_schema, table_name, privilege_type FROM information_schema.table_privileges WHERE grantee = 'example_user';

# set example_user to read only on example_database
# source: https://docs.digitalocean.com/products/databases/postgresql/how-to/modify-user-privileges/
REVOKE ALL ON DATABASE example_database FROM example_user;
GRANT CONNECT ON DATABASE example_database TO example_user;
GRANT SELECT ON ALL TABLES IN SCHEMA public TO example_user;
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT SELECT ON TABLES TO example_user;

# set example_user to read/write on example_database
REVOKE ALL ON DATABASE example_database FROM example_user;
GRANT CONNECT ON DATABASE example_database TO example_user;
GRANT INSERT ON ALL TABLES IN SCHEMA public TO example_user;
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT INSERT ON TABLES TO example_user;
GRANT CREATE ON DATABASE example_database TO example_user;
```


