# ConfigMap and Secrets
- https://kubernetes.io/docs/concepts/configuration/configmap/
- https://kubernetes.io/docs/concepts/configuration/secret/

## 1. Introduction

### What is a ConfigMap?
A **ConfigMap** is a Kubernetes resource used to store non-confidential data as key-value pairs. It helps decouple configuration data from application code.

### What is a Secret?
A **Secret** is a Kubernetes resource designed to store confidential data, such as passwords or API keys, in a secure and encoded format. Secrets are encoded using Base64, providing a layer of obfuscation but not encryption.

### Why Use ConfigMap and Secret?
- **Separation of Concerns:** Decouples configuration from application logic.
- **Ease of Updates:** Configuration changes do not require rebuilding or redeploying applications.
- **Security:** Secrets ensure sensitive data is handled securely.

---

## 2. Practical Steps

### Step 1: Create the ConfigMap

#### Manifest File
Save the following content in a file named `config.yaml`:

```yaml
apiVersion: v1 
kind: ConfigMap
metadata:
    name: my-cred
data: 
  PASSWORD: "redhat123"
  USERNAME: "admin"
  CITY: "pune"
```

#### Apply the ConfigMap
Run the following command to create the ConfigMap in your cluster:

```bash
kubectl apply -f config.yaml
```

#### Verify the ConfigMap
View the created ConfigMap:

```bash
kubectl get configmap my-cred
```
#### To describe  the configmap
```bash
kubectl describe configmap my-config
```
### Step 2: Create the Secret
**to create encrypted values**
```sh
echo -n "redhat123" | base64
```
#### Manifest File
Save the following content in a file named `secret.yaml`:

```yaml
apiVersion: v1 
kind: Secret
metadata:
    name: my-secret
data: 
  PASSWORD: "cmVkaGF0MTIzCg=="
  USERNAME: "YWRtaW4K"
  CITY: "cHVuZQo="
```

#### Apply the Secret
Run the following command to create the Secret in your cluster:

```bash
kubectl apply -f secret.yaml
```

#### Verify the Secret
To check the Secret:

```bash
kubectl get secret my-secret
```

Note: The values will appear base64-encoded. To decode, use:

```bash
echo "QWRtaW4=" | base64 --decode
```

---

## 3. Using ConfigMap and Secret in a Pod

### Example Pod Manifest

Create a file named `pod-with-config-and-secret.yaml` with the following content:

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: my-pod
spec:
   containers:
      - name: my-c
        image: mysql:latest
        ports:
          - containerPort: 3306
        env:
          - name: MYSQL_ROOT_PASSWORD
            valueFrom:
              secretKeyRef:
                name: my-secret
                key: PASSWORD
  
          - name: USER_NAME
            valueFrom:
              secretKeyRef:
                name: my-secret
                key: USERNAME

          - name: MY_CITY
            valueFrom:
              secretKeyRef:
                name: my-secret
                key: CITY
```

### Apply the Pod Manifest

```bash
kubectl apply -f pod-with-config-and-secret.yaml
```

### Verify the Pod Environment Variables

```bash
kubectl exec pod-with-config-secret -- printenv | grep DB_
```

---

## 4. Notes and Best Practices

# 🔹 ConfigMap vs Secret in Kubernetes

##  Difference Table

| Feature | ConfigMap | Secret |
|--------|----------|--------|
| Purpose | Store non-sensitive data | Store sensitive data |
| Data Type | Plain text | Base64 encoded |
| Use Case | App configs, env variables | Passwords, tokens, API keys |
| Security | Not secure | More secure (can enable encryption) |
| Storage | Stored as plain text in etcd | Stored encoded in etcd |
| Access Control | Basic | Use RBAC for strict control |
| Example | DB host, app config | DB password, API token |

---

##  Key Notes

###  ConfigMap Notes
1. **Non-Confidential Data:** Do not store sensitive data.
2. **Dynamic Updates:** Can update without restarting Pods (if mounted as volume).
3. **Lightweight:** Avoid storing large or complex data.

###  Secret Notes
1. **Secure Handling:** Avoid plain text files; use `kubectl`.
2. **Encryption:** Enable encryption at rest for better security.
3. **Access Control:** Use RBAC to restrict access.

---

## ⚡ Quick Summary

- **ConfigMap → Non-sensitive configuration**
- **Secret → Sensitive/secure data**

---
