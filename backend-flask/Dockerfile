FROM python:3.10-slim-buster

# Make working directory inside container
WORKDIR /backend-flask

#install flask modules
COPY requirements.txt requirements.txt
RUN pip3 install -r requirements.txt

#copy backend-flask source code into working directory inside container
COPY . .

#Define and set environment variable 
ENV FLASK_ENV=development

#expose the port
EXPOSE ${PORT}

#run the command
# python3 -m flask run --host=0.0.0.0 --port=4567
CMD [ "python3", "-m" , "flask", "run", "--host=0.0.0.0", "--port=4567"]