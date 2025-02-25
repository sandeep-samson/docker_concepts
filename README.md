# Review of Docker Concepts and Understanding a Dockerfile

## Overview:
- **Docker Concepts**
- **Understanding a Dockerfile**

## Docker Conceptes

### Overview
Docker simplifies the process of creating, deploying, and managing applications by using containers. Containers allow you to package an application with its dependencies into a standardized unit for software development. Docker provides tools and a platform to build, ship, and run containers across various environments.

### Dockerfile
A text file that contains instructions for building a Docker image. It specifies the base image, sets the working directory, installs dependencies, copies application code, exposes ports, and defines commands to run application.

### Containers
An instance of a Docker image that runs as a process on the host machine. Containers are lightweight, portable, and isolated, making them ideal for deploying and scaling application.

### Docker Image Storage
Docker image are stored in registries, which can be public or private. Public registries like Docker Hub host millions of images, while organizations often uses private registries for security and control.

### Where Docker Images Exist
**Local Machine:** When you build a Docker image, it's initially stored locally on your machine. You can list local Docker images using the `Docker images` command.

**Registry:** After building, you can push Docker images to a registry, making them accessible from anywhere with access to that registry.

## Dockerfile
Below is a simple Dockerfile for reference. The subsequent explanation details the commands used, providing a comprehensive understanding of how to write a Dockerfile.

**Dockerfile**
```docker
# Use the official Node.js image as the base image
FROM node:14
    
# Set environment variables
ENV NODE_ENV=production
ENV PORT=3000

# Set the woking directory
WORKDIR /app

# Copy package.json and package-lock.json files
COPY package*.json ./

# Install dependencies
RUN npm install --production

# Copy the rest of the application code
COPY . .

# Add additional file
ADD public/index.html /app/public/index.html

# Specify the default command to run when the container starts
CMD ["node", "app.js"]

# Labelling the image
LABEL version="1.0"
LEBEL description="Node.js application Docker image"
LABEL maintainer="Your name"

# Healthcheck to ensure the conatiner is running correctly
HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retires=3 CMD curl -fs http://localhost:$PORT || exit 1

# Set a non-root user for security purposes
USER node
```
## Understanding the Dockerfile

1. **Specify the Base Image**
to bigin specify the base image using the `FROM` instruction. In this case, the official Node.js image version 14 is used.

```docker
FROM node:14
```

`FROM`: Specifies the base image for the Docker container.

`node:14`: Pulls the official Node.js image with version 14 from Docker registry.

2. **Set Environment Variables**
Next, define the necessary environment variables with the `ENV` instruction. Here, `NODE_ENV` is set to production and `PORT` is set to 3000.

```docker
ENV NODE_ENV=production
ENV PORT=3000
```
`ENV`: Sets environment variables inside the Docker containers.

`NODE_ENV=production`: Sets the Node.js environment to production.

`PORT=3000`: Sets the port on which the Node.js application will listen.

3. **Set the Working Directory**
Then, specify the working directory inside the container using the `WORKDIR` insruction. The sets `/app` as the working directory.
```docker
WORKDIR /app
```
`WORKDIR`: Defines the working directory for subsequent instruction, simplifying the path references.

`/app`: The directory inside the container to store your application code, keeping it organized and easily accessible.

4. **Copy Package Files**
After defining the working directory, use the `COPY` instrcution to copy the `package.json` and `package-lock.json` files into the container. These files are essential for installing the application dependencies.
```docker
COPY package*.json ./
```
`COPY`: Copies fies from your local machine to the container.

`package*.json ./`: Copies both 'package.json' and `package-lock.json` files.

5. **Install Dependencies**
With the package files copied, execute the `RUN` instruction to install the production dependencies listed in `package.json`.
```docker
RUN npm install --production
```
`RUN`: Executes commands in the container during the build process.

`npm install --production`: Installsd ependencies listed in `package.json` without devDependencies.

6. **Copy Application Code**
Once the dependencies are installed, copy the rest of the application code into the conatainer using the `COPY` instruction.
```docker
COPY . .
```
`COPY . .`: Copies all files and directories from the current directory on the local host to the current directory in the Docker container.

7. **Add Additional File(s)**
