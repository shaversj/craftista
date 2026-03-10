FROM golang:1.20-alpine AS builder

WORKDIR /app

COPY go.mod go.sum ./
RUN go mod download

COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o recommendation .

FROM alpine:3.19

WORKDIR /app

COPY --from=builder /app/recommendation .
COPY --from=builder /app/config.json .
COPY --from=builder /app/static ./static
COPY --from=builder /app/templates ./templates
COPY --from=builder /app/data ./data

EXPOSE 8080

CMD ["./recommendation"]
