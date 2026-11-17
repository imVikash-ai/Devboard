FROM dhi.io/golang:1.26.11-alpine3.24-dev AS builder

WORKDIR /app

# Download dependencies first for better layer caching.
COPY go.mod go.sum ./
RUN go mod download

COPY . .
RUN mkdir -p /out \
    && CGO_ENABLED=0 GOOS=linux go build \
       -trimpath -ldflags="-s -w" \
       -o /out/devboard-backend .

FROM dhi.io/golang:1.26.11-alpine3.24

WORKDIR /app
COPY --from=builder --chown=65532:65532 /out/devboard-backend ./devboard-backend

USER 65532:65532
EXPOSE 8080
ENTRYPOINT ["/app/devboard-backend"]
