# drobboo-project-guide

docker ps

docker ps -a

docker rm <id> // to remove container

netstat -tunlp

kill -9 24596

yarn db:delete

yarn db:create

yarn db:sync

docker start mongo
 
docker start postgres

docker run -it --name postgres -d -e POSTGRES_PASSWORD="password" -p 5432:5432 -v /var/lib/docker-db/postgresql/data:/var/lib/postgresql/data postgres

docker run -it --name mongo -d -e MONGO_INITDB_ROOT_USERNAME=mongo -e MONGO_INITDB_ROOT_PASSWORD="password" -p 27017:27017 -v /var/lib/docker-db/mongodb/data:/data/db mongo

docker exec -it -u postgres bash
 
_________nginx____

after installing nginx go to : etc/nginx/sites-enabled/default

copy past these codes: 

upstream back-end {

        server localhost:7000;

}


upstream customer-home {

        server localhost:7001;

}


///server block 


location /core/ {

                proxy_pass http://back-end/;

}


location /customer-home/ {

                proxy_pass http://customer-home/;

}


