FROM golang:1.22 AS build

ARG GIT_BRANCH

WORKDIR /src
RUN git clone -b "${GIT_BRANCH:-develop}" --depth=1 https://github.com/prysmaticlabs/prysm ./
RUN go mod download

WORKDIR /src/cmd/beacon-chain
RUN go install

WORKDIR /src/cmd/validator
RUN go install

FROM ubuntu
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y ca-certificates && rm -rf /var/lib/apt/lists/*

COPY --from=build /go/bin/validator /go/bin/beacon-chain /usr/local/bin
