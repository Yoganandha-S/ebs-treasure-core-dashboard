FROM golang:1.24-alpine AS builder
RUN apk add --no-cache git
WORKDIR /app
COPY . .

# Initialize with a generic module name
RUN go mod init ebs-dashboard || true
RUN go get k8s.io/client-go@v0.28.2
RUN go get k8s.io/apimachinery@v0.28.2
RUN go mod tidy

# Build the binary with a generic name
RUN go build -o ebs-app .

FROM alpine:latest
RUN apk add --no-cache ca-certificates
WORKDIR /root/

# Copy the binary and frontend from the builder stage
COPY --from=builder /app/ebs-app .
COPY --from=builder /app/frontend ./frontend

EXPOSE 8080

# Execute the generic binary
CMD ["./ebs-app"]
