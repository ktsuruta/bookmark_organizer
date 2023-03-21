# bookmark_organizer

# How to install it
## requirement
docker

docker-compose

## 1. Move to work directory
```
cd bookmark_organizer
mkdir images
```

## 2. Download / Clone related repositories.
### image of work directory

```
.
..
docker-compose.yaml
bookmark_frontend/
bookmark_api/
preview_api
```


### docker-compose.yaml
```
version: '3'
services:
  web:
    image: httpd:2.4
    ports: 
      - "80:80"
    volumes:
      - ./images:/usr/local/apache2/htdocs
    hostname: web
  db:
    image: mongo:5.0
    ports: 
      - "27017:27017"
    hostname: db-server
    tty: true
  frontend:
    build: ./bookmark_frontend
    ports:
      - "3000:3000"
    hostname: frontend 
    tty: true
    depends_on:
      - backend
    volumes:
      - ./bookmark_frontend:/code/bookmark_frontend
  backend:
    build: ./bookmark_api
    ports:
      - "5000:5000"
    depends_on:
      - db
    hostname: backend
    volumes:
      - ./bookmark_api:/code/bookmark_api
      - ./images:/images
    tty: true
  preview:
    build: ./preview_api
    ports:
      - "5001:5000"
    hostname: preview
    tty: true
    volumes:
      - ./preview_api:/code/preview_api
      - ./images:/images

  chrome:
    image: selenium/node-chrome:4.5.3-20221024
    shm_size: 2gb
    depends_on:
      - selenium-hub
    environment:
      - SE_EVENT_BUS_HOST=selenium-hub
      - SE_EVENT_BUS_PUBLISH_PORT=4442
      - SE_EVENT_BUS_SUBSCRIBE_PORT=4443

  edge:
    image: selenium/node-edge:4.5.3-20221024
    shm_size: 2gb
    depends_on:
      - selenium-hub
    environment:
      - SE_EVENT_BUS_HOST=selenium-hub
      - SE_EVENT_BUS_PUBLISH_PORT=4442
      - SE_EVENT_BUS_SUBSCRIBE_PORT=4443

  firefox:
    image: selenium/node-firefox:4.5.3-20221024
    shm_size: 2gb
    depends_on:
      - selenium-hub
    environment:
      - SE_EVENT_BUS_HOST=selenium-hub
      - SE_EVENT_BUS_PUBLISH_PORT=4442
      - SE_EVENT_BUS_SUBSCRIBE_PORT=4443

  selenium-hub:
    image: selenium/hub:4.5.3-20221024
    container_name: selenium-hub
    ports:
      - "4442:4442"
      - "4443:4443"
      - "4444:4444"



```

### bookmark_api
```
git clone https://github.com/ktsuruta/bookmark_api.git
```

### bookmark_frontend
```
git clone https://github.com/ktsuruta/bookmark_frontend.git
```

### preview_api
```
git clone https://github.com/ktsuruta/preview_api.git
```


##　3. Create database
```
docker-compose up
```
if bookmark_frontend does not run, enter the docker and run 'npm install'

##　4. Create database
```
> docker container exec -it bookmark_organizer_db_1 bash
> mongo
> use test
> db
-> see that you have "test" database.

```

## 5. Run frontend manually...
``

If frontend does not run, you need to log in frontend container and run `npm install` and `npm start`


