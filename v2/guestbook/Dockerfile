# IBM DevOps and Software Engineering - Introduction to Containers w/ Docker, Kubernetes & OpenShift - Lab: Build and Deploy a Simple Guestbook App
# https://github.com/SnowY4you
# Stage 1: Builder Stage
FROM golang:1.18 AS builder

# Set the working directory
WORKDIR /app

# Copy the application source code (main.go) and any other source files
COPY . . 

# Go Modules to allow 'go get'
RUN go mod init guestbook

# The 'go get' commands are necessary to download and install the dependencies
RUN go get github.com/codegangsta/negroni
RUN go get github.com/gorilla/mux github.com/xyproto/simpleredis

# The 'go mod tidy' step ensures dependencies are recorded correctly
RUN go mod tidy

# Build the executable (binary) from the main.go file
RUN CGO_ENABLED=0 go build -o guestbook main.go

# --- Stage 2: Final (Production) Stage ---
FROM alpine:latest

# Install necessary runtime dependencies (e.g., ca-certificates for HTTPS)
# RUN apk --no-cache add ca-certificates

# Set the working directory for the final image
WORKDIR /app

# Copy the compiled binary from the builder stage
COPY --from=builder /app/guestbook /app/guestbook

ADD public/index.html /app/public/index.html
ADD public/script.js /app/public/script.js
ADD public/style.css /app/public/style.css
ADD public/jquery.min.js /app/public/jquery.min.js

# Set the application entry point
CMD ["./guestbook"]

# Expose the port the Go application listens on
EXPOSE 3000
