
## HPA with Minikube
- https://kubernetes.io/docs/concepts/workloads/autoscaling/horizontal-pod-autoscale/
### make sure your minikube cluster is running
### enable metrics-server
```bash
minikube addons enable metrics-server   
```
### create deployment 
```bash
vim hpa-dep.yaml
```

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hpa-nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: hpa-nginx
  template:
    metadata:
      labels:
        app: hpa-nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        resources:
          requests:
            cpu: "100m"
          limits:
            cpu: "200m"
        ports:
        - containerPort: 80
```
```bash
kubectl apply -f hpa-dep.yaml
```

---
# create service.yaml
```bash
vim service.yaml
```
```yaml
apiVersion: v1
kind: Service
metadata:
  name: hpa-service
spec:
  selector:
    app: hpa-nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: ClusterIP
```
```bash
kubectl apply -f  service.yaml
```

# create hpa.yaml
```bash
vim hpa.yaml
```

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: hpa-nginx
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: hpa-nginx
  minReplicas: 1
  maxReplicas: 5
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 20
```
```bash
kubectl apply -f hpa.yaml
```

# Generate Load
```bash
kubectl run -i --tty load-generator --image=busybox /bin/sh
```
*note:* - run below command in shell

```bash
while true; do wget -q -O- http://hpa-service; done
```

## check
- take an ssh on another terminal to check the cluster pods and hpa 
```bash
kubectl get hpa -w
kubectl get pods
```

![](./images/k8sday8.png)


