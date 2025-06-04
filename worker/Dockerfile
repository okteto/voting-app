FROM golang:1.22-alpine AS dev

RUN apk add --no-cache git bash curl

COPY bashrc /root/.bashrc

WORKDIR /app

RUN go install github.com/air-verse/air@v1.52.3 && \
    go install github.com/go-delve/delve/cmd/dlv@v1.22.1 && \
    go install golang.org/x/tools/gopls@v0.15.3

COPY . .

RUN go mod tidy && go mod download

RUN CGO_ENABLED=0 GOOS=linux go build -v -o /usr/local/bin/worker main.go

FROM alpine:3.19

RUN apk add --no-cache ca-certificates

COPY --from=dev /usr/local/bin/worker /usr/local/bin/worker

ENTRYPOINT ["/usr/local/bin/worker"]
