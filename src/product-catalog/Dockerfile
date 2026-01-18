FROM golang:1.22-alpine AS builder

WORKDIR /usr/src/app/

# Cache optimization
RUN --mount=type=cache,target=/go/pkg/mod \
    --mount=type=cache,target=/root/.cache/go-build \
    mkdir -p /root/.cache/go-build

# Copy go modules
COPY go.mod go.sum ./
RUN go mod download

# Copy source
COPY . .

# Build binary
RUN go build -o product-catalog .

####################################

FROM alpine:latest AS release

WORKDIR /usr/src/app/

# Copy binary only
COPY --from=builder /usr/src/app/product-catalog .

ENV PRODUCT_CATALOG_PORT=8088
EXPOSE 8088

ENTRYPOINT ["./product-catalog"]
