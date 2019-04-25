FROM golang:1.10.2-alpine3.7

RUN apk update; apk upgrade

RUN apk add git

RUN go get -u github.com/golang/dep/cmd/dep

WORKDIR /go/src/app

VOLUME ["/go/src/app"]

#to build: docker build -t go .
#to run : docker run -it go /bin/sh
#in the new terminal make a go file. Add the script:

#package main
#import "fmt"
#func main() {
#fmt.Println("Hello World!")
#}

# and run command:
# go run <filename>.go
