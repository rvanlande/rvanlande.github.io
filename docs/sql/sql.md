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

        select * from pg_stat_activity;

* get information about database

        select table_name, column_name
        from information_schema.columns
        where column_name like '%xxx%' order by table_name, column_name;

* display enumeration type

        select n.nspname as enum_schema, t.typname as enum_name, e.enumlabel as enum_value 
        from pg_type t join pg_enum e
        on t.oid = e.enumtypid join pg_catalog.pg_namespace n
        on n.oid = t.typnamespace 
        where t.typname = ${type};