# eea.docker.bwd

run the container with docker-compose:

create the data container bwd_home and bwd_data (see below)

```
# git clone <reponame>
# docker-compose build 
# docker-compose up -d
```

run the container:

```
# docker run --restart=always --name bwd --volumes-from=bwd_data --volumes-from=bwd_home  -p <port_host>:80 -e MYSQL_ROOT_PASSWORD=<secret_password> -d eeacms/bwd
```

current <port_host> = 50006

moving data volume containers from one host to another:

- donor host

```
# docker run --rm --volumes-from=bwd_home -v $(pwd):/backup busybox tar cvfp /backup/bwd_home.tar /var/www
# docker run --rm --volumes-from=bwd_data -v $(pwd):/backup busybox tar cvfp /backup/bwd_data.tar /var/lib/mysql
```

- target host

```
# docker run -d --name bwd_home eeacms/php_data 
# docker run -d --name bwd_data eeacms/mysql_data
# docker run --rm --volumes-from=bwd_home -v $(pwd):/backups busybox tar xvf /backups/bwd_home.tar 
# docker run --rm --volumes-from=bwd_data -v $(pwd):/backups busybox tar xvf /backups/bwd_data.tar
```

Ps: in case of resubmission of the database, be sure to have the right credentials in the database:

```
# update mysql.user set host = '%' where host = 'localhost' and user != 'root';
   # GRANT ALL ON bwd.* to '!!!changeme!!!'@'%' identified by '!!!changeme!!!';
   # flush privileges;
```
