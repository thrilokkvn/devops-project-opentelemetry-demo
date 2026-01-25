FROM golang:1.24-alpine AS builder

RUN apk add --no-cache git ca-certificates

WORKDIR /usr/src/app

COPY go.mod go.mod
COPY go.sum go.sum

RUN go mod download

COPY . .

RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o /usr/src/app/product-catalog .

#####################

FROM gcr.io/distroless/base-debian12 AS release

WORKDIR /usr/src/app

COPY ./products ./products

COPY --from=builder /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/
COPY --from=builder /usr/src/app/product-catalog .

ENV PRODUCT_CATALOG_PORT=8088

USER nonroot:nonroot

ENTRYPOINT ["/usr/src/app/product-catalog"]
