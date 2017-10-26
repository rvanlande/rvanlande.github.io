## PostgreSql

* connect to database

        psql -U ${user} ${db_name}

* dsiplay result in page mode

        \pset pager on

* change database

        \c <other_db>

* save database

        pg_dump -Fc ${db_name} > ${dump_name}.dmp

* restore database

        pg_restore -d ${db_name} ${dump_name}.dmp
        
* drop database

        dropdb -U rcv ${db_name}

* duplicate database

        $ su -
        $ su - postgres   // connect as postgre user
        $ psql            // conect to database
        ~=> CREATE DATABASE ${db_name2} WITH TEMPLATE ${db_name1} OWNER rcv;

* sessions connected to database

```sql
select * from pg_stat_activity;
```

* get information about database

```sql
select table_name, column_name
from information_schema.columns
where column_name like '%xxx%' order by table_name, column_name;
```        

* display enumeration type

```sql
select n.nspname as enum_schema, t.typname as enum_name, e.enumlabel as enum_value 
from pg_type t join pg_enum e
on t.oid = e.enumtypid join pg_catalog.pg_namespace n
on n.oid = t.typnamespace 
where t.typname = ${type};
```

### Postgresql / JSON : sheet cheat

Merci à [Hackernoon](https://hackernoon.com/how-to-query-jsonb-beginner-sheet-cheat-4da3aa5082a3).

* **Select items by the value of a first level attribute (#1 way)**

```sql
SELECT * FROM users WHERE metadata @> '{"country": "Peru"}';
```
 
* **Select items by the value of a first level attribute (#2 way)**

```sql
SELECT * FROM users WHERE metadata->>'country' = 'Peru';
```

* **Select item attribute value**

```sql
SELECT metadata->>'country' FROM users;
```

* **Select only items where a particular attribute is present**

```sql
SELECT * FROM users WHERE metadata->>'country' IS NOT NULL;
```

* **Select items by the value of a nested attribute**

```sql
SELECT * FROM users WHERE metadata->'company'->>'name' = "Mozilla";
SELECT * 
  FROM users 
  WHERE metadata @> '{"company":{"name": "Mozilla"}}';
```  
    
* **Select items by the value of an attribute in an array**

```sql
SELECT * FROM users WHERE metadata @> '{"companies": ["Mozilla"]}';
```

* **IN operator on attributes**

```sql
SELECT * FROM users 
  WHERE metadata->>'countries' IN ('Chad', 'Japan');
```  
  
* **Insert a whole object**

```sql
UPDATE users SET metadata = '{"country": "India"}';
```

* **Update or insert an attribute**

```sql
UPDATE users SET metadata = metadata || '{"country": "Egypt"}';
```

* **Removing an attribute**

```sql
UPDATE users SET metadata = metadata - 'country';
```


