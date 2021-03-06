# golang

[![dockeri.co](https://dockeri.co/image/snowdreamtech/golang)](https://hub.docker.com/r/snowdreamtech/golang)


[![GitHub issues](https://img.shields.io/github/issues/snowdreamtech/dockerfiles.svg "GitHub issues")](https://github.com/snowdreamtech/dockerfiles) [![GitHub stars](https://img.shields.io/github/stars/snowdreamtech/dockerfiles.svg "GitHub stars")](https://github.com/snowdreamtech/dockerfiles)

## Tags

* latest, alpine, 1, 1.16, 1.16.3, 1.16.3-alpine3.13

##  How to use this image

Note: `/go` is world-writable to allow flexibility in the user which runs the container (for example, in a container started with `--user 1000:1000`, running `go get github.com/example/...` will succeed). While the `777` directory would be insecure on a regular host setup, there are not typically other processes or users inside the container, so this is equivilant to `700` for Docker usage, but allowing for `--user` flexibility.

### Start a Go instance in your app

The most straightforward way to use this image is to use a Go container as both the build and runtime environment. In your Dockerfile, writing something along the lines of the following will compile and run your project:

```bash
FROM golang:1.14

WORKDIR /go/src/app
COPY . .

RUN go get -d -v ./...
RUN go install -v ./...

CMD ["app"]
```

You can then build and run the Docker image:

```bash
$ docker build -t my-golang-app .
$ docker run -it --rm --name my-running-app my-golang-app
```

### Compile your app inside the Docker container

There may be occasions where it is not appropriate to run your app inside a container. To compile, but not run your app inside the Docker instance, you can write something like:

```bash
$ docker run --rm -v "$PWD":/usr/src/myapp -w /usr/src/myapp golang:1.14 go build -v
```

This will add your current directory as a volume to the container, set the working directory to the volume, and run the command go build which will tell go to compile the project in the working directory and output the executable to myapp. Alternatively, if you have a Makefile, you can run the make command inside your container.

```bash
$ docker run --rm -v "$PWD":/usr/src/myapp -w /usr/src/myapp golang:1.14 make
```

### Cross-compile your app inside the Docker container

If you need to compile your application for a platform other than linux/amd64 (such as windows/386):

```bash
$ docker run --rm -v "$PWD":/usr/src/myapp -w /usr/src/myapp -e GOOS=windows -e GOARCH=386 golang:1.14 go build -v
```

Alternatively, you can build for multiple platforms at once:

```bash
$ docker run --rm -it -v "$PWD":/usr/src/myapp -w /usr/src/myapp golang:1.14 bash
$ for GOOS in darwin linux; do
>   for GOARCH in 386 amd64; do
>     export GOOS GOARCH
>     go build -v -o myapp-$GOOS-$GOARCH
>   done
> done
```