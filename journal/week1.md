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
After running, click the PORT tab, click the address link whic port is 4567 and append this endpoint : /api/activities/home

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



  

  
  
  






