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

#### Deploy with Docker

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

By default it runs the command: `gosu nonroot browsepy 0.0.0.0 5050 --directory /files`, but it can be overwritten.

#### Deploy with Kubernetes

Example of YAML files for deploying to kubernetes:

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    role: file-browser
  name: browsepy
  namespace: browser
spec:
  selector:
    matchLabels:
      app: browsepy
  template:
    metadata:
      labels:
        app: browsepy
        role: file-browser
    spec:
      containers:
        - image: 'blankdots/docker-browsepy'
          imagePullPolicy: Always
          command: ["browsepy", "0.0.0.0", "5050", "--directory", "/files"]
          name: browsepy
          ports:
            - containerPort: 5050
              name: browsepy
              protocol: TCP
          volumeMounts:
            - mountPath: /files
              name: data
      volumes:
        - name: data
          # persistentVolumeClaim:
          #  claimName: browsepy-data
          hostPath:
            path: /local/path
---
apiVersion: v1
kind: Service
metadata:
  name: browsepy
  labels:
    app: browsepy
  namespace:browser
spec:
  type: NodePort
  ports:
  - port: 5050
    targetPort: 5050
    protocol: TCP
    name: web
  selector:
    app: browsepy

```


### How to access the files:

Go to `http://hostname:5050`
