# Week 1 — App Containerization

## Containerize Backend 

### Test it running on local environment

Before containerize, i'll make sure the backend app running on local environment without errors.
First, typing below commands to install the flask modules

```sh
  cd backend-flask
  pip3 install -r requirements.txt
```

Set this two Environment Variable

```sh
  export FRONTEND_URL='*'
  export BACKEND_URL='*'
```

Then running an app on port 4567

```sh
  python3 -m flask run --host=0.0.0.0 --port=4567
```
After running, click the PORT tab, click the address link which port is 4567 and append this endpoint : /api/activities/home

When succesfull, there will be json output on browser screen

![Running Test](assets/week1/running-test-backend.png)


### Create Dockerfile

![Create Dockerfile](assets/week1/Create-Dockerfile.png)


### Build Docker Image

The next step was build docker image from a Dockerfile, give it backend-flask with default latest tag

```sh
  docker build -t backend-flask .
```
Check the resulting image by typing this command

```sh
  docker images
```
![Create Docker Image](assets/week1/build-backend-flask-image.png)

### Run Container

Run container using image that was created before using these option :
```
  -p 5000:4567  => binding host port 5000 into container port 4567
  -name         => name the container a given name
  -d            => run container in background and print container ID (detach mode)
```
```sh
  docker run -p 5000:4567 -d --name backend-flask backend-flask
```
Cek and list the running container by this command

  ```sh
    docker ps
  ```
  
![Run Container](assets/week1/run-and-list-container.png)
  
Try to test the running container using curl :

```
  curl -X GET http://localhost:5000/api/activities/home -H "Accept: application/json" -H "Content-Type: application/json"
```

Ooppss! Type Error on terminal screen. 

![Type Error](assets/week1/type-error.png)


Show the logs using docker logs command

```
  docker logs backend-flask
```

![Docker Logs](assets/week1/docker-logs.png)


The environment variable seems not set yet. Try to debug using docker exec command and check the environment variables.

```
  docker exec -it backend-flask bash
```
![Docker Exec](assets/week1/docker-exec.png)


Stop and remove the container, re-run the command with additional option -e to set enviroment variables 

```sh
  docker stop backend-flask
  docker rm backend-flask
  docker run -p 5000:4567 -d --name backend-flask -e FRONTEND_URL='*' -e BACKEND_URL='*'  backend-flask
```
Then test again. Success !

![Re-run Container](assets/week1/re-run-container.png)


## Containerize Frontend 

### Create Dockerfile

Change directory to frontend-react-js and create Dockerfile

```
  cd frontend-react-js
  touch Dockerfile
```

![Create Dockerfile](assets/week1/Create-Dockerfile-Frontend.png)


### Build Docker Image

Build docker image from a Dockerfile, give it frontend-react-js with default latest tag

```sh
  docker build -t frontend-react-js .
```

![Create Docker Image](assets/week1/build-frontend-image.png)


Check the resulting image by typing this command

```sh
  docker images
```
![Docker Images](assets/week1/Docker-images.png)


### Run Container

```sh
  docker run -p 6000:3000 -d --name frontend-react-js frontend-react-js
```
Cek and list the running container by this command

  ```sh
    docker ps
  ```
![Run Container](assets/week1/run-fontend-container.png)


Test the running frontend container, click the PORT tab on terminal, click the address link which port is 6000

![Address Link](assets/week1/frontend-link.png)


Output show on browser

![Frontend](assets/week1/frontend-test.png)


## Multiple Containers

Here I would build and running backend and frontend containers using **docker-compose**

### Run NPM Install

We have to run **npm install** in order to downloads a package and it's dependencies before building the container

```
  cd frontend-react-js
  npm install
```

### Create a docker-compose file

Go back to the root directory of the project workspace from frontend-react-js directory, create docker-compose.yml

```
  cd ..
  touch docker-compose.yml
```
As the the same with what i am doing before on the containerize Backend and Frontend, I binding the host port 5000 to backend container port 4567 and bind the host port 6000 to frontend container port 3000

![docker-compose](assets/docker-compose.png)

### Run docker-compose

```
  docker compose -f docker-compose.yml up
```
When successfull, backend container, frontend container and network was created

![docker-compose-result1](assets/docker-compose-result1.png)


Click on PORTS tab, URL Address and port would look like below :

![docker-compose-result2](assets/docker-compose-result2.png)


### Testing

Click the frontend URL, the frontend page appear but seem the data from backend not load. 

![console inspect](assets/test-not-success-yet.png)


#### Inspecting 

I inspect backend and frontend container to check if the environment variable are set correctly. 

I clicked the docker tab on left side VSCode web, then right click backend container, click on inspect and check the port bindings and env list. I found they were set correctly.

![inspect backend](assets/inspect-backend-container.png)


Right click frontend container, click on inspect and check the port bindings and env list. I found they were set correctly.

![inspect frontend](assets/inspect-frontend-container.png)


Then I inspect the web page, and found CORS issue and a 401 Unauthorized error.

![console inspect](assets/inspect-console.png)


![network inspect](assets/inspect-network.png)


After trying any references on the same issue on discord like change the origin into anywhere, insert @cross_origin() to any route on file app.py, etc ... **I resolve the issue by make the backend to be public**

![public backend](assets/backend-to-be-public.png)

After re-test, frontend and backend worked each other

![Success](assets/test-success.png)




## Notifications

I would add notifications endpoint on backend and notification page on frontend.

### Backend Notification Endpoint

  1. Create function that return JSON list on NotificationsActivities class, save as notifications_activities.py inside services folder

      ![Notification Class](assets/week1/notifications-activities-py.png)

  2. Open app.py file on backend-flask folder, Import all module from services.notifications_activities and /
     create API endpoint /api/activities/notifications to return the result

      ![Api Endpoint](assets/week1/notifications-api-endpoint.png)
      
      
#### Test Notification API Endpoint

I build and running backend container, click the Backend URL addresss, append /api/activities/notifications, enter or reload the page and got the result as expected.

![Notification Api Endpoint Test](assets/week1/notifications-api-endpoint-test.png)


#### Update OpenAPI Document for Notifications Endpoint

I open openapi-3.0.yml file to add new path for /api/activities/notifications

![OpenAPI Document Update](assets/week1/openapi-document-update.png)



### Frontend Notifications Page

  1. I create Notification page with this steps :
      - Go to frontend-react-js/src/pages directory
      - Copy HomeFeedPage.js and rename to be NotificationPage.js
      - Copy HomeFeedPage.css and rename to be NotificationPage.css
      - edit NotificationPage.js, importing NotificationPage.css, change function name, change backend_url with API endpoint i created before, change title page

      ![Notification Page](assets/week1/notification-pages.png)

  2. I opened the app.js file, import NotificationPage and add route /notifications to NotificationPage

      ![App.js File](assets/week1/edit-app-js.png)
      
      
#### Test Notifications Page

I am running docker compose, click the Frontend URL addresss, Login into Cruddur, and click Notifications on left side bar

![Notification](assets/week1/notification-test-success.png)


## Deploy DynamoDB and PostgreSQL Locally

I added dynamoDB, Postgress and volume on docker-compose.yml file

![docker-compose edit](assets/week1/docker-compose-edit.png)

I running docker-compose via VS Code Menu, right click docker-compose.yml, then click compose up

![compose up](assets/week1/compose-up.png)

All containers are up !

![All Containers](assets/week1/all-containers-are-up.png)


### Test DynamoDB Local

I follow [this Example of using DynamoDB local](https://github.com/100DaysOfCloud/challenge-dynamodb-local) and using AWS CLI to create table, item, list table and item.

#### Create Table

```
  aws dynamodb create-table \
      --endpoint-url http://localhost:8000 \
      --table-name Music \
      --attribute-definitions \
          AttributeName=Artist,AttributeType=S \
          AttributeName=SongTitle,AttributeType=S \
      --key-schema AttributeName=Artist,KeyType=HASH AttributeName=SongTitle,KeyType=RANGE \
      --provisioned-throughput ReadCapacityUnits=1,WriteCapacityUnits=1 \
      --table-class STANDARD
```

![create tables](assets/week1/create-table-dynamodb-local.png)


#### Show List of Tables created

```
  aws dynamodb list-tables --endpoint-url http://localhost:8000
```

![show tables](assets/week1/show-table-created.png)


#### Create an Item

```
  aws dynamodb put-item \
    --endpoint-url http://localhost:8000 \
    --table-name Music \
    --item \
        '{"Artist": {"S": "No One You Know"}, "SongTitle": {"S": "Call Me Today"}, "AlbumTitle": {"S": "Somewhat Famous"}}' \
    --return-consumed-capacity TOTAL  
```

![create item](assets/week1/create-item-dynamodb-local.png)


#### Get and show the Items

```
  aws dynamodb scan --table-name Music --query "Items" --endpoint-url http://localhost:8000
```

![Get items](assets/week1/get-items-dynamodb-local.png)


### Test PostgreSQL

I add init configuration on file gitpod.yml to install postgress client whenever new Gitpod enviroment loaded and install postgeSQL client extention for vs code.

![Install postgreSQL Client](assets/week1/install-postgreSQL-client.png)


Login to postgreSQL using psql command on terminal

```
   psql -h localhost -Upostgres
```

I also test it using Database Explorer, create connection and connected successfully

![Connection Test](assets/week1/postgres-test-success.png)



## Homework Challenges

### Push and tag a image to DockerHub

Requirements:
  1. A docker hub account.
  2. Installed docker software.
  3. An image which you want to push to docker hub repository.

I already have docker hub account and docker software installed on gitpod environment. I prepared the backend-flask image and frontect-react-js image as i do in the Containerize Backend and Frontend above.

```
  docker build -t backend-flask ./backend-flask
  docker build -t frontend-react-js ./frontend-react-js
  docker images
```

![Docker Images](assets/week1/hwc-docker-images.png)

To push the image into docker hub :
  1. Tag the local image into MY_DOCKERHUB_ACCOUNT_NAME/IMAGE_NAME:TAG. 

      ![Docker Tag](assets/week1/hwc-docker-tag.png)

  2. Login into docker hub using docker login command and input my docker hub user and password

      ![Docker Login](assets/week1/hwc-docker-login.png)
      
  3. Push the new tag into docker hub using docker push command

      ![Docker Login](assets/week1/hwc-docker-push.png)

  4. Check on the docker hub repositories, there are two repositories exist with contain 1 image each

      ![Docker Hub](assets/week1/hwc-docker-hub.png)
      
      
### Deploy and Run Container using Docker on Local Computer

The steps are :
  1. I install docker on my windows PC following [Install Docker Desktop on Windows documentation](https://docs.docker.com/desktop/install/windows-install/)
  2. I am using Visual Studio Code on my local computer as an IDE. Then I installed Docker and GitHub Pull Requests and Issues extentions in My VS Code
  3. Login into my github account from vs code and clone the aws-bootcamp-cruddur-2023 repository
  
     ![Git Clone](assets/week1/hwc-git-clone.png)
     
  4. Run Docker Desktop and connect to it from vs code

     ![Docker on VSC](assets/week1/hwc-docker-on-vsc.png)

  6. Click docker-compose.yml file, then select compose up. The resulting container were started and up.

     ![Compose Up](assets/week1/hwc-deploy-container-locally.png)
     
     
     ![Docker Desktop](assets/week1/hwc-docker-desktop.png)
















































  
  

  
  
  






