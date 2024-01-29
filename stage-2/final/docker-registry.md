# Docker Private Registry
**Requirements**

-   Docker Registry Private

**Instructions**

[ _Docker Registry_ ]

-   Deploy Docker Registry Private on Registry Server

## Create docker compose file
- Create folder name registry and create docker-compose.yaml in it
```sh
version: '3'

services:
    docker-registry:
        container_name: registry.rakha.studentdumbways.my.id
        image: registry:2
        ports:
            - 5000:5000
        restart: always
        volumes:
            - ./volume-reg:/var/lib/registry

    docker-registry-ui:
        container_name: docker-registry-ui
        image: konradkleine/docker-registry-frontend:v2
        ports:
            - 8080:80
        environment:
            ENV_DOCKER_REGISTRY_HOST: registry.rakha.studentdumbways.my.id
            ENV_DOCKER_REGISTRY_PORT: 5000
```
## Create reverse proxy
- Create reverse proxy into webserver
```sh
server {
        listen 80;
        listen 443 ssl;
        server_name registry.rakha.studentdumbways.my.id;

        ssl_certificate /etc/letsencrypt/live/rakha.studentdumbways.my.id/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/rakha.studentdumbways.my.id/privkey.pem;
        client_max_body_size            0;
    	chunked_transfer_encoding       on;

        location / {
                proxy_pass http://103.179.57.85:8080/;
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
		auth_basic off;
		allow all;
	}

    	location /v2/rakhafe {
      		proxy_pass                    http://103.179.57.85:5000;
	      	proxy_set_header              Host  $host;
      		proxy_read_timeout            900;
      		auth_basic                    off;
	}
 }


```
## Access and push image 
- Access UI registry using domain that we have configured on reverse proxy
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2017-49-48.png?raw=true)
-  We can push the docker image from server with the condition that the name must be set as follows, example :
	```sh
	registry.rakha.studentdumbways.my.id/rakhafe/node-fe:tag
	```
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2017-50-05.png?raw=true)
