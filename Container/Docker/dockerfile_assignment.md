## Explanation of Dockerfile
To create a dockerfile, you will need to understand the dependencies of the application including: 
- OS version
- OS files required
- Application dependencies
- Location of application files to copy
- How to start the application
- Which Port for app to listen?

####Dockerfile example for a Nodejs app
```
# Base image: OS version
FROM node:6-alpine

# Get OS dependencies
RUN apk add --update tini;

# Set the directory of the app
WORKDIR /usr/src/app

# Download App dependencies
COPY package*.json ./

# Install the dependencies. We run npm cache clean on the same line to optimize on image space
RUN npm install && npm cache clean --force

# Copy the app files
COPY . .

# Command to start the application
CMD ["/sbin/tini", "--", "node", "./bin/www"]

# Port to open
EXPOSE 3000
```
#####Building the image
```
docker image build --tag demo_node:latest .
```

We want to expose the app at port 80 in host when starting the container
```
docker run --rm -p 80:3000 demo_node:latest
```