# drobboo-project-guide


## Docker Configuration
- After installing Docker, run the following commands to create and start two containers of MongoDB and PostgreSQL databases.
```
$ docker run -it --name postgres -d -e POSTGRES_PASSWORD="password" -p 5432:5432 -v /var/lib/docker-db/postgresql/data:/var/lib/postgresql/data postgres
```
```
$ docker run -it --name mongo -d -e MONGO_INITDB_ROOT_USERNAME=mongo -e MONGO_INITDB_ROOT_PASSWORD="password" -p 27017:27017 -v /var/lib/docker-db/mongodb/data:/data/db mongo
```
Make sure you have to run this command only once in life time. 
After that you have to run the following command to start the container of databases:
```
$ docker start <container_name>
```
OR
```
$ docker start <container_id>
```
Example:
```
$ docker start mongo
$ docker start postgres
 ```

- To see the full list of created containers(both running and stopped) 
```
$ docker ps -a
```

- To see the full list of created containers(only running ) 
```
$ docker ps
```
- To remove container,run the following:
```
$ docker rm <id> 
```
or 
```
$ docker rm <container_name> 
```
- To see the running information of running processes and PID:
```
$ netstat -tunlp
```
- To kill any runnning process
```
$ kill -9 <PID>
```
- To create the core database in `Drobboo Server` project:
```
$ yarn db:create
```

- Before entering the container , run the following command to copy the `pgsql` file of PostgreSQL from HOST OS into the container OS:
```
$ docker cp <path to pgsql file> <container_id>:<path to backup folder inside container where the pgsql file will be saved>
```

For Example: 
```
$ docker cp /home/sajid576/Public/drobboo/drobboo_dbfile.pgsql  fa14cac10c04:/var/lib/postgresql/data/backup
```

- To enter into postgres container as `postgres` user,run the following command:
```
$ docker exec -it -u postgres postgres bash
```
- To enter into postgres container as `Root` user,run the following command:
```
$ docker exec -it  postgres bash
```

- After entering into container as `postgres` user, run the following command import the data of `pgsql` file to a database named `drobboo_dev` :
```
$ psql -U <username> <database_name>     <     <path to pgsql file inside  container>
```
`drobboo_dev` database is defined in `env` file. 
 For Example:
 ```
 $ psql -U postgres drobboo_dev <    /var/lib/postgresql/data/backup/drobboo_dbfile.pgsql
 ```
 `drobboo_dev`  database should now contain the data that is in the .pgsql file.
- Then go to `Drobboo-server` repository, and run the following to migrate the database to project:
```
$ yarn db:migrate:all
```




## NGINX Configuration

- After installing nginx go to below directory:
```
 etc/nginx/sites-enabled/default
```
- Copy paste these codes: 

```
upstream back-end {

        server localhost:7000;

}


upstream customer-home {

        server localhost:7001;

}

upstream storage {
	server localhost:7011;
}

```
Copy paste below code in the server block of  `default` file: 

```
location /core/ {

                proxy_pass http://back-end/;

}


location /customer-home/ {

                proxy_pass http://customer-home/;

}

location /storage/ {
		proxy_pass http://storage/;
}
```



