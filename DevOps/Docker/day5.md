# Day 5 – Docker Volumes & Persistent Storage
- https://docs.docker.com/engine/storage/volumes/#named-and-anonymous-volumes

## Why We Need Volumes

- Containers are **ephemeral**:
  - If you delete a container, its internal filesystem changes are lost.
  - Updating images or redeploying may recreate containers from scratch.
- Many applications (databases, stateful services) need **persistent data** that survives container deletion.

**Solution**: Docker **volumes** and **bind mounts** provide persistent storage outside the container’s writable layer.

---

## Types of Storage in Docker

1. **Container’s writable layer**
   - Temporary; deleted with the container.

2. **Volumes (named volumes)**
   - Managed by Docker (`docker volume` commands).
   - Stored in Docker’s storage area.
   - Best for production use.

3. **Bind mounts**
   - Host directory or file mounted into container.
   - Path comes from host filesystem (e.g., `/data/app` or `$(pwd)/data`).
   - Useful for development, sharing source code, or mounting config files.

This day focuses on Docker **volumes** and using `-v` with `docker run`.

---

## Docker Volume Commands

### List Volumes

```bash
docker volume ls
```

- Shows all volumes managed by Docker.

### Create a Volume

```bash
docker volume create mydata
```

- Creates a named volume `mydata`.

### Inspect a Volume

```bash
docker volume inspect mydata
```

- Shows mountpoint on host and containers using it.

### Remove a Volume

```bash
docker volume rm mydata
```

- Volume must not be in use by any container.

### Remove Unused Volumes

```bash
docker volume prune -a
```

- Deletes all volumes not used by at least one container.
- Use carefully; confirm the prompt.

---

## Using Volumes with `docker run -v`

### Named Volumes

Syntax: `-v volume_name:container_path`

Example – MySQL with named volume:

```bash
docker volume create mysqldata

docker run -d --name db \
  -e MYSQL_ROOT_PASSWORD=root \
  -v mysqldata:/var/lib/mysql \
  mysql:8
```
- standard
```bash
docker run --mount type=volume,src=<volume-name>,dst=<mount-path>
docker run --volume <volume-name>:<mount-path>
```


- MySQL data is stored in volume `mysqldata`.
- You can remove/recreate the `db` container without losing database data.

### Bind Mounts

Syntax: `-v host_path:container_path`

Example – mount current directory into Nginx web root:

```bash
docker run -d --name web \
  -p 8080:80 \
  -v $(pwd)/html:/usr/share/nginx/html \
  nginx
```

- Files in `./html` on the host are served by Nginx.
- Changing files on host updates content in the container immediately.

> Use absolute paths or `$(pwd)` (Linux/macOS) for predictable behavior.

---

## Best Practices

- Use **named volumes** for application data that must persist across container recreations.
- Use **bind mounts** for:
  - Local development (mounting source code).
  - Sharing configuration and log directories.

Examples:

- App logs:

```bash
docker run -d --name app \
  -v app-logs:/var/log/myapp \
  myusername/myapp:1.0
```

- Developer source code mount:

```bash
docker run -it --name app-dev \
  -p 3000:3000 \
  -v $(pwd):/app \
  -w /app \
  node:18-alpine \
  npm start
```

---

## Summary of Key Commands (Day 5)

- Volume management:
  - `docker volume ls`
  - `docker volume create <name>`
  - `docker volume inspect <name>`
  - `docker volume rm <name>`
  - `docker volume prune -a`

- Use with containers:
  - Named volume: `docker run -v mydata:/container/path ...`
  - Bind mount: `docker run -v /host/path:/container/path ...`

Practice:
- Create a named volume.
- Run a database container using that volume.
- Stop/remove the container and verify data persists.
- Try a bind mount to serve static HTML content via Nginx.
