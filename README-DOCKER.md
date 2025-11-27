# Docker Setup for minimal-springboot

This document explains how to build and run the minimal-springboot application using Docker.

## Prerequisites

- Docker Desktop installed and running on your system
- Java application already built (or let Docker build it)

## Files Created

1. **Dockerfile** - Multi-stage build configuration
   - Stage 1: Builds the application using Maven
   - Stage 2: Creates a lightweight runtime image with just the JRE

2. **.dockerignore** - Excludes unnecessary files from Docker build context

3. **docker-compose.yml** - Simplified Docker Compose configuration

## Building and Running

### Option 1: Using Docker Commands

#### Build the Docker image:
```bash
docker build -t minimal-springboot:latest .
```

#### Run the container:
```bash
docker run -d -p 8080:8080 --name minimal-springboot minimal-springboot:latest
```

#### Stop the container:
```bash
docker stop minimal-springboot
```

#### Remove the container:
```bash
docker rm minimal-springboot
```

### Option 2: Using Docker Compose (Recommended)

#### Start the application:
```bash
docker compose up --build
```

#### Start in detached mode (background):
```bash
docker compose up -d --build
```

#### Stop the application:
```bash
docker compose down
```

#### View logs:
```bash
docker compose logs -f
```

## Accessing the Application

Once the container is running, you can access the application at:

- **Base URL**: http://localhost:8080
- **Hello Endpoint**: http://localhost:8080/hello

## Troubleshooting

### Check if Docker is running:
```bash
docker --version
docker ps
```

### View container logs:
```bash
docker logs minimal-springboot
```

### Check if port 8080 is available:
```bash
netstat -an | findstr :8080
```

### Rebuild without cache:
```bash
docker build --no-cache -t minimal-springboot:latest .
```

## Image Size Optimization

The multi-stage build ensures:
- Build dependencies (Maven, full JDK) are not included in the final image
- Only the JRE and application JAR are in the runtime image
- Alpine Linux base for minimal size

