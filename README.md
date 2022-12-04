# Docker

### Introduction Tutorial
 - Give Docker a go by running a hello world container using the docker run command:

 ``` docker run --rm hello-world ```

![](/images/Introduction_tutorial_1.PNG)

<hr>

### Images Tutorial
 - Firts, register an account with Dockerhub 
 - Authenticate the docker CLI to Dockerhub with the 
 login command 
 ``` docker login ```

 - Download the oficcial Java image 
 ``` docker search Java ```

 - The search command returns a table of images relevant to the search term with their description and wether they are official image
 - User the docker pull command to download the java image
 ``` docker pull openjdk ``` 
 - Replace <your_docker_username> with your Docker username and run the following command:
 ``` docker tag java <your_docker_username>/java:7 ```

 - Now, when we run docker images, we can see two images. We have created a new image with a different name

 ![](/images/images_1.PNG)

- Now we can push the newly tagged image to our repository by tagging it in the format [USERNAME]/[IMAGE]:[TAG]

``` docker push <your_docker_username>/java:7 ```

- Navigate to Dockerhub, and you should see a new repository

 ![](/images/docker_hub.PNG)

 - Now that the image has been uploaded, we can delete the images we have locally and free up space.

 ``` docker rmi java ```

 - Repeat for the renamed image
 
 ```docker rmi <your_docker_username>/java:7```

 <hr>

 ### Tutotial Containers

 Setting up a Jenkins instance inside a container:

 - Spin up a container using the official Jenkins image
 - Map port the machine's port 8080 to the container port 8080
 
 ``` docker run -d -p 8080:8080 --name my_jenkins jenkins/jenkins ```

- Check if the container has started properly:

``` docker ps ```

- Retrieve the initial administrator password from the container

``` docker exec my_jenkins cat var/jenkins_home/secrets/initialAdminPassword ```

**d2a90d937877480481456b7d0f76b621**

- Now you have the initial admin password, copy your machine's public IP address and navigate to it in your browser on a new tab. Make sure you have allowed incoming connections on port 8080

http://3.8.190.67:8080/login?from=%2F

- You will be prompted to provide the initial admin password that you retrieved previously. Copy it in and follow the installation instructions to install the plugins

**Tear down**

- Stop the container

``` docker stop jenkins_container ```

- Deletes the container

``` docker rm jenkins_container ```
<hr>

### Tutotial Dockerfile
This exercise will get you to take the NGINX Docker Image and change the default index.html file that is served.
These changes will be packed into your own Docker Image that you can run and view the changes for yourself.

1 - Create a new directory dockerfile_exercises

```
mkdir dockerfile_exercises 
cd dockerfile_exercises 
```

2 - Make a Dockerfile

``` touch Dockerfile ```

3 - Place the following contents within the Dockerfile:
```
FROM nginx:latest
RUN printf "My custom NGINX Image\n" > /usr/share/nginx/html/index.html
```

4 - Build the image from Dockerfile and give the new image name _ournginx_

```
docker build -t ourginx .

```

5 - Run ournginx image on port 80:

```
docker run -d -p 80:80 --name nginx ourginx
```
![](/images/dockerfile.PNG)

6 - Stop and remove the container

```
docker stop nginx
docker rm nginx
```

7 - Remove the ourginx image 

```
docker rmi ourginx
```
<hr>

### Tutorial Dockerfile Instructions

For this tutorial, we will create an image for a simple Flask app to be ran in a container.

1 - create a directory called myapp with a file named app.py
```
mkdir myapp
cd myapp
touch app.py
```

2 - In app.py, enter the following code:
```
from flask import Flask
app = Flask(__name__)

@app.route('/home')
@app.route('/')
def home():
    return "Hello world!"

if __name__ == '__main__':
    app.run(port=5000, host='0.0.0.0', debug=True)
```

3 - Create the Dockerfile to build an image
```
touch Dockerfile
```

4 - Paste the instructions in the dockerfile:
```
# Python base image.
FROM python:3.7
# Create and set the work directory inside the image named 'app'
WORKDIR /app
# Execute a pip install command using the list 'requirements.txt'
RUN pip install Flask
# Copy the app file into the image working directory
COPY app.py .
# State the listening port for the container. 
# The app's code does not actually specify the port, so it would be useful to include here.
EXPOSE 5000
# Run 'python app.py' on container start-up. This is the main process.
ENTRYPOINT ["python", "app.py"]
```

5- Build the image
```
docker build -t myapp_image .
```

6 - Run a container from the image:
```
docker run -d -p 5000:5000 --name myapp myapp_image 
```

7 - Navigate to the app [IP_ADDRESS]:5000 to check

![](images/dockerfile_instructions.PNG)

8 - Teardown
```
# stop container
docker stop myapp
# remove container
docker rm myapp
# remove image
docker rmi myapp_image
```