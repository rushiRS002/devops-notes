# Topic: POD + SERVICES
## Kubernetes Networking: Intra-Pod and Inter-Pod Communication

Kubernetes networking is fundamental for ensuring smooth communication between various components, including pods, services, and external clients. It provides flexible networking configurations for intra-pod and inter-pod communication.

### Intra-Pod Communication
- **Definition**: Intra-pod communication refers to the communication between containers within the same pod.
- **Mechanism**: Containers in a pod share the same network namespace, which means they:
  - Share the same IP address.
  - Can communicate directly using `localhost` and exposed container ports.

### Inter-Pod Communication
- **Definition**: Inter-pod communication refers to the communication between pods.
- **Mechanism**:
  - Kubernetes assigns each pod a unique IP address.
  - Pods communicate directly using these IP addresses or via Kubernetes services.
  - Kubernetes ensures a flat network model where all pods can communicate without NAT.
---

## ** IMPERATIVE APPROACH (Command Based)**

- You directly give commands to Kubernetes.
- Fast for testing & practice.

 **Not recommended for production.**

---

###  **Check Nodes**

```bash
kubectl get nodes
```

- Shows all present nodes.

---

### **Create Pod Using Command**

```bash
kubectl run pod-1 --image=nginx
```

- Creates a pod named **pod-1** with nginx.

---

###  **List Pods**

```bash
kubectl get pods
```

- Shows all running pods.

---

### ** 1 Expose Pod To Access Application(Create Service Using Command)**



Types of Services:
1. ClusterIP
2. NodePort
3. LoadBalancer


## **ClusterIP (Internal Access Only)**

```bash
kubectl expose pod pod-1 --port=80 --target-port=80 --type=ClusterIP
```
```
kubectl get svc
```
````
kubectl exec -it  podnname  -- curl clusterIP
````
---

## **NodePort (Access from Browser using Node IP)**

```bash
kubectl expose pod pod-1 --port=80 --target-port=80 --type=NodePort
```
```
kubectl get svc
```

Access:

```text
http://<Node-IP>:<NodePort>
```

---

## **LoadBalancer (Public IP – Cloud Only)**

```bash
kubectl expose pod pod-1 --port=80 --target-port=80 --type=LoadBalancer
```
```
kubectl get svc
```

Access:

```text
http://<LoadBalancer-Link>
```

---


##  ** 2 DECLARATIVE APPROACH (YAML File Based)**

- You write everything in a YAML file.

- Kubernetes creates resources exactly as defined.

- **Best for production, GitHub & DevOps pipelines.**


---

### **Create Pod Using YAML**

`pod.yaml`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-1
  labels:
    app: nginx
spec:
  containers:
  - name: cont-1
    image: nginx
    ports:
    - containerPort: 80
```

**Apply the file**

```bash
kubectl apply -f pod.yaml
```

---
