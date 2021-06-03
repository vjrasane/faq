# Databases

## Postgresql

### Simple join

```sql
  SELECT table2.* FROM table1, table2

  WHERE table1.uuid = table2.relation_id

  AND table1.uuid = $1;
```

### Composite constraint

```sql
CONSTRAINT constraint_name CHECK
(
(field1 = 'value' AND field2 IS NOT NULL AND field3 IS NULL)
OR (field1 IS NULL AND field2 IS NOT NULL)
)
```

### Update timestamp trigger

```sql
CREATE OR REPLACE FUNCTION trigger_set_timestamp()
RETURNS TRIGGER AS $$
BEGIN
NEW.updatetime = NOW();
RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER set_timestamp
BEFORE UPDATE ON tablename
FOR EACH ROW
EXECUTE PROCEDURE trigger_set_timestamp();
```

### Client usage

#### Connect:

```shell
psql postgresql://<user>:<pass>@<hostname>:<port>/<db>
```

#### Execute script:

```shell
psql postgresql://<user>:<pass>@<hostname>:<port>/<db> -f /path/to/script
```

#### Show tables:
```shell
\dt
```
#### Change db:
```shell
\c <db>
```

## MongoDB

### Read env variable in init script

```shell
#!/bin/bash
set -e
mongo <<EOF
use admin
print('Start #################################################################');

db = db.getSiblingDB('$DB_NAME');
db.createUser(
{
user: '$DB_USER',
pwd: '$DB_PASSWORD',
roles: [{ role: 'readWrite', db: '$DB_NAME' }]
}
);

db.createCollection('user-settings');
print('END #################################################################');
EOF

```