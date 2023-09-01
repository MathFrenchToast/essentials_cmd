# MySQL Essentials

This short recipes applies to mysql database management 5.7 and 8.0

# show all users
e.g. to adjust grants : `SELECT user,host FROM mysql.user;`

show grant of a user (using the data read above) : `SHOW GRANTS FOR '{username}'@'{ip}';`
 
# create user and grant
```
CREATE USER '{username}'@'{ip}' IDENTIFIED BY '{password}}';
GRANT SELECT, INSERT, UPDATE, DELETE ON {dbname}.* TO '{username}'@'{ip}';
flush privileges;
```

other usefull privileges are :
* CREATE, DROP to enable create and drop table_name
* REFERENCES to enable FK creation
* LOCK TABLES
* ALTER

Revoke privileges
```
REVOKE ALL PRIVILEGES, GRANT OPTION FROM 
GRANT SELECT, INSERT, UPDATE, DELETE ON *.* TO '{username}'@'{ip}';
```

delete a user:  
`DROP USER '{username}'@'{ip}';`

change user password:  
`ALTER USER '{username}'@'{ip}' IDENTIFIED BY '{auth_string}';`

exemple : `ALTER USER 'jeffrey'@'localhost'  IDENTIFIED BY 'new_password'`

create a second admin (beside the root user create when installing), only from local network :
`GRANT ALL PRIVILEGES ON *.* TO 'admin'@'192.168.0.0/255.255.255.0' IDENTIFIED BY 'adminpwd';`

# get Mysql Version
SHOW VARIABLES LIKE "%version%";

# create database with charset

create database {databasename} CHARACTER SET utf8 COLLATE utf8_unicode_ci;

## enable a query log
being root 

set GLOBAL general_log_file='/home/{username}/tmp/mysqlbinlog/{filename}.log';
SET GLOBAL log_output='FILE';
SET GLOBAL general_log = 'ON';

## list tables
SELECT table_name FROM information_schema.tables
WHERE table_schema = 'your_database_name';

to add a condition on check column on  `describe information_schema.tables`


# using the mysql client configurator
Create a new config with
`mysql_config_editor set --login-path={configname}  --host=localhost --user=$USERNAME --password`
enter the passwd and save
then to connect with  
`mysql --login-path={configname}`
to list all configs:  
`mysql_config_editor print --all`
   
# size
## size of the database
SELECT table_schema "DB Name",
        ROUND(SUM(data_length + index_length) / 1024 / 1024, 1) "DB Size in MB" 
FROM information_schema.tables 
GROUP BY table_schema; 

## size tables
```
set @schema='mychema';

SELECT
  TABLE_NAME AS `Table`,
  ROUND((DATA_LENGTH + INDEX_LENGTH) / 1024 / 1024) AS `Size (MB)`
FROM
  information_schema.TABLES
WHERE
  TABLE_SCHEMA = @schema
ORDER BY
  (DATA_LENGTH + INDEX_LENGTH)
DESC;
```

# processes and locks

## list processes

`show [full] processlist;`   
or for more control on result:  
`SELECT * FROM INFORMATION_SCHEMA.PROCESSLIST where info is not null;`  

## lock tables

SHOW OPEN TABLES IN {db_name} WHERE in_use >= 1 ;

# reset root password
https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html

# dump 
dump of full database
`mysqldump --login-path={clientconfigname} --set-gtid-purged=OFF --disable-keys --single-transaction {databasename} [tablename] >  {file.sql} `
--set-gtid-purged=OFF : if there is a replication, do not included the gtid
--disable-keys --single-transaction : to avoid locking the tables

dump with where close
same as above using --where= option

using MysqlDump as of v.8 add  --column-statistics=0 flag


# work with transaction
steps:  
- first disable autocommit,
- then start tx, 
- run your sql stmt
- close the tx with commit (ok) or rollback (ko)
```
SET autocommit = 0;
START TRANSACTION;
-- here your sql stmt
COMMIT;
```


