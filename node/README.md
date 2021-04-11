# node

[![dockeri.co](https://dockeri.co/image/snowdreamtech/node)](https://hub.docker.com/r/snowdreamtech/node)


[![GitHub issues](https://img.shields.io/github/issues/snowdreamtech/dockerfiles.svg "GitHub issues")](https://github.com/snowdreamtech/dockerfiles) [![GitHub stars](https://img.shields.io/github/stars/snowdreamtech/dockerfiles.svg "GitHub stars")](https://github.com/snowdreamtech/dockerfiles)


## Tags

* 15.14.0-alpine3.13, 15.14-alpine3.13, 15-alpine3.13, alpine3.13, current-alpine3.13

## How to use this image

### Create a `Dockerfile` in your Node.js app project

```dockerfile
# specify the node base image with your desired version node:<version>
FROM node:10
# replace this with your application's default port
EXPOSE 8888
```

You can then build and run the Docker image:

```console
$ docker build -t my-nodejs-app .
$ docker run -it --rm --name my-running-app my-nodejs-app
```

If you prefer Docker Compose:

```yml
version: "2"
services:
  node:
    image: "node:8"
    user: "node"
    working_dir: /home/node/app
    environment:
      - NODE_ENV=production
    volumes:
      - ./:/home/node/app
    expose:
      - "8081"
    command: "npm start"
```

You can then run using Docker Compose:

```console
$ docker-compose up -d
```

Docker Compose example copies your current directory (including node_modules) to the container.
It assumes that your application has a file named [`package.json`](https://docs.npmjs.com/files/package.json)
defining [start script](https://docs.npmjs.com/misc/scripts#default-values).

### Best Practices

We have assembled a [Best Practices Guide](./docs/BestPractices.md) for those using these images on a daily basis.

### Run a single Node.js script

For many simple, single file projects, you may find it inconvenient to write a
complete `Dockerfile`. In such cases, you can run a Node.js script by using the
Node.js Docker image directly:

```console
$ docker run -it --rm --name my-running-script -v "$PWD":/usr/src/app -w /usr/src/app node:8 node your-daemon-or-script.js
```

### Verbosity

Prior to 8.7.0 and 6.11.4 the docker images overrode the default npm log
level from `warn` to `info`. However due to improvements to npm and new Docker
patterns (e.g. multi-stage builds) the working group reached a [consensus](https://github.com/nodejs/docker-node/issues/528)
to revert the log level to npm defaults. If you need more verbose output, please
use one of the following methods to change the verbosity level.

#### Dockerfile

If you create your own `Dockerfile` which inherits from the `node` image you can
simply use `ENV` to override `NPM_CONFIG_LOGLEVEL`.

```dockerfile
FROM node
ENV NPM_CONFIG_LOGLEVEL info
...
```

#### Docker Run

If you run the node image using `docker run` you can use the `-e` flag to
override `NPM_CONFIG_LOGLEVEL`.

```console
$ docker run -e NPM_CONFIG_LOGLEVEL=info node ...
```

#### NPM run

If you are running npm commands you can use `--loglevel` to control the
verbosity of the output.

```console
$ docker run node npm --loglevel=warn ...
```