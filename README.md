# Usage

## Clone this docker-compose.yml file
## Start 
```
feng@ubuntu:~/mariadb$ docker-compose up -d
Creating network "mariadb_host" with the default driver
Creating mariadb_mariadb_1 ... done
feng@ubuntu:~/mariadb$ docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
88f8196dbc2d        mariadb:10.2        "docker-entrypoint.s…"   28 seconds ago      Up 22 seconds       0.0.0.0:3306->3306/tcp   mariadb_mariadb_1
feng@ubuntu:~/mariadb$ 
```

## Test from inside container
```
feng@ubuntu:~/mariadb$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
88f8196dbc2d        mariadb:10.2        "docker-entrypoint.s…"   2 minutes ago       Up 2 minutes        0.0.0.0:3306->3306/tcp   mariadb_mariadb_1
feng@ubuntu:~/mariadb$ docker exec -it 88f bash
root@88f8196dbc2d:/# mysql -u root -p
Enter password: 
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 8
Server version: 10.2.25-MariaDB-1:10.2.25+maria~bionic mariadb.org binary distribution

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mybb               |
| mysql              |
| performance_schema |
+--------------------+
4 rows in set (0.00 sec)

MariaDB [(none)]> exit
Bye
root@88f8196dbc2d:/# exit
exit
feng@ubuntu:~/mariadb$ 
```

## Test from docker host
```
feng@ubuntu:~/mariadb$ docker inspect -f "{{ .NetworkSettings.Networks.mariadb_host.IPAddress }}" 88f
172.20.0.2
feng@ubuntu:~/mariadb$ 

feng@ubuntu:~/mariadb$ mysql -h 172.20.0.2 -P 3306 -u root -p mybb
Enter password: 
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 12
Server version: 10.2.25-MariaDB-1:10.2.25+maria~bionic mariadb.org binary distribution

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [mybb]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mybb               |
| mysql              |
| performance_schema |
+--------------------+
4 rows in set (0.001 sec)

MariaDB [mybb]> quit
Bye
feng@ubuntu:~/mariadb$
```

## git housekeeping

### create git repo from command line
```
curl -u 'fen9li' https://api.github.com/user/repos -d '{"name":"docker-mariadb"}'

git init
git remote add origin git@github.com:fen9li/docker-mariadb.git

feng@ubuntu:~/mariadb$ git remote -v
origin  git@github.com:fen9li/docker-mariadb.git (fetch)
origin  git@github.com:fen9li/docker-mariadb.git (push)
feng@ubuntu:~/mariadb$ 

echo mariadb >> .gitignore

git add .
git commit -am "Initial commit"
git push --set-upstream origin master

```
