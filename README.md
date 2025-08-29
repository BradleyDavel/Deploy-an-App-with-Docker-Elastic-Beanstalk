<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />
![Docker](https://img.shields.io/badge/Tool-Docker-blue)
![AWS Elastic Beanstalk](https://img.shields.io/badge/AWS-Elastic%20Beanstalk-orange)
![NextWork](https://img.shields.io/badge/Project-NextWork-lightgrey)
![Status](https://img.shields.io/badge/Status-Completed-success)
![License](https://img.shields.io/badge/License-MIT-lightgrey)

# Deploy an App with Docker

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-compute-eb)

**Author:** Bradley Davel  
**Email:** bradley.davel@outlook.com

---

![Image](http://learn.nextwork.org/sparkling_indigo_heroic_bat/uploads/aws-compute-eb_c4df13c84)

---

## Introducing Today's Project!

### What is Docker?

Docker is a platform that lets me package my application and all of its dependencies into a single container image. This makes my app portable, consistent, and easy to run anywhere — whether that’s on my laptop, a server, or in the cloud.

It is useful because it solves the classic “it works on my machine” problem by ensuring the app always runs in the same environment, no matter where it’s deployed. It also makes scaling and deployment much easier.

I used Docker in this project to containerize a simple web app, run it locally for testing, and then deploy it to AWS Elastic Beanstalk so it could be accessed live on the internet.

### One thing I didn't expect...

One thing I didn’t expect in this project was running into port conflicts when starting my container. At first, I tried mapping the container to port 80, but since something else on my machine was already using that port, Docker threw an error.

This forced me to troubleshoot and learn how to remap ports (for example, using 8080:80). It turned out to be a valuable lesson in how container networking works in the real world.

### This project took me...

This project took me approximately 15 minutes to complete.

The most challenging part was… honestly, nothing — it was straightforward and beginner-friendly.

It was most rewarding to see my custom Docker image deployed on AWS Elastic Beanstalk and accessible live on the internet.

This gave me confidence in working with Docker and Elastic Beanstalk, and it showed me how quickly I can take a containerized app from my laptop to the cloud. 🚀

---

## Understanding Containers and Docker

### Containers

Containers are lightweight, portable environments that package an application and all of its dependencies together. This means I can run my app consistently anywhere — on my laptop, on a server, or in the cloud — without worrying about “it works on my machine” issues.

They’re like little shipping boxes 🐳 for software: everything the app needs (code, libraries, runtime) is inside the container, and it runs the same no matter where I deploy it.

A container image is like a blueprint for a container. It’s a read-only template that includes everything my app needs to run — the code, libraries, dependencies, and instructions for execution.

When I run a container, Docker takes that image and creates a live, running instance from it.

-Image = recipe/blueprint 📦
-Container = the running application built from that image 🚀

### Docker

Docker is an open-source platform that lets me package my application and all its dependencies into a single container. That container runs the same way everywhere — on my laptop, on a server, or in the cloud.

Docker Desktop is the tool I installed on my machine to make working with Docker easier. It includes the Docker Engine, Docker CLI, and a simple dashboard so I can build, run, and manage containers without worrying about low-level setup.

The Docker daemon is the background service on my machine that does the heavy lifting for Docker. It’s always running in the background, listening for Docker commands from the CLI or the Docker Desktop UI.

When I tell Docker to build an image, run a container, or pull something from Docker Hub, the daemon is the part that actually makes it happen. In short, the daemon is the engine that powers Docker.

---

## Running an Nginx Image

Nginx (pronounced “engine-x”) is a web server, which is a program you use to run websites and web apps. If you want people to visit your site, you're going to need a web server to deliver your website's files to their browsers!

Engineers pick Nginx when they need to manage lots of visitors at once, making sure your site doesn’t slow down when traffic spikes. Nginx is also the web server of choice for containerized apps because it's lighter and uses less memory, compared to other options like Apache.

The command I ran to start a new container was "docker run -d -p 80:80 nginx"

Docker run starts a new container. We're using a pre-existing container image called nginx and we're starting this container in detached mode (-d) so it runs in the background. Then, this -p 80:80 maps port 80 on your host machine to port 80 in the container, which means you'll be able to access the webpage that Nginx is running through your computer's web browser.

![Image](http://learn.nextwork.org/sparkling_indigo_heroic_bat/uploads/aws-compute-eb_6245f5bb10)

---

## Creating a Custom Image

A Dockerfile is a document with all the instructions for building your Docker image. Docker would read a Dockerfile to understand how to set up your application's environment and which software packages it should install.

FROM nginx:latest → I’m telling Docker to start with the official Nginx base image. This gives me a lightweight web server without needing to install or configure it manually.

COPY index.html /usr/share/nginx/html/ → I copy my own index.html file into Nginx’s default web root folder, replacing the default welcome page. This makes my own webpage the one served by the container.

EXPOSE 80 → I declare that the container will use port 80, the standard HTTP port, so it can receive web traffic.

In short: I’m using Nginx as the web server, adding my own content, and making it accessible through port 80. 🚀

The command I used to build a custom image with my Dockerfile was:

"docker build -t my-web-app ."

-t my-web-app → This tags the image with the name my-web-app so I can easily reference it later when running containers.

. (dot at the end) → This tells Docker to look in the current directory for the Dockerfile and all the files it needs (like index.html).

So with one command, I created a portable container image that packages Nginx and my custom webpage together.

![Image](http://learn.nextwork.org/sparkling_indigo_heroic_bat/uploads/aws-compute-eb_4c741d1913)

---

## Running My Custom Image

There was an error when I ran my custom image because port 80 was already in use on my machine. The Docker daemon tried to bind my container’s port 80 to my computer’s port 80, but another process (or another container) was already using it.

The container image is the blueprint 📦. In my case, it’s the custom image I built with Nginx and my index.html file inside it. It’s read-only and reusable — I can store it, share it, and run it multiple times.

The container is the running instance 🚀 of that image. When I used docker run -p 8080:80 my-web-app, Docker started a live container from the image. That container is what actually served my webpage when I visited http://localhost:8080.

In short:
-Image = recipe
-Container = the finished meal you can eat (run)

![Image](http://learn.nextwork.org/sparkling_indigo_heroic_bat/uploads/aws-compute-eb_74b5c3d619)

---

## Elastic Beanstalk

AWS Elastic Beanstalk is a service that makes it easy to deploy cloud applications without worrying about the underlying infrastructure. You simply upload your code and Elastic Beanstalk handles everything needed to get it running, like setting up servers and managing scaling. This lets you focus on your application code without having to spend too much time on managing cloud infrastructure.

In more technical terms, Elastic Beanstalk handles things like:

-Capacity provisioning
-Load balancing
-Auto-scaling
-Application health monitoring

Deploying my custom image with AWS Elastic Beanstalk took me about 5 or so minutes. The EB CLI handled provisioning the environment, uploading my Dockerfile, and setting up the underlying infrastructure automatically.

This was much faster than manually configuring EC2, load balancers, and security groups — and it gave me a live, working containerized app on the web with just a few commands. 🚀

![Image](http://learn.nextwork.org/sparkling_indigo_heroic_bat/uploads/aws-compute-eb_26d5573b23)

---

---
