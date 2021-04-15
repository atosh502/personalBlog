---
title: "PostgreSQL: password authentication failed for user"
date: "2021-04-15T22:40:32.169Z"
template: "post"
draft: false
slug: "postgresql-password-authentication-failed-for-user"
category: "postgresql"
tags:
  - "postgresql"
  - "password-authentication-failed"
description: "Resolve password authentication failed error for user in Postgresql"
socialImage: "/media/image-0.jpg"
---

By default we can log into the `psql` command line interface using the user `postgres`. The user `postgres` has full access to the entire PostgreSQL instance and can be logged into using the user's login password in ubuntu. Next we can create the user and the database from the `psql` command line. 

```
sudo -u postgres psql
postgres=# create database dbName;
postgres=# create user userName with password 'userPassword';
postgres=# grant all privileges on database dbName to userName; 
```

When we try logging in using the created user and password we may get the error: password authentication failed for user.

In my case, the error was due to the method used to authenticate the newly created user. The method of authentication specified in file  `/etc/postgresql/<version>/main/pg_hba.conf` was `md5`. I changed `md5` to `trust` and was able to use the user password to login to the database using pgAdmin. 

If we were to use the `md5` method to login the user, we would need to hash the password in a certain way and use that string as a password for login. That is, if the user is `userName` and password is `userPassword` then the correct password string would be the md5 hash of `userPassworduserName` with the string `md5` added to the beginning of that string. So, the final password string would look something like this `md55fad6e9567aea4a2475b167de8e8cb07`.

Resources: 
* https://stackoverflow.com/a/17431573/7358595
* https://stackoverflow.com/a/7696398/7358595
* https://medium.com/coding-blocks/creating-user-database-and-adding-access-on-postgresql-8bfcd2f4a91e
* https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-20-04
