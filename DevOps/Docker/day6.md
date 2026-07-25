# Day 6 – Dockerfile Basics

## What Is a Dockerfile?

- A **Dockerfile** is a text file with a set of instructions to build a Docker image.
- `docker build` reads the Dockerfile and produces an image layer by layer.
- Each instruction usually creates a new layer, which can be cached and reused.

General build flow:
1. Start from a **base image** (`FROM`).
2. Install dependencies (`RUN`).
3. Copy application code (`COPY` / `ADD`).
4. Configure environment (`ENV`, `WORKDIR`, `EXPOSE`, `USER`).
5. Define the startup command (`CMD` / `ENTRYPOINT`).

---

## Key Dockerfile Instructions

### 1. `FROM` – Base Image

- Mandatory first instruction in most Dockerfiles.
- Sets the base layer for your image.

Examples:

```dockerfile
FROM ubuntu:22.04
```

```dockerfile
FROM node:18-alpine
```

### 2. `LABEL` – Metadata

- Attaches metadata to the image (e.g., maintainer, version, description).

```dockerfile
LABEL maintainer="you@example.com" \
      version="1.0" \
      description="Sample app image"
```

### 3. `RUN` – Execute Commands at Build Time

- Runs commands in a new layer on top of the current image and commits the result.

```dockerfile
RUN apt-get update && apt-get install -y curl
```

- Often used for installing packages and preparing the environment.
- Prefer combining related commands with `&&` to reduce the number of layers.

### 4. `CMD` – Default Container Command

- Specifies the default command to run when a container starts.
- Can be overridden by providing a command in `docker run`.
- Only **one** `CMD` is used (last one in Dockerfile if multiple are defined).

**Exec form (recommended):**

```dockerfile
CMD ["nginx", "-g", "daemon off;"]
```

**Shell form:**

```dockerfile
CMD nginx -g 'daemon off;'
```

### 5. `ENTRYPOINT` – Main Executable

- Defines the main process of the container.
- Typically **not** overridden; instead, arguments are appended.

Example:

```dockerfile
ENTRYPOINT ["python", "app.py"]
```

With `docker run myimage --debug`, `--debug` becomes an argument to `app.py`.

You can combine `ENTRYPOINT` and `CMD`:

```dockerfile
ENTRYPOINT ["python", "app.py"]
CMD ["--port", "8000"]
```

- `ENTRYPOINT` defines the executable.
- `CMD` provides default arguments.

### 6. `ENV` – Environment Variables

- Sets environment variables in the image.

```dockerfile
ENV APP_ENV=production \
    APP_DEBUG=false
```

- Available to processes inside the container at runtime.

### 7. `ARG` – Build-Time Arguments

- Variables that exist only during the **build** stage.
- Can be passed with `--build-arg` during `docker build`.

```dockerfile
ARG APP_VERSION=1.0.0
RUN echo "Building version $APP_VERSION"
```

Build with:

```bash
docker build --build-arg APP_VERSION=2.0.0 -t myapp:2.0 .
```

### 8. `COPY` – Copy Files into Image

- Copies files/directories from build context (local directory) into the image.

```dockerfile
COPY package*.json ./
COPY src/ /app/src/
```

- Used for adding application code, configuration files, etc.

### 9. `ADD` – Copy + Extra Features

- Similar to `COPY`, but also:
  - Can extract local tar archives.
  - Can download files from URLs.

Example:

```dockerfile
ADD config.tar.gz /etc/myapp/
```

> Best practice: use `COPY` for simple file copies and `ADD` only when you need its extra features.

### 10. `EXPOSE` – Document Ports

- Informs Docker (and humans) which ports the container listens on.
- Does **not** publish ports to the host; use `-p` in `docker run` for that.

```dockerfile
EXPOSE 80
EXPOSE 8080
```

### 11. `USER` – Set User

- Sets the user (and optionally group) used for subsequent instructions and when the container runs.

```dockerfile
USER appuser
```

- Running as non-root is a security best practice for production.

### 12. `WORKDIR` – Working Directory

- Sets the working directory for subsequent instructions (`RUN`, `CMD`, `ENTRYPOINT`, `COPY`, `ADD`).

```dockerfile
WORKDIR /app
```

- If the directory doesn’t exist, it is created.

---

## Sample Dockerfile Putting It All Together

```dockerfile
FROM node:18-alpine

LABEL maintainer="you@example.com" \
      version="1.0" \
      description="Simple Node.js app"

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

ENV NODE_ENV=production

EXPOSE 3000

CMD ["npm", "start"]
```

- Starts from `node:18-alpine`.
- Sets metadata.
- Uses `/app` as working directory.
- Installs dependencies and copies source code.
- Sets environment variable and documents port 3000.
- Default command: `npm start`.

---

## Summary of Key Concepts (Day 6)

- Dockerfile describes how to build an image.
- Core instructions:
  - Base and metadata: `FROM`, `LABEL`.
  - Build steps: `RUN`, `COPY`, `ADD`.
  - Configuration: `ENV`, `ARG`, `EXPOSE`, `WORKDIR`, `USER`.
  - Startup behavior: `CMD`, `ENTRYPOINT`.

Practice:
- Write a Dockerfile for a simple web app (Node.js, Python, or any language).
- Use `FROM`, `WORKDIR`, `COPY`, `RUN`, `ENV`, `EXPOSE`, `CMD`.
