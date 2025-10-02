FROM golang:1.24-bullseye AS builder

ARG COMMIT_SHA
WORKDIR /build/shared

COPY .shared/go.mod .shared/go.sum ./
RUN go mod download

COPY .shared/pkg pkg

WORKDIR /build/orchestrator

COPY go.mod go.sum ./
RUN go mod download

COPY main.go Makefile ./
COPY internal internal
COPY .git .git

RUN go mod tidy

RUN --mount=type=cache,target=/root/.cache/go-build make build-local COMMIT_SHA=${COMMIT_SHA}

FROM scratch

COPY --from=builder /build/orchestrator/bin/orchestrator .
