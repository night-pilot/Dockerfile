## Plugins

 1.logstash-filter-aggregate
 2.logstash-filter-mutate
 3.logstash-input-mongodb


## Customize configuration

Customize configuration for logstash

- modify file `logstash.conf` in docker container

```yml
input {
    jdbc {
        jdbc_driver_library => "${LOGSTASH_JDBC_DRIVER_JAR_LOCATION}"
        jdbc_driver_class => "${LOGSTASH_JDBC_DRIVER}"
        jdbc_connection_string => "${LOGSTASH_JDBC_URL}"
        jdbc_user => "${LOGSTASH_JDBC_USERNAME}"
        jdbc_password => "${LOGSTASH_JDBC_PASSWORD}"
        schedule => "* * * * *"
        statement => "select ... from table_name"
    }
}

output {
    stdout { codec => json_lines }
}
```

*Environment example*

- LOGSTASH_JDBC_DRIVER_JAR_LOCATION: 
    - for MySQL 5.7+ Database: `/usr/share/logstash/logstash-core/lib/jars/mysql-connector-java.jar`
    - for MS-SQL Server 2017 linux+ Database: `/usr/share/logstash/logstash-core/lib/jars/mssql-jdbc.jar`
    - for Oracle 11g XE Database: `/usr/share/logstash/logstash-core/lib/jars/ojdbc6.jar`
    - for PostgreSQL 9.6+ Database: `/usr/share/logstash-core/lib/jars/logstash/postgresql.jar`
- LOGSTASH_JDBC_DRIVER: `com.mysql.jdbc.Driver`
- LOGSTASH_JDBC_URL: `jdbc:mysql://[host]:[port]/[database-name]`
- LOGSTASH_JDBC_USERNAME: `database_user`
- LOGSTASH_JDBC_PASSWORD: `database_user_password`

 
*Example config for MongoDB*
```
input {
        uri => 'mongodb://usernam:password@anurag-00-00-no6gn.mongodb.net:27017/anurag?ssl=true'
        placeholder_db_dir => '/opt/logstash-mongodb/'
        placeholder_db_name => 'logstash_sqlite.db'
        collection => 'users'
        batch_size => 5000
}
filter {
}
output {
        stdout {
                codec => rubydebug
        }
        elasticsearch {
                action => "index"
                index => "mongo_log_data"
                hosts => ["localhost:9200"]
        }
}
```

