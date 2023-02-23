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

The next step was build docker image from a Dockerfile, give it tag as backend-flask

```sh
  docker build -t backend-flask .
```
Check the resulting image by typing this command

```sh
  docker images
```



