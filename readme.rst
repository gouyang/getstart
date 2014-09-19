#########
Get start
#########
Most of the examples are based on Fedora.

Window manager
==============

- awesome

  * install::
      
      yum install awesome

  * configure::

      cp /etc/xdg/awesome/ ~/.config/awesome

  * theme::

      cp -R /usr/share/awesome/themes ~/.config/awesome
      vi rc.lua with theme line as below:
      beautiful.init(awful.util.getdir("config") .."/themes/default/theme.lua")

Programming Language
====================

- Python

  * yum install python python-devel

- Javascript

- Go


Database
========

- **mongodb**

  * install::

      yum install mongodb-server

  * daemon::

      systemctl start mongod

  * client::

      mongo

  * basic usage::

      > show dbs

      > db mydb

      > use mydb

      > db

      > var i = {"name": "test", "version": 1}

      > db.testdb.insert(i)

      > db.testdb.findOne()

      > db.testdb.find()

      > for (j=0; j<10; j++) db.testdb.insert(i)

      > db.testdb.find().limit(3)

  * remote access::
      
      on mongod server

      vi  /etc/mongodb.conf
      bind_ip = 0.0.0.0

      on remote server connect by

      mongo 192.168.1.2:27017/mydb

- **mariadb**

  * install::

      yum install mariadb-server

  * daemon::

      systemctl start mariadb

  * setup::

      mysql_secure_installation

  * client::

      mysql

  * basic usage::

      > show databases;

      > use test;

      > show tables;

      > create table testtable (name varchar(10), age int(4));

      > insert into testtable values ('test', 2014);

  * remote access::

      > grant all privileges on test.* to admin@'%' identified by
      'password' with grant option;

      #change '%' to remote hostname will be much better

- **postgresql**

  * install::

      yum install postgresql-server
      postgresql-setup initdb

  * daemon::

      systemctl start postgresql

  * client::

      psql

  * basic usage::

      $ su - postgres

      $ createdb mydb or

      > create database mydb owner postgres

      $ createuser lenny or 

      > create user lenny with password 'securepasswd'

  * Remote access::

      vi /var/lib/pgsql/data/postgresql.conf

      change listen_address to "*"

      vi pg_hba.conf

      add line such as

      host mydb lenny 192.168.1.2 255.255.255.0 trust

