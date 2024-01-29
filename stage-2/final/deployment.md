# Deployment
**Requirements**

-   Deployments on top Docker
-   Frontend :  [fe-dumbmerch](https://github.com/demo-dumbways/fe-dumbmerch)
-   Backend :  [be-dumbmerch](https://github.com/demo-dumbways/be-dumbmerch)

**Instructions**

[ _Database_ ]

-   App database using  _PostgreSQL_
-   Deploy postgresql on top docker
-   Set the volume location in  `/home/$USER/`
-   Allow database to remote from another server

[ _Application_ ]

-   Create a Docker image for frontend & backend
-   Staging: A lightweight docker image (as small as possible)
-   Production: Deploy a production ready app
-   Building Docker image on every environment using docker multistage build
-   Create load balancing for frontend and backend

## Database setup
- Create database using postgres on top docker compose 
- Adding pgadmin4 for postgress using UI
	```sh
	version: '3.9'

	services:
	  database:
	    container_name: postgres
	    image: postgres:14
	    ports:
	      - 5432:5432
	    volumes:
	      - ~/apps/postgres:/var/lib/postgresql/data
	    env_file:
	      - ~/apps/group_vars/variables.env
	    networks:
	      apps:

	  pgadmin:
	    container_name: pgadmin4
	    image: dpage/pgadmin4
	    restart: always
	    env_file:
	      - ~/apps/group_vars/pgadmin.env
	    ports:
	      - 5050:80
	    networks:
	      apps:
	      
	networks:
	  apps:

	```
- Adding group_vars and create variables of database
  
	group_vars/

	├── pgadmin.env

	└── variables.env

	```sh
	#variables.env
	POSTGRES_PASSWORD=XXXXX
	POSTGRES_USER=rakhadmin
	POSTGRES_DB=dumbmerch

	#pgadmin.env
	PGADMIN_DEFAULT_EMAIL: admin@admin.com
	PGADMIN_DEFAULT_PASSWORD: root

	```
## Application setup
### Backend
#### Staging
Staging will deploy on server rakha0 with domain = https://staging-be.rakha.studentdumbways.my.id/  
- Use the repository that have been configure before 
- Create dockerfile for backend application
	```sh
	FROM golang:1.19-alpine as build

	WORKDIR /app    

	COPY . .

	RUN go build -o go-docker

	FROM golang:1.19-alpine

	WORKDIR /app
	
	COPY --from=build /app .

	EXPOSE 5000

	CMD ["./go-docker"]
	```
- Build docker  with `docker build -t go-be:1.0`
- Attach to docker compose and restart docker compose
	```sh
	services:
	  database:
	    container_name: postgres
	    image: postgres:14
	    ports:
	      - 5432:5432
	    volumes:
	      - ~/apps/dumbmerch/postgres:/var/lib/postgresql/data
	    env_file:
	      - ~/apps/dumbmerch/group_vars/variables.env
	    networks:
	      apps:

	  pgadmin:
	    container_name: pgadmin4
	    image: dpage/pgadmin4
	    restart: always
	    env_file:
	      - ~/apps/dumbmerch/group_vars/pgadmin.env
	    ports:
	      - 5050:80
	    networks:
	      apps:

	  backend:
	    container_name: be
	    image: go-be:1.0
	    depends_on:
	      - database
	    ports:
	      - 5000:5000
	    networks:
	      apps:
	networks:
	apps:
	```
#### Production
Production will deploy on kubernetes cluster with domain = api-prod-rakha.studentdumbways.my.id. In this kubernetes cluster the application will perform load balancing
- When the test on staging is fixed. Then attach the docker image in the kubernetes cluster manifest
- First create go-be.yml to manifest directory on master node in kubernetes cluster
	```sh
	apiVersion: apps/v1
	kind: Deployment 
	metadata: 
	  name: go-be-deployment
	  namespace: rakha
	spec:
	  replicas: 2
	  selector: 
	    matchLabels:
	      app: go-be
	  template:
	    metadata:
	      labels:
	        app: go-be
	    spec:
	      containers:
	        - image: registry.rakha.studentdumbways.my.id/rakhafe/go-be:v1
	          name: go-be
	          ports:
	          - containerPort: 5000

	---

	apiVersion: v1
	kind: Service
	metadata:
	  name:  go-be-service
	  namespace: rakha
	spec:
	  selector:
	    app: go-be
	  type: NodePort
	  ports:
	  - name: http
	    port:  80
	    protocol: TCP
	    targetPort: 5000
	  - name: https
	    port:  443
	    protocol: TCP
	    targetPort: 5000

	---

	apiVersion: networking.k8s.io/v1
	kind: Ingress 
	metadata:
	  name: go-be-ingress
	  namespace: rakha
	  annotations:
	    kubernetes.io/ingress.class: "nginx"
	    nginx.ingress.kubernetes.io/rewrite-target: /
	spec:
	  rules:
	  - host: api-prod-rakha.studentdumbways.my.id
	    http:
	      paths:
	      - path: /
	        pathType: Prefix
	        backend:
	          service:
	            name: go-be-service
	            port:
	              number: 80

	```
	In this case, I used the docker image from my private registry
	
### Frontend
#### Staging
Staging will deploy on server rakha0 with domain = https://staging-fe.rakha.studentdumbways.my.id/  
- Use the repository that have been configure before 
- Create dockerfile for frontend application
	```sh 
	# First stage - Building the application
	FROM node:16-alpine AS build

	WORKDIR /app

	COPY . .

	RUN npm install

	# Second stage - Serve the application
	FROM node:16-alpine

	WORKDIR /app

	COPY --from=build /app .

	EXPOSE 3000

	CMD ["npm", "start"]

	```
- Build docker  with `docker build -t node-fe:1.0`
 - Attach to docker compose and restart docker compose
	```sh
	version: '3.9'

	services:
	  database:
	    container_name: postgres
	    image: postgres:14
	    ports:
	      - 5432:5432
	    volumes:
	      - ~/apps/dumbmerch/postgres:/var/lib/postgresql/data
	    env_file:
	      - ~/apps/dumbmerch/group_vars/variables.env
	    networks:
	      apps:

	  pgadmin:
	    container_name: pgadmin4
	    image: dpage/pgadmin4
	    restart: always
	    env_file:
	      - ~/apps/dumbmerch/group_vars/pgadmin.env
	    ports:
	      - 5050:80
	    networks:
	      apps:

	  backend:
	    container_name: be
	    image: go-be:1.6
	    depends_on:
	      - database
	    ports:
	      - 5000:5000
	    networks:
	      apps:

	  frontend:
	    container_name: fe
	    image: node-fe:1.1
	    depends_on:
	      - database
	      - backend
	    ports:
	      - 3000:3000
	    networks:
	      apps:

	networks:
	  apps:

	```
#### Production

Production will deploy on kubernetes cluster with domain = prod-rakha.studentdumbways.my.id. In this kubernetes cluster the application will perform load balancing
- When the test on staging is fixed. Then attach the docker image in the kubernetes cluster manifest
- First create node-fe.yml to manifest directory on master node in kubernetes cluster
	```sh
	apiVersion: apps/v1
	kind: Deployment 
	metadata: 
	  name: node-fe-deployment
	  namespace: rakha
	spec:
	  replicas: 2
	  selector: 
	    matchLabels:
	      app: node-fe
	  template:
	    metadata:
	      labels:
	        app: node-fe
	    spec:
	      containers:
	        - image: registry.rakha.studentdumbways.my.id/rakhafe/node-fe:v1
	          name: node-fe
	          ports:
	          - containerPort: 3000

	---

	apiVersion: v1
	kind: Service
	metadata:
	  name:  node-fe-service
	  namespace: rakha
	spec:
	  selector:
	    app: node-fe
	  type: NodePort
	  ports:
	  - name: http
	    port:  80
	    protocol: TCP
	    targetPort: 3000
	  - name: https
	    port:  443
	    protocol: TCP
	    targetPort: 3000

	---

	apiVersion: networking.k8s.io/v1
	kind: Ingress 
	metadata:
	  name: node-fe-ingress
	  namespace: rakha
	  annotations:
	    kubernetes.io/ingress.class: "nginx"
	    nginx.ingress.kubernetes.io/rewrite-target: /
	spec:
	  rules:
	  - host: prod-rakha.studentdumbways.my.id
	    http:
	      paths:
	      - path: /
	        pathType: Prefix
	        backend:
	          service:
	            name: node-fe-service
	            port:
	              number: 80

	```
	In this case, I used the docker image from my private registry

## Access using web browser
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2020-04-11.png?raw=true)
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2020-04-14.png?raw=true)
