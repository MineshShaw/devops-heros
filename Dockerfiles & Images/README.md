# Docker Multi-Stage Build & Deployment Assignment

**Name:** Minesh Shaw  
**Enrollment Number:** 24BCS10029

## Task 1 & 2: Multi-Stage Dockerfile Execution

The repository containing the multi-stage Dockerfile was successfully cloned, built, and executed. The multi-stage build optimized the final image size by discarding build dependencies and only transferring the compiled binary/artifacts to the final production image.

**Build and Run Commands Used:**
```bash
docker build -t multi-stage-hello .
docker run -d -p 8080:8080 --name multi-stage-container multi-stage-hello
```

## Verification 1: Application Output
The application successfully serves the required text on port 8080.

![Application Running]([Insert Screenshot of 'curl http://localhost:8080' or browser showing "Hello World from Docker multi-stage build"])

## Verification 2: Docker PS
The container is running correctly with the port mapping established (Host 8080 -> Container 8080).

![Docker PS Output]([Insert Screenshot of 'docker ps' showing the container on port 8080])

## Task 3: Docker Application Deployments
To demonstrate the versatility of Docker, three distinct technology stacks were deployed in isolated containers.

### 1. Node.js Deployment
A lightweight Alpine Node.js image running an HTTP server.

Command: docker run -d --name node-app -p 3000:3000 node:alpine sh -c "npm install -g http-server && echo 'Node.js App Running' > index.html && http-server -p 3000"

#### Proof of Execution:
![Node App Output]([Insert Screenshot of 'docker ps' showing node-app OR browser at localhost:3000])

### 2. Python Deployment
An Alpine Python image utilizing the built-in http.server module.

Command: docker run -d --name python-app -p 8000:8000 python:alpine sh -c "echo 'Python App Running' > index.html && python -m http.server 8000"

#### Proof of Execution:
![Python App Output]([Insert Screenshot of 'docker ps' showing python-app OR browser at localhost:8000])

### 3. Java Deployment
A Tomcat 9 server running on the Java Runtime Environment (JRE 17).

Command: docker run -d --name java-tomcat-app -p 8081:8080 tomcat:9.0-jre17

#### Proof of Execution:
![Java Tomcat Output]([Insert Screenshot of 'docker ps' showing java-tomcat-app OR browser at localhost:8081 showing Tomcat default page])