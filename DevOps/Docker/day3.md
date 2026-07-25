# Day 3 – Docker Images & Registries

## Introduction to Docker Images

- **Image**: a read-only template with instructions for creating a container.
- Contains:
  - Base OS layer (e.g., Alpine, Ubuntu).
  - Application files.
  - Runtime dependencies.
  - Configuration (default command, environment variables, etc.).
- Built from **layers**; each Dockerfile instruction typically creates a new layer.

### Image Naming Convention

Format: `registry/namespace/repository:tag`

Examples:
- `nginx` → defaults to `docker.io/library/nginx:latest`.
- `nginx:1.25` → specific tagged version.
- `myusername/myapp:1.0.0`.
- `123456789012.dkr.ecr.us-east-1.amazonaws.com/myapp:latest` (ECR example).

If tag is omitted, `:latest` is assumed by default.

---

## Docker Hub and Amazon ECR

### Docker Hub

- Default public registry used by Docker.
- Contains:
  - Official images (maintained by Docker or vendors) like `nginx`, `mysql`, `redis`.
  - Community images (user-contributed).
- Accessed implicitly when you run commands like `docker pull nginx`.

### Amazon ECR (Elastic Container Registry)

- Private, secure, and scalable container registry by AWS.
- Integrated with:
  - IAM for authentication and authorization.
  - ECS, EKS, and other AWS services.
- Typical flow:
  1. Authenticate Docker client with ECR (using AWS CLI).
  2. Tag local image with ECR repository URL.
  3. Push image to ECR.
  4. Use image in ECS/EKS deployments.

Conceptually, both Docker Hub and ECR are registries: remote stores for images.

---

## Basic Image Management Commands

### List Local Images

```bash
docker images
```

- Shows repository, tag, image ID, size, and creation time.

### Pull an Image

```bash
docker pull nginx:latest
```

- Downloads the image from the default registry (Docker Hub) or specified registry.

### Remove Images

Remove specific image by repo:tag or ID:

```bash
docker image rm nginx:latest
# or
docker rmi nginx:latest
```

Remove dangling/unused images:

```bash
docker image prune
```

Remove all unused images (no containers use them):

```bash
docker image prune -a
```

> Use `-a` with care; it can remove images you still need.

---

## Login, Pull, and Push

### Login to a Registry

```bash
docker login
```

- Prompts for username and password for Docker Hub (or other configured registry).
- For other registries (e.g., ECR), you may pass the registry URL:

```bash
docker login myregistry.example.com
```

### Pull (Download) an Image

```bash
docker pull myusername/myapp:1.0
```

- Downloads the specified image and tag.

### Tag an Image

Tagging gives an image a new name (and/or points to new registry):

```bash
# Local image `myapp:latest` → Docker Hub repo `myusername/myapp:1.0`
docker tag myapp:latest myusername/myapp:1.0
```

### Push (Upload) an Image

```bash
docker push myusername/myapp:1.0
```
![](./images/file1.png)

![](./images/file1.png)


- Uploads the image layers to the remote registry under that name.

---

## Creating Images from Containers – `docker commit`

- `docker commit` creates a new image from a **container’s current filesystem state**.
- Useful for quick experiments or debugging, but **not** ideal for production/reproducible builds.

Example:

```bash
# Suppose you modified a running container manually
# Save it as a new image

docker commit mycontainer myimage:debug
```

- Now `myimage:debug` can be used to run new containers with those changes.

Best practice: use **Dockerfile** to define images, and use `commit` only for temporary or ad‑hoc work.

---

## Saving and Loading Images – `save` and `load`

### Save Image to a Tar File

```bash
docker save -o myimage.tar myusername/myapp:1.0
```

- Exports one or more images (plus their layers) to a `.tar` file.
- Useful for offline transfer or backups.

### Load Image from a Tar File

```bash
docker load -i myimage.tar
```

- Imports images from the tar archive into the local Docker image store.

---

## Cleaning Up Images – `prune`

- Over time, many unused images accumulate and consume disk space.

### Remove Dangling Images

```bash
docker image prune
```

- Removes images that are not tagged and not referenced by any container.

### Remove All Unused Images

```bash
docker image prune -a
```

- Removes images not used by **any** container (running or stopped).
- Always review the prompt before confirming.

---

## Summary of Key Commands (Day 3)

- Registries: Docker Hub (public), ECR (AWS private) – both store images.
- List images: `docker images`.
- Download and upload: `docker pull`, `docker push`, `docker login`.
- Manage images: `docker image rm` / `docker rmi`, `docker image prune`.
- Create from containers: `docker commit`.
- Export/import: `docker save`, `docker load`.

Practice:
- Pull an image, run a container, make a small change, `commit` it.
- Tag and push it to a Docker Hub repo (or a test registry).
- Save and load the image using `save`/`load`.
