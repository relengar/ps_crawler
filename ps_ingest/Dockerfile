FROM golang:1.22 as build

WORKDIR /usr/src/app

COPY go.mod go.sum ./
RUN go mod download && go mod verify

COPY . .
RUN go build -v -o ./ ./...

# FROM alpine:latest as deploy # throws error, might be just if running on windows
FROM golang:1.22 as deploy

WORKDIR /usr/src/app

RUN adduser ingest
# RUN adduser -D ingest # use with alpine

COPY --from=build --chown=crawler --chmod=777 /usr/src/app/ps_ingest ./

USER ingest

CMD ["./ps_ingest"]