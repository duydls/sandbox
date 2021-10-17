FROM golang:alpine
WORKDIR /go/src/app

COPY goweb.go .

RUN go build goweb.go

EXPOSE 8080

CMD ["./goweb"]
