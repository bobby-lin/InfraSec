## Docker Compose
https://docs.docker.com/compose/compose-file/

Service names are very similar to like container names.

### Purpose:
1. Configure relationship between containers
2. Save our Docker container run settings in easy-to-read file
3. Create one-liner developer env startups

### Tools
1. YAML file to describe solution options for containers, networks and volumes
2. Usually for local dev/test automation: `docker-compose` cli tool

### Example:
```
version: '2'

# same as 
# docker run -p 80:4000 -v $(pwd):/site bretfisher/jekyll-serve

services:
  jekyll:
    image: bretfisher/jekyll-serve
volumes:
  - .:/site

ports:
  - '80:4000'
```

### Commands
The cli wil be useful for setting up dev environment and then stopping it when not required. Basically, the docker-compose cli is talking to Docker's api.

Start
```
docker-compose up
```
End
```
docker-compose down
```
Remove volume as well
```
docker-compose down -v
```
### Assignment: Set up Drupal and Postgres with docker-compose

```
version: '3'

services:
  database:
    image: postgres
    container_name: psql
    environment: 
      POSTGRES_PASSWORD: '{$password}'
  cms:
    image: drupal
    ports:
      - '8080:80'
    depends_on: 
      - database
    volumes:
      - drupal-modules:/var/www/html/modules
      - drupal-profiles:/var/www/html/profiles
      - drupal-sites:/var/www/html/sites
      - drupal-themes:/var/www/html/themes

volumes:
  drupal-modules:
  drupal-profiles:
  drupal-sites:
  drupal-themes:
```

Once done, run docker-compose down and remove volume as well:
```
docker-compose down -v
```

### Tips
##### Which is the minimum recommended version for a docker-compose.yml file?
Version 2 and above provide significantly more features then the old default version 1, and what we will be using as a default base for this course. Bonus Note: v2.x is actually better for local docker-compose use, and v3.x is better for use in server clusters (Swarm and Kubernetes)

##### Which of the following does "docker-compose up" automatically create, even if not manually specified in the compose file?
The bridge network is created at runtime by default so the containers can communicate with one another across it.

##### What is the 'build context' in a compose file supposed to do?
The "." indicates the dockerfile exists in the same directory as the compose file.

##### The "ports:" key in a compose file does the same thing as the EXPOSE stanza in a Dockerfile.
The "ports:" key publishes the particular service on whatever port you specify, and is the docker run equivalent to the -p flag. EXPOSE will only expose particular vpn port.

##### Usernames for a database, when placed in a compose file of a specific service, would best fit placed under which of the following keys?
Usernames are typically an environment-specific setting, that can change often, so they are often set in environment variables.

##### Where does compose derive a DNS name from?
DNS names (set as aliases) for containers in a compose file come from the service name declared in the .yml