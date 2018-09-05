---
title: Docker commands cheatsheet
layout: post
---
<img src="https://files.realpython.com/media/flask-docker.272232b872e7.png"/>     
In this article, we will learn how to run a flask application in a docker container by following these simple steps:

### Step 1: Create a simple flask application    
First of all, lets create a simple flask application. Save the code shown below as ***app.py***.

<pre><code>
from flask import Flask
app = Flask(__name__)
@app.route("/")
def joke():
    return "I haven't slept for three days, because that would be too long"
if __name__ == "__main__":
    app.run(host="0.0.0.0", port=8080)
</code></pre>

### Step 2: Create a Dockerfile  
Now that we have a flask application, let us create a Dockerfile to install the necessary dependencies and run it in a docker container. Save the following code as ***Dockerfile*** in the same directory as your ***app.py***.

<pre><code>
FROM ubuntu:16.04

RUN apt-get update -y    
RUN apt-get install python -y    
RUN apt-get install python-pip -y    
RUN pip install flask

COPY app.py /home/app.py

ENTRYPOINT FLASK_APP=/home/app.py flask run --host=0.0.0.0
</code></pre>
This ***RUN*** commands in this Dockerfile installs python, python-pip and flask in our docker container. The ***COPY*** command copies the app.py file in our computer to /home directory of our docker container. The ***ENTRYPOINT*** command starts the flask application when you run the docker container.

### Step 3: Building a docker image    
In this step, we will build a docker image using the Dockerfile we created in step 2. Run the following command to build an image:
<pre><code>docker build . -t flask-app</code></pre>
You can verify that this image is created by running
<pre><code>docker images</code></pre>

### Step 4: Running the flask app in docker container
We are all set to run our flask application in a docker container. We can do this by running the following command:
<pre><code>docker run -p 8080:5000 flask-app</code></pre>
You should see output like this    
<img src="https://cdn-images-1.medium.com/max/800/1*X82xd1fUgDJGEOpKBnCbtQ.png"/>

### Step 5: Testing our application
Open a web browser and go to **http://localhost:8080/** to verify that the application is up and running.