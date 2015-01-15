# Get start

Collect get starts.

### Contents

- [Window manager](#window-manager)
- [Database](#database)
    - [mongodb](#mongodb)
    - [redis](#redis)
    - [mariadb](#mariadb)
    - [postgresql](#postgresql)
- [SSL certificates](#ssl-certificates)

## Window manager

### awesome

- install
      
    ```
    # yum install awesome -y
    ```

- configure

    ```
    # cp /etc/xdg/awesome/ ~/.config/awesome
    ```

- theme

    ```
    # cp -R /usr/share/awesome/themes ~/.config/awesome
    ```

    edit rc.lua with theme line as below:

    ```
    beautiful.init(awful.util.getdir("config") .."/themes/default/theme.lua")
    ```

## Database

### mongodb

- install

    ```
    # yum install mongodb-server -y
    ```

- daemon

    ```
    # systemctl start mongod
    ```

- client

    ```
    # mongo
    ```

- basic usage

    ```
    # mongo
    > show dbs // show all dbs on host
    > use testdb
    > db
    > show collections
    > var i = {"name": "test", "version": 1}
    > db.testdb.insert(i)
    > db.testdb.findOne()
    > db.testdb.find()
    > db.testdb.remove({url: "https://github.com"})
    > for (j=0; j<10; j++) db.testdb.insert(i)
    > db.testdb.find().limit(3)
    ```

- remote access
      
    on mongod server(192.168.1.2)
    edit /etc/mongodb.conf

    ```
    bind_ip = 0.0.0.0
    ```

    on remote server

    ```
    # mongo 192.168.1.2:27017/mydb
    ```

### redis

- install

    ```
    # yum install redis -y
    ```

- daemon

    ```
    # systemctl start redis
    ```

- client
     
    ```
    # redis-cli
    ```

- basic usage

    Set password, uncomment below line in /etc/redis.conf and change password to yours

    ```
    # requirepass foobared
    ```

    Get all keys

    ```
    127.0.0.1:6379> keys *
    ```

- [ALL redis cli commands](http://redis.io/commands)

### mariadb

- install

    ```
    # yum install mariadb-server -y
    ```

- daemon

    ```
    # systemctl start mariadb
    ```

- setup

    ```
    # mysql_secure_installation
    ```

- client

    ```
    # mysql
    ```

- basic usage

    ```
    > show databases;
    > use test;
    > show tables;
    > create table testtable (name varchar(10), age int(4));
    > insert into testtable values ('test', 2014);
    ```

- remote access

    ```
    > grant all privileges on test.* to admin@'%' identified by
      'password' with grant option;
    ```

    Note: change '%' to remote hostname will be much better

### postgresql

- install

    ```
    # yum install postgresql-server -y
    # postgresql-setup initdb
    ```

- daemon

    ```
    # systemctl start postgresql
    ```

- client

    ```
    # psql
    ```

- basic usage

    ```
    $ su - postgres
    $ createdb mydb or
      > create database mydb owner postgres
    $ createuser lenny or 
      > create user lenny with password 'securepasswd'
    ```

- remote access

    
    edit /var/lib/pgsql/data/postgresql.conf
      change listen_address to "*"

    vi pg_hba.conf
      add line such as
      host mydb lenny 192.168.1.2 255.255.255.0 trust

## SSL certificates

- Step one - Install Mod SSL

    ```
    # yum install mod_ssl -y
    ```

- Step two - Create a self-signed certificate

    ```
    # mkdir /etc/httpd/ssl
    # cd /etc/httpd/ssl
    # openssl genrsa -out ca.key 2048
    # openssl req -new -key ca.key -out ca.csr
    # openssl x509 -req -days 365 -in ca.csr -signkey ca.key -out ca.crt
    ```

    or

    ```
    # openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/httpd/ssl/ca.key -out /etc/httpd/ssl/ca.crt
    ```

- Step three - Setup the certificate

    ```
    # vi /etc/httpd/conf.d/ssl.conf
      SSLEngine on
      SSLCertificateFile /etc/httpd/ssl/ca.crt
      SSLCertificateKeyFile /etc/httpd/ssl/ca.key 
    ```

    ```
    # systemctl restart httpd
    ```
