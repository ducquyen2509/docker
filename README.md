1. Pull mysql-server image from docker hub. I will use mysql-server version 5.6
```console
docker pull mysql/mysql-server:5.6
```

2. Let's run container in deamon mode and export port 3306 to external network for client can connect to it. At this step, we start a new container so MySql server will create `root` user with onetime generated password.
```console
docker run -d -p 3306:3306 --name mysql-server  mysql/mysql-server:5.6
```

3. Ok. Let's get generated password for `root` user at step 2
```console
docker logs mysql-server
>
> 
> Warning: Unable to load '/usr/share/zoneinfo/zone.tab' as time zone. Skipping it.
> Warning: Unable to load '/usr/share/zoneinfo/zone1970.tab' as time zone. Skipping it.
> [Entrypoint] GENERATED ROOT PASSWORD: =@RoG#Em)3vp4q=eRAqKyj4byf=


```

4. Open bash shell of mysql-server container with `root` user. We will use password got from step 3.
```console
docker exec -it mysql-server mysql -u root -p
```

5. Because we are using onetime generated password so MySql server will require you to reset password before excuting any statements. I will change password of `root` user to `123456789`
```MYSQL
ALTER USER 'root'@'localhost' IDENTIFIED BY '123456789';
```

6. We should create new user to connect to database. I will create a new user named `admin` will full permissions and set password to `123456789`.
```mysql
CREATE USER 'admin'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456789';

GRANT ALL PRIVILEGES ON *.* TO 'admin'@'localhost' WITH GRANT OPTION;

CREATE USER 'admin'@'%' IDENTIFIED WITH mysql_native_password BY '123456789';

GRANT ALL PRIVILEGES ON *.* TO 'admin'@'%' WITH GRANT OPTION;

FLUSH PRIVILEGES;
```

https://stackoverflow.com/questions/1559955/host-xxx-xx-xxx-xxx-is-not-allowed-to-connect-to-this-mysql-server
