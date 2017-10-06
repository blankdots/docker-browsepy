## Docker browsepy

Makes use of https://github.com/ergoithz/browsepy to browse files from a specific directory.
Uses https://github.com/tianon/gosu runs a `browsepy` as a non-root user.

`docker pull blankdots/docker-browsepy`

### How to Build

Simple way of building the container locally:
```shell
docker build -t blankdots/docker-browsepy
```

### How to Run

* Run with a local folder attached (recommended):
```shell
docker run -p 5050:5050 -v /local/folder:/files blankdots/docker-browsepy
```

* Run as part of docker compose:
```yml
version: '2'
services:

  browser:
    image: blankdots/docker-browsepy
    volumes:
      - /local/folder:/files
    ports:
      - '5050:5050'
```

### How to access the files:

Go to `http://hostname:5050`
