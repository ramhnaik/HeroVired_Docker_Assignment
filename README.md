# Graded Assignment on Dockerizing a Plain HTML Page With Nginx
#### The objective of this assignment is to familiarize yourself with Docker and containerization by Dockerizing a simple HTML page using Nginx as the web server.

## Steps

To containerize a simple HTML page using Nginx and Docker, and then push the image to ECR (Elastic Container Registry), here are the steps:

## Step 1: Create a Simple HTML Page
Create a directory for your project and inside it, create a file named index.html with the following content:

~~~
<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dockerized Nginx</title>
</head>
<body>
    <h1>Hello, Docker!</h1>
</body>
</html>
~~~

## Step 2: Create an Nginx Configuration File
In the same directory, create an nginx.conf file to configure Nginx to serve the HTML page:

~~~
# nginx.conf
events {}

http {
    server {
        listen 80;
        location / {
            root /usr/share/nginx/html;
            index index.html;
        }
    }
}
~~~

## Step 3: Create a Dockerfile
In the same directory, create a Dockerfile to define the image:

~~~
# Dockerfile
# Use the official Nginx base image from Docker Hub
FROM nginx:alpine

# Copy the Nginx configuration file
COPY nginx.conf /etc/nginx/nginx.conf

# Copy the HTML page to the default Nginx HTML folder
COPY index.html /usr/share/nginx/html/index.html

# Expose port 80
EXPOSE 80

# Start Nginx server
CMD ["nginx", "-g", "daemon off;"]
~~~

## Step 4: Build the Docker Image
Open a terminal in the project directory and run the following command to build the Docker image:

~~~
docker build -t my-nginx-image .
~~~

This will create a Docker image named my-nginx-image.

## Step 5: Test the Docker Image Locally
Run the container to test it locally:

~~~
docker run -p 8080:80 my-nginx-image
~~~

You can access the page in a browser by visiting http://localhost:8080, and it should display "Hello, Docker!".

## Step 6: Push the Docker Image to AWS ECR
1. Create a Public ECR Repository
Log in to AWS Console, go to ECR, and create a new public repository.

2. Authenticate Docker with ECR
Use the following command to authenticate Docker with AWS ECR:
~~~
aws ecr-public get-login-password --region <region> | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.<region>.amazonaws.com
~~~

Replace <region> with your AWS region and <aws_account_id> with your AWS account ID.

3. Tag the Image
Tag your Docker image for ECR:

~~~
docker tag my-nginx-image:latest <aws_account_id>.dkr.ecr.<region>.amazonaws.com/<repository_name>:latest
~~~

4. Push the Image to ECR
Push the image to the ECR repository:
~~~
docker push <aws_account_id>.dkr.ecr.<region>.amazonaws.com/<repository_name>:latest
~~~
