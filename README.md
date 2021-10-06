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







docker exec -it -u postgres bash
 
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



