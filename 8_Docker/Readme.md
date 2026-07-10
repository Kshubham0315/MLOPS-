 Docker Master Experiment (6 Stages)

Is experiment ke baad tumhe ye concepts practically samajh aa jayenge:

✅ Docker Images
✅ Docker Containers
✅ Dockerfile
✅ Volumes
✅ Bind Mounts
✅ Environment Variables
✅ Docker Networks
✅ Docker Compose
✅ Container Logs
✅ Image Layers
✅ Port Mapping
✅ Multi-container Applications
Stage 1: Hello Docker
Folder
Docker-Learning/
    app.py
    requirements.txt
    Dockerfile
app.py
from flask import Flask

app = Flask(__name__)

@app.route("/")
def home():
    return "Hello Docker!"

app.run(host="0.0.0.0", port=5000)

requirements.txt

flask

Dockerfile

FROM python:3.11

WORKDIR /app

COPY requirements.txt .

RUN pip install -r requirements.txt

COPY . .

CMD ["python","app.py"]

Build Image

docker build -t flask-demo .

Run

docker run -p 5000:5000 flask-demo

👉 Yahan tum seekhoge:

Image kya hoti hai
Container kya hota hai
Build kaise hota hai
Port Mapping
Stage 2: Docker Layers

Ab Dockerfile ko modify karo.

FROM python:3.11

WORKDIR /app

COPY requirements.txt .

RUN pip install -r requirements.txt

COPY . .

CMD ["python","app.py"]

Ek baar build karo.

Fir sirf app.py change karo.

Fir dobara build.

Observe:

Using cache

Ab requirements.txt change karo.

Fir build.

Observe:

Poora pip install dobara hoga.

Isse Docker Layers samajh aayenge.

Stage 3: Volumes

Container run karo

docker run -v myvolume:/app/data flask-demo

Python code

with open("data/test.txt","w") as f:
    f.write("Hello")

Container delete kar do.

docker rm

Fir naya container banao.

File fir bhi hogi.

Yahi Volume hai.

Stage 4: Bind Mount

Run

docker run -p 5000:5000 -v ${PWD}:/app flask-demo

Windows PowerShell:

docker run -v ${PWD}:/app flask-demo

Code change karo.

Container restart.

Image build nahi karni padegi.

Ye development mode hai.

Stage 5: Environment Variables

Python

import os

print(os.getenv("NAME"))

Run

docker run -e NAME=Shubham flask-demo

Output

Shubham
Stage 6: Multiple Containers (🔥 Best Part)

Ab Flask app ko SQLite ki jagah PostgreSQL se connect karo.

Run PostgreSQL

docker run \
--name postgres \
-e POSTGRES_PASSWORD=1234 \
-p 5432:5432 \
postgres

Ab Flask app ko connect karo.

Tum seekh jaoge:

Networking
Container Communication
Stage 7: Docker Network

Network banao

docker network create mynetwork

Run PostgreSQL

docker run \
--network=mynetwork \
--name postgres \
postgres

Run Flask

docker run \
--network=mynetwork \
flask-demo

Python

host="postgres"

IP ya localhost use nahi karna.

Docker khud resolve karega.

Stage 8: Docker Compose

Instead of

docker run

docker run

docker run

Ek file

version: "3"

services:

  app:
    build: .
    ports:
      - "5000:5000"

  postgres:
    image: postgres

    environment:
      POSTGRES_PASSWORD: 1234

Run

docker compose up

Bas.

Stage 9: Logs
docker ps

docker logs container_name

Live Logs

docker logs -f container_name
Stage 10: Container Shell
docker exec -it container_name bash

Ab tum container ke andar ho.

Commands

ls

pwd

python

pip list
Stage 11: Stop vs Remove
docker stop

docker start

docker restart

docker rm

Difference observe karo.

Stage 12: Image vs Container
docker images

docker ps

docker ps -a

Delete Image

docker rmi image_name

Delete Container

docker rm container_name

Difference samajh aa jayega.

Stage 13: Push to Docker Hub

Login

docker login

Tag

docker tag flask-demo username/flask-demo:v1

Push

docker push username/flask-demo:v1

Dusri machine

docker pull username/flask-demo:v1

Run

docker run username/flask-demo:v1
🎯 Final Challenge (Real MLOps
