FROM golang:1.22-bullseye as dev

COPY bashrc /root/.bashrc

WORKDIR /app

RUN go install github.com/codegangsta/gin@latest && \
    go install github.com/go-delve/delve/cmd/dlv@v1.23.1 && \
    go install golang.org/x/tools/gopls@v0.16.1 && \
    curl -sSfL https://raw.githubusercontent.com/cosmtrek/air/master/install.sh | sh -s -- -b /usr/bin

ADD go.mod go.sum ./
RUN go mod download all

ADD . .
RUN go get

RUN --mount=type=cache,target=/root/.cache/go-build CGO_ENABLED=0 GOOS=linux go build -v -o /usr/local/bin/worker main.go

FROM scratch

COPY --from=dev /usr/local/bin/worker /usr/local/bin/worker

ENTRYPOINT ["/usr/local/bin/worker"]
