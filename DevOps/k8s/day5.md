## 🚀 Deployment Strategies in Kubernetes

🔗 Reference: [https://medium.com/@ezekiel.umesi/understanding-deployment-strategies-in-kubernetes-3e447d060280](https://medium.com/@ezekiel.umesi/understanding-deployment-strategies-in-kubernetes-3e447d060280)

---

### 1. Recreate Strategy

```
All existing pods are terminated first, then new pods are created.

- Simple but causes downtime ❌
- Application is unavailable during update
- Suitable for non-critical apps
```

---

### 2. Rolling Update Strategy (Default ✅)

```
Pods are updated one by one without downtime.

- New pods are created gradually
- Old pods are terminated step by step
- Ensures zero downtime ✔️

Key Config:
maxSurge: 1        → 1 extra pod can be created
maxUnavailable: 1  → 1 pod can be down

👉 Flow: +1 new pod → -1 old pod → repeat
```

---

### 3. Blue/Green Deployment

```
Two environments: Blue (current) and Green (new)

- Deploy new version in parallel (Green)
- Test it before switching traffic
- Switch traffic using Service

✔️ No downtime
✔️ Easy rollback
❌ Requires more resources
```

---

### 4. Canary Deployment

```
New version is released to a small set of users first.

- Gradual rollout
- Monitor before full release
- Reduces risk in production

✔️ Safe deployment
✔️ Real user testing
```

---

##  Example Deployment (Rolling Update)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 6
  selector:
    matchLabels:
      app: my-app
  minReadySeconds: 5
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 1
      maxSurge: 1
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-pod-game
        image: mukunddeo9325/super-mario
        ports:
        - containerPort: 80
```

---

##  Useful Commands

```bash
kubectl get pods                          # Check pods
kubectl apply -f deployment.yaml          # do changes and see difference 
kubectl get pods -w                       # to watch 
kubectl describe deployment my-app        # Detailed info - 
kubectl rollout status deployment/my-app  # Rollout status -
kubectl rollout history deployment/my-app # Version history -
kubectl rollout undo deployment/my-app    # Rollback
```
```
kubectl set image deployment/my-app my-container=nginx:latest    # Change image without editing YAML
kubectl scale deployment my-app --replicas=5                     # Scale deployment
```
---

 **Quick Summary:**

* Recreate → downtime
* Rolling → zero downtime (default)
* Blue/Green → switch traffic
* Canary → gradual release

---


## StatefulSet

StatefulSet is used to manage **stateful applications** in Kubernetes.

- Provides **unique identity** to each pod  
- Maintains **stable hostname and storage**  
- Pods are created and deleted **in order (sequentially)**  

---

### Key Features

- **Stable Pod Names**
  → pod-0, pod-1, pod-2 (fixed identity)

- **Persistent Storage**
  → Each pod gets its own PersistentVolume  
  → Data is not lost after restart  

- **Ordered Deployment**
  → Pods start one by one (0 → N)  
  → Termination happens in reverse order  

- **Stable Network Identity**
  → Each pod has a fixed DNS name  

---

### Use Cases

- Databases (MySQL, MongoDB)  
- Distributed systems (Kafka, Zookeeper)  
- Applications requiring persistent data  

---

| Feature              | Deployment                        | StatefulSet                           |
| -------------------- | --------------------------------- | ------------------------------------- |
| **Application Type** | Stateless apps                    | Stateful apps                         |
| **Pod Identity**     | Interchangeable                   | Unique identity (fixed)               |
| **Pod Naming**       | Random names                      | Fixed names (`pod-0`, `pod-1`)        |
| **Storage**          | No persistent storage (ephemeral) | Persistent storage (PVC for each pod) |
| **Scaling**          | Fast and unordered                | Ordered scaling (0 → n)               |
| **Pod Creation**     | Parallel                          | Sequential                            |
| **Pod Deletion**     | Random                            | Reverse order                         |
| **Data Handling**    | Data can be lost                  | Data is preserved                     |
| **Use Cases**        | Web apps, APIs                    | Databases, Kafka, Zookeeper           |

---

### Example: StatefulSet

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: my-stateful-app
spec:
  serviceName: "my-service"
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: nginx
        volumeMounts:
        - name: data
          mountPath: /data

  volumeClaimTemplates:
  - metadata:
      name: data
    spec:
      accessModes: ["ReadWriteOnce"]
      resources:
        requests:
          storage: 1Gi
```

## Deamonset
- A Kubernetes DaemonSet ensures a specific pod runs on all (or a subset of) nodes in a cluster. As nodes are added or removed, the DaemonSet automatically adds or removes the required pods, making it ideal for background tasks like logging agents, monitoring, and network plugins.

```deamonset.yaml
apiVersion: apps/v1
kind:  DaemonSet
metadata: 
  name: daemon
  labels:
    app: depapp
spec:
    selector: 
      matchLabels: 
          app: depapp
    template:
      metadata:
        labels:
          name: cwatch
          app: depapp
      spec:
        containers:
          - name: cwatch
            image: amazon/cloudwatch-agent:latest
            ports:
                - containerPort: 80
                  protocol: TCP
```










