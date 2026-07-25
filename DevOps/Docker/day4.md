# Day 4 – Docker Networking
- https://docs.docker.com/engine/network/

## Introduction to Docker Networking

- Docker provides virtual networking so containers can communicate:
  - With each other.
  - With the host.
  - With external networks (e.g., the internet).
- Each container typically gets its own virtual network interface and IP address.
- Networks are created and managed by Docker, and containers attach to them.

Use this to see existing networks:

```bash
docker network ls
```

---

## Default Networks

On a typical Docker host, you’ll see networks like:

- `bridge` – default network for containers (on Linux).
- `host` – uses the host’s network stack directly.
- `none` – disables networking for containers attached to it.

![d2](./images/docker2.png)



These are created automatically when Docker is installed.

---

## Network Drivers

A **network driver** defines how the underlying Docker network behaves.

### 1. Bridge (Default)

- Default driver when you run `docker run` without specifying `--network`.
- Docker creates a virtual bridge (e.g., `docker0` on Linux).
- Containers on the same bridge network can communicate using internal IPs or container names.
- To expose container ports to the host, use `-p` in `docker run`.

Example:

```bash
docker run -d --name web1 nginx
```

- Container attaches to default `bridge` network.
- Internally accessible to other containers on `bridge`.

### 2. Host

- Container shares the host’s network namespace.
- No separate container IP or port mapping; container uses host’s IP and ports directly.

Example:

```bash
docker run --network host nginx
```

- On Linux, Nginx will bind directly to the host’s network interfaces.
- No `-p` mapping is needed (and is ignored) with `--network host`.

### 3. None

- Disables networking for the container (no external network access).

Example:

```bash
docker run --network none busybox:latest
```

- Container has only a loopback interface.
- Useful for highly isolated workloads.

> Other drivers (beyond this day’s scope) include `overlay` for multi-host networking (e.g., Docker Swarm, Kubernetes).

---

## Managing Networks with `docker network` Commands

### List Networks

```bash
docker network ls
```

- Shows all networks, their driver, and scope.

### Inspect a Network

```bash
docker network inspect bridge
```

- Shows detailed information, including:
  - Subnet and gateway.
  - Containers attached.
  - IP address assignments.

---

## Creating and Deleting User-Defined Bridge Networks

### Create a Network

```bash
docker network create mynet
```

```bash
docker network create --subnet=192.168.0.0/24 my_custom_network
```
```
multipal cont 
cont1--> cont2 ---> cont3 
frontend --> backend--> database
```
- Creates a user-defined bridge network named `mynet`.
- Benefits over default `bridge`:
  - Automatic DNS-based service discovery by container name.
  - Better isolation and control.

### Run Containers on a Specific Network

```bash
docker run -d --name web1 --network mynet nginx

docker run -d --name web2 --network mynet nginx
```

- `web1` and `web2` can reach each other by name (e.g., `curl web2` from `web1`).

### Connect/Disconnect Containers from Networks

Connect an existing container to a network:

```bash
docker network connect mynet web1
```

Disconnect it:

```bash
docker network disconnect mynet web1
```

### Delete a Network

```bash
docker network rm mynet
```

- Network must have no containers attached (stop/remove or disconnect them first).

---

## Using Networks with `docker run --network`

### Example 1 – Application and Database on Same Network

```bash
# Create a dedicated app network
docker network create appnet

# Start database on appnet
docker run -d --name db --network appnet -e MYSQL_ROOT_PASSWORD=root mysql:8

# Start app container on appnet
docker run -d --name app --network appnet -p 8080:8080 myusername/myapp:1.0
```

- Inside `app`, you can use `db` as the hostname for the database.
- External access to the app via host port `8080`.

### Example 2 – Testing `host` Network

```bash
# Linux-only typical usage

docker run --network host --name webhost nginx
```

- Nginx container uses host network directly.
- Access using host’s IP on port 80.
![](./images/docker3.png)
---

## Summary of Key Commands (Day 4)

- List and inspect networks: `docker network ls`, `docker network inspect`.
- Create/delete networks: `docker network create`, `docker network rm`.
- Attach containers to networks: `docker run --network`, `docker network connect`, `docker network disconnect`.
- Drivers:
  - `bridge` – default, isolated container network on one host.
  - `host` – share host network.
  - `none` – disable networking.

Practice:
- Create a user-defined network.
- Run two containers on that network and test communication by name.
- Connect and disconnect containers from different networks.
