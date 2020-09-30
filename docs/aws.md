# AWS Guide

[Return to Table of Contents](../README.md)

## Intro

There are the most popular ways to use AWS:

1. Create EC2 instance, connect to this instance with SSH, install docker clone your repo, and start your docker container here.
2. Use ECS. Here you should create a cluster, build your docker image to ECR, then create service, and start this service with a task.
3. Also, you can pack your project to a zip archive and deploy it with AWS Elastic Beanstalk
   It's not all ways to deploy your project, but they are the most popular and easier.
   ​
   In this tutorial, we going to try two from this small list

### Prerequisites

First of all, you, of course, need to install [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html) and [Docker](https://docs.docker.com/docker-for-mac/install/).
After that, you should configure it for your project, for this use command

```bash
-aws configure
```

Here you have to add your secret and access keys, region, and format(usually it "json").
​
Good, now you can work with AWS from your console. It will need a little later

#### Docker Files

Add Dockerfile and .dockerignore. In .dockerignore file add:

```
.git
node_modules
npm-debug
package-lock.json
```

​
And in Dockerfile add:

```bash
FROM node:latest
WORKDIR /
COPY package.json /
RUN npm install
COPY . /
EXPOSE 3000
CMD npm run start:prod
```

#### Node.js Project Docker File

​
For a node.js project, where EXPOSE it's port for running your project, and:

```bash
FROM node:latest as builder
WORKDIR /app
COPY package.json /app
RUN npm install
COPY . /app
RUN npm run build
​
FROM nginx
​
COPY nginx.conf /etc/nginx/conf.d/default.conf
​
COPY --from=builder /app/build /usr/share/nginx/html
​
EXPOSE 80
​
CMD ["nginx", "-g", "daemon off;"]
```

​
for a web project. Also if you want to deploy a web project your need to add nginx.conf file in the root.

```bash
server {
   listen       80;
   server_name  [your server name];
   if ($http_x_forwarded_proto = "http") {
       return 301 https://$server_name$request_uri;
   }
   location / {
       root   /usr/share/nginx/html;
       index  index.html index.htm;
       try_files $uri /index.html;
   }
   error_page   500 502 503 504  /50x.html;
   location = /50x.html {
       root   /usr/share/nginx/html;
   }
}
```

Then build your images with the command:
​

```bash
- docker build -t [name] .
```

​
Check them on the local machine. It should be run on the same port as you set in Dockerfile
​
Test your build on low memory machine with the next command:
​

```bash
- docker run --memory="512m" -p [port] [image name]
```

​
We run this with 512 Mb because the smallest and free instance has
exactly the same amount of memory, this information will need a little later. And finally, check how it all works. If all works well, go to the next step. If not, update the memory limit to 1024 and double-check previous steps and [try again](#prerequisites)

## Deploy to EC2

Let's create an EC2 instance. For this go to AWS console, click on search input, and choose EC2 here. After this on the left side panel select "instances" and click on the "Launch instance" button. In the first step, you have to choose the type of instance, I'm proposing select "free only", on the left panel, and select "Ubuntu Server", because of it most popular and easier Linux system. Click next and choose instance(at list "micro").
​
<img style="transform: scale(0.65)" src="https://user-images.githubusercontent.com/22969597/93709556-cc79b700-fb47-11ea-8db3-9e430ee99398.png">
​
Click the "next" button. Here enter the number of instances that you need and choose or create a VPC. If you already have some project and this project somehow connected with each other, select VPC, which this project using and click "Next".
​
Here select existing or create a new Security Group with ports which you will use
​
<img style="transform: scale(0.65)" src="https://user-images.githubusercontent.com/22969597/93709559-d00d3e00-fb47-11ea-9e50-17094e1f2970.png">
If it is a simple project you can skip two next steps and going to configure Security Group.
​
Click the "next" button and here double-check all information and after that click "launch". You will see the modal window with a propose to select existing or create a new SSH key. Do it, and click "Launch instances"
​
Ok, now we going to connect to this instance, install docker here and run our project. For this select your instance in the list and click "connect" on the top of the page and select
​

```bash
EC2 Instance Connect (browser-based SSH connection)
```

You will see the modal window with a bash terminal. It is your instance that you just created. Install docker here and clone the repository with your project. If you have a private repository you have to create SSH keys and clone it with SSH. Follow the steps from this [link](https://help.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
​
ok, now install docker here, clone repository with your project, build and run the image, like in the previous [steps](#dockerfile)
This all! You can find the public DNS address in the description of your instance and if you done all right you will find your project at this address!
​

## ECS (The second way)

​
Going to ECR and create a new repository. For this in the search input enter
​

```bash
elastic container registry
```

​
and click it.

Click
​

```bash
create repository
```

​
and follow instructions.
​
For the next step, you have to have a project with Dockerfile. Going to your repository that you just created, click
​

```bash
  view push commands
```

<img style="transform: scale(0.65)" src="https://user-images.githubusercontent.com/22969597/93709561-d0a5d480-fb47-11ea-910b-a817c237a9c0.png">
​
and follow the instructions.
After that, you have to create a cluster, a service, and a task. In the left side panel, you will see the "cluster" button. Click it, on the next screen click "create cluster" and follow the instructions.
​
<img style="transform: scale(0.65)" src="https://user-images.githubusercontent.com/22969597/93709611-31cda800-fb48-11ea-86e7-cee3007f85a7.png">
​
Select "ec2 Linux + networking" on the next step enter the name of the instance and select a type(we discuss it in the previous steps), number of instances, [VPC](#instance), and click "create".
​
## Task
​
The next step is creating the task. As you can see in the left side panel we have a "task definition" button.
​
<img style="transform: scale(0.65)" src="https://user-images.githubusercontent.com/22969597/93709562-d13e6b00-fb47-11ea-8e6b-aa9fc4fb5ac8.png">
​
Click it and click "create a new task definition". In the next step select EC2. Enter the name of the task and click "add container". You will see a modal window. Here you should enter the name of the container, image link(you can copy it in the repository) memory limit, and ports. The first always will be 80, the second, it's port which using your application. If this a web application enter 80 too.
​
<img style="transform: scale(0.65)" src="https://user-images.githubusercontent.com/22969597/93709563-d13e6b00-fb47-11ea-947c-38d4dc8f007b.png">
Click "Add".
​
Finally, we will create a service for running our task with a docker image from our repository that we created in the previous steps. For this go to your cluster, select the "services" tab and click "create"
​
<img style="transform: scale(0.65)" src='https://user-images.githubusercontent.com/22969597/93709564-d1d70180-fb47-11ea-9305-ba45e3c71d14.png'>
​
Here select "EC2", your task from previous [steps](#task), cluster, enter the number of the task and change "Minimum healthy percent" to 0, and "Maximum percent" to 100. Next step you can skip and create a service. This all, you can find your project by the address from the ec2 instance description.
