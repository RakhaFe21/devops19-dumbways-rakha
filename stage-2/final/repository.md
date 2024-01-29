# Repository

**Requirements**

-   Personal Github/GitLab accounts
-   Frontend :  [fe-dumbmerch](https://github.com/demo-dumbways/fe-dumbmerch)
    -   NodeJS v16.x or above
    -   Create .env file for FE+BE Integration (REACT_APP_BASEURL)
-   Backend :  [be-dumbmerch](https://github.com/demo-dumbways/be-dumbmerch)
    -   Golang v1.16.x or above
    -   Modify .env file for DB Integration

**Instructions**

-   Create a repository on Github or Gitlab
-   **Private**  repository access
-   Set up 2 branches
    -   Staging
    -   Production
-   Each Branch have their own CI/CD

---
## Edit the source code
**Backend**
1. Clone source code to the Appserver 
2. Modify .env file for DB Integration
	```sh
	#SECRET_KEY=bolehapaaja
	PATH_FILE=http://localhost:5000/uploads/
	SERVER_KEY=SB-Mid-server-fJAy6udMPnJCIyFguce8Eot3
	CLIENT_KEY=SB-Mid-client-YUogx3u74Gq9MTMS
	EMAIL_SYSTEM=demo.dumbways@gmail.com
	PASSWORD_SYSTEM=rbgmgzzcmrfdtbpu
	DB_HOST=103.127.132.63
	DB_USER=rakhadmin
	DB_PASSWORD=Kakikukaku123
	DB_NAME=dumbmerch
	DB_PORT=5432
	PORT=5000
	```
3. Add branch for staging and production which is :
	```sh
	Staging = staging
	Production = circleci-setup-project
	```
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2016-01-21.png?raw=true)
4.  Create a private repository in github server and push this Backend repository
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2016-05-19.png?raw=true)

**Frontend**
1. Clone source code to the Appserver
 2.   Create .env file for FE-BE Integration
		```sh
		REACT_APP_BASEURL=https://api.rakha.studentdumbways.my.id/api/v1
		```
 3. Add branch for staging and production which is :
	```sh
	Staging = staging
	Production = circleci-setup-project
	```
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2016-23-31.png?raw=true)

4.  Create a private repository in github server and push this Frontend repository 
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2016-55-05.png?raw=true)
