# bookmark_organizer

# How to install it
## requirement
docker

docker-compose

## 1. Move to work directory
```
cd hogehoge
```

## 2. Download / Clone related repositories.
### image of work directory

```
.
..
docker-compose.yaml
bookmark_frontend/
bookmark_api/
```


### docker-compose.yaml
```
version: '3'
services:
  db:
    image: mongo
    ports:
      - "27017:27017"
    tty: true
  frontend:
    build: ./bookmark_frontend
    ports:
      - "3000:3000"
    tty: true
    depends_on:
      - backend
  backend:
    build: ./bookmark_api
    ports:
      - "5000:5000"
    depends_on:
      - db
    tty: true
```

### bookmark_api
```
git clone https://github.com/ktsuruta/bookmark_api.git
```

### bookmark_frontend
```
git clone https://github.com/ktsuruta/bookmark_frontend.git
```

##　3. Create database
```
> docker container exec -it bookmark_organizer_db_1 bash
> mongo
> use test
> dbs
-> see that you have "test" database.

```

##　4. Create database
```
docker-compose up
```
