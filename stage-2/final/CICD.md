# CICD
**Requirements**

-   CI/CD with CircleCI

**Instructions**
[ _CI/CD_ ]
-   Using CircleCI, create a pipeline running:
    -   CircleCI server
    -   Repository pull
    -   Image build
    -   Testing
    -   Push Image into your own docker registry private
    -   SSH into your biznet server
    -   Pull image from docker registry private
    -   Redeploy your deployment apps

## Preamble
In this case, I used a combination of 2 different tools. On Staging side I used Gitlab-ci for doing CICD. On Production side I used CircleCI for doing CICD. As for **Staging**, it would be processed in the Gitlab repository with Gitlab-ci tool. As for **Production** would be processed in the github repository with circleci connection

## Set git remote for Staging and Production
- Backend
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2020-41-08.png?raw=true)
- Frontend
	In this case, the remote github will point to v2 because the default repository has constraint	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2020-43-08.png?raw=true)
	
## Staging
### Backend
- Create file .gitlab-ci.yml on branch staging
	```sh
	stages:
	  - pulling-repository
	  - build
	  - test
	  - push

	Pulling code:
	  stage : pulling-repository
	  image: docker
	  services:
	    - docker:dind
	  before_script:
	    - 'which ssh-agent || ( apt-get install -qq openssh-client )'
	    - mkdir -p ~/.ssh
	    - touch ~/.ssh/id_rsa
	    - echo "$SSH_PRIVATE_KEY" | tr -d '\r' > ~/.ssh/id_rsa
	    - chmod 600 ~/.ssh/id_rsa
	    - echo -e "Host *\nStrictHostKeyChecking no\n" > ~/.ssh/config
	    - eval "$(ssh-agent -s)"
	    - ssh-add ~/.ssh/id_rsa


	  script :
	    - ssh -o StrictHostKeyChecking=no $VAR_USER@$VAR_IP -p $SSH_PORT "cd $VAR_DIREKTORI && git pull origin staging && exit"
	    - echo "pulling code success!"

	Dockerize:
	  stage : build
	  image: docker
	  services:
	    - docker:dind
	  before_script:
	    - 'which ssh-agent || ( apt-get install -qq openssh-client )'
	    - mkdir -p ~/.ssh
	    - touch ~/.ssh/id_rsa
	    - echo "$SSH_PRIVATE_KEY" | tr -d '\r' > ~/.ssh/id_rsa
	    - chmod 600 ~/.ssh/id_rsa
	    - echo -e "Host *\nStrictHostKeyChecking no\n" > ~/.ssh/config
	    - eval "$(ssh-agent -s)"
	    - ssh-add ~/.ssh/id_rsa


	  script : 
	    - ssh -o StrictHostKeyChecking=no $VAR_USER@$VAR_IP -p $SSH_PORT "cd $VAR_DIREKTORI && docker build -t $VAR_IMAGE:$CI_PIPELINE_IID ."
	    - ssh -o StrictHostKeyChecking=no $VAR_USER@$VAR_IP -p $SSH_PORT "cd $VAR_DIREKTORI && sed -i '29c\\    image:\ $VAR_IMAGE:$CI_PIPELINE_IID' /home/finaltask-rakha/apps/fe-dumbmerch/docker-compose.yaml"
	    - ssh -o StrictHostKeyChecking=no $VAR_USER@$VAR_IP -p $SSH_PORT "cd $VAR_DIREKTORI && cd ../fe-dumbmerch && docker compose down backend"
	    - ssh -o StrictHostKeyChecking=no $VAR_USER@$VAR_IP -p $SSH_PORT "cd $VAR_DIREKTORI && cd ../fe-dumbmerch && docker compose up -d backend && exit "
	    - ssh -o StrictHostKeyChecking=no $VAR_USER@$VAR_IP -p $SSH_PORT "cd $VAR_DIREKTORI && docker system prune -a -f"
	    - echo "building application success!"

	Testing:
	  stage : test
	  image: docker
	  services:
	    - docker:dind
	  before_script:
	    - 'which ssh-agent || ( apt-get install -qq openssh-client )'
	    - mkdir -p ~/.ssh
	    - touch ~/.ssh/id_rsa
	    - echo "$SSH_PRIVATE_KEY" | tr -d '\r' > ~/.ssh/id_rsa
	    - chmod 600 ~/.ssh/id_rsa
	    - echo -e "Host *\nStrictHostKeyChecking no\n" > ~/.ssh/config
	    - eval "$(ssh-agent -s)"
	    - ssh-add ~/.ssh/id_rsa

	  script : 
	    - ssh -o StrictHostKeyChecking=no $VAR_USER@$VAR_IP -p $SSH_PORT "cd $VAR_DIREKTORI"
	    - ssh -o StrictHostKeyChecking=no $VAR_USER@$VAR_IP -p $SSH_PORT "curl localhost:5000"
	    - echo "testing application success!"

	Push image to registry:
	  stage : push
	  image: docker
	  services:
	    - docker:dind
	  before_script:
	    - 'which ssh-agent || ( apt-get install -qq openssh-client )'
	    - mkdir -p ~/.ssh
	    - touch ~/.ssh/id_rsa
	    - echo "$SSH_PRIVATE_KEY" | tr -d '\r' > ~/.ssh/id_rsa
	    - chmod 600 ~/.ssh/id_rsa
	    - echo -e "Host *\nStrictHostKeyChecking no\n" > ~/.ssh/config
	    - eval "$(ssh-agent -s)"
	    - ssh-add ~/.ssh/id_rsa


	  script : 
	    - ssh -o StrictHostKeyChecking=no $VAR_USER@$VAR_IP -p $SSH_PORT "docker push $VAR_IMAGE:$CI_PIPELINE_IID && exit"
	    - echo "Push image to registry success!"

	```
- Setting variables on gitlab repository
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2021-54-11.png?raw=true)
- git push to gitlab repository using gitlab remote on branch staging
	```sh
	git push gitlab staging
	```
- Pipeline result
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2021-56-56.png?raw=true)
### Frontend
- Create file .gitlab-ci.yml on branch staging
```sh
stages:
  - pulling-repository
  - build
  - test
  - push

Pulling code:
  stage : pulling-repository
  image: docker
  services:
    - docker:dind
  before_script:
    - 'which ssh-agent || ( apt-get install -qq openssh-client )'
    - mkdir -p ~/.ssh
    - touch ~/.ssh/id_rsa
    - echo "$SSH_PRIVATE_KEY" | tr -d '\r' > ~/.ssh/id_rsa
    - chmod 600 ~/.ssh/id_rsa
    - echo -e "Host *\nStrictHostKeyChecking no\n" > ~/.ssh/config
    - eval "$(ssh-agent -s)"
    - ssh-add ~/.ssh/id_rsa


  script :
    - ssh -o StrictHostKeyChecking=no $VAR_USER@$VAR_IP -p $SSH_PORT "cd $VAR_DIREKTORI && git pull origin staging && exit"
    - echo "pulling code success!"

Dockerize:
  stage : build
  image: docker
  services:
    - docker:dind
  before_script:
    - 'which ssh-agent || ( apt-get install -qq openssh-client )'
    - mkdir -p ~/.ssh
    - touch ~/.ssh/id_rsa
    - echo "$SSH_PRIVATE_KEY" | tr -d '\r' > ~/.ssh/id_rsa
    - chmod 600 ~/.ssh/id_rsa
    - echo -e "Host *\nStrictHostKeyChecking no\n" > ~/.ssh/config
    - eval "$(ssh-agent -s)"
    - ssh-add ~/.ssh/id_rsa


  script : 
    - ssh -o StrictHostKeyChecking=no $VAR_USER@$VAR_IP -p $SSH_PORT "cd $VAR_DIREKTORI && docker build -t $VAR_IMAGE:$CI_PIPELINE_IID ."
    - ssh -o StrictHostKeyChecking=no $VAR_USER@$VAR_IP -p $SSH_PORT "cd $VAR_DIREKTORI && sed -i '39c\\    image:\ $VAR_IMAGE:$CI_PIPELINE_IID' docker-compose.yaml"
    - ssh -o StrictHostKeyChecking=no $VAR_USER@$VAR_IP -p $SSH_PORT "cd $VAR_DIREKTORI && docker compose down frontend"
    - ssh -o StrictHostKeyChecking=no $VAR_USER@$VAR_IP -p $SSH_PORT "cd $VAR_DIREKTORI && docker compose up -d frontend && exit "
    - ssh -o StrictHostKeyChecking=no $VAR_USER@$VAR_IP -p $SSH_PORT "cd $VAR_DIREKTORI && docker system prune -a -f"
    - echo "building application success!"

Testing:
  stage : test
  image: docker
  services:
    - docker:dind
  before_script:
    - 'which ssh-agent || ( apt-get install -qq openssh-client )'
    - mkdir -p ~/.ssh
    - touch ~/.ssh/id_rsa
    - echo "$SSH_PRIVATE_KEY" | tr -d '\r' > ~/.ssh/id_rsa
    - chmod 600 ~/.ssh/id_rsa
    - echo -e "Host *\nStrictHostKeyChecking no\n" > ~/.ssh/config
    - eval "$(ssh-agent -s)"
    - ssh-add ~/.ssh/id_rsa


  script : 
    - ssh -o StrictHostKeyChecking=no $VAR_USER@$VAR_IP -p $SSH_PORT "cd $VAR_DIREKTORI"
    - ssh -o StrictHostKeyChecking=no $VAR_USER@$VAR_IP -p $SSH_PORT "wget --spider localhost:3000"
    - echo "testing application success!"

Push image to registry:
  stage : push
  image: docker
  services:
    - docker:dind
  before_script:
    - 'which ssh-agent || ( apt-get install -qq openssh-client )'
    - mkdir -p ~/.ssh
    - touch ~/.ssh/id_rsa
    - echo "$SSH_PRIVATE_KEY" | tr -d '\r' > ~/.ssh/id_rsa
    - chmod 600 ~/.ssh/id_rsa
    - echo -e "Host *\nStrictHostKeyChecking no\n" > ~/.ssh/config
    - eval "$(ssh-agent -s)"
    - ssh-add ~/.ssh/id_rsa


  script : 
    - ssh -o StrictHostKeyChecking=no $VAR_USER@$VAR_IP -p $SSH_PORT "docker push $VAR_IMAGE:$CI_PIPELINE_IID && exit"
    - echo "Push image to registry success!"

```
- Setting variables on gitlab repository
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2021-54-11.png?raw=true)

- git push to gitlab repository using gitlab remote on branch staging
	```sh
	git push gitlab staging
	```
- Pipeline result
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2021-56-56.png?raw=true)

## Production
### Backend
- Switch branch to circleci-setup-project (production)
- Create folder .circleci  and create config.yml into it. This stuff will run the CICD on production server
	```sh
	version: 2.1

	jobs:
	  deploy:
	    machine:
	      image: ubuntu-2204:2023.07.2
	    steps:
	      - run:
	          name: Deploy Over SSH
	          command: |
	            ssh -o StrictHostKeyChecking=no $SSH_USER@$SSH_HOST -p $SSH_PORT "sed -i '17c\\        - image: registry.rakha.studentdumbways.my.id/rakhafe/go-be:v1' /var/lib/rancher/k3s/server/manifests/go-be.yml"

	workflows:
	  deploy-production:
	    jobs:
	      - deploy
	```
- Create new project > github connection > give a name project and attach the ssh key github > choose be-dumbmerch
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2022-17-45.png?raw=true)
- Setting SSH key host to run deploy over ssh on circleci website. Insert Host of the kubernetes cluster member
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2022-20-07.png?raw=true)
	
- Setting variables
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2022-22-27.png?raw=true)

- git push using github remote and circleci-setup-project branch
	```sh
	 git push github circleci-setup-project
	```
- Result pipeline
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2022-23-42.png?raw=true)

### Frontend
- Switch branch to circleci-setup-project (production)
- Create folder .circleci  and create config.yml into it. This stuff will run the CICD on production server
	```sh
	version: 2.1

	jobs:
	  deploy:
	    machine:
	      image: ubuntu-2204:2023.07.2
	    steps:
	      - run:
	          name: Deploy Over SSH
	          command: |
	            ssh -o StrictHostKeyChecking=no $SSH_USER@$SSH_HOST -p $SSH_PORT "sed -i '17c\\        - image: registry.rakha.studentdumbways.my.id/rakhafe/node-fe:v1' /var/lib/rancher/k3s/server/manifests/node-fe.yaml"

	workflows:
	  deploy-production:
	    jobs:
	      - deploy
	```
- Create new project > github connection > give a name project and attach the ssh key github > choose be-dumbmerch
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2022-17-45.png?raw=true)
- Setting SSH key host to run deploy over ssh on circleci website. Insert Host of the kubernetes cluster member
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2022-20-07.png?raw=true)
	
- Setting variables
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2022-22-27.png?raw=true)

- git push using github remote and circleci-setup-project branch
	```sh
	 git push github circleci-setup-project
	```
- Result pipeline
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2022-23-42.png?raw=true)
