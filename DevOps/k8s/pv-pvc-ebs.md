````markdown id="x9v2pq"
# Complete Kubernetes PV & PVC Practical using Existing AWS EBS Volume in EKS

# Objective

In this practical we will learn:

- What is PersistentVolume (PV)
- What is PersistentVolumeClaim (PVC)
- How EKS uses AWS EBS Volume
- How to attach existing EBS volume to Kubernetes
- How pod stores persistent data
- How data remains after pod deletion

This document contains ALL steps from start to end.

---

# Architecture

```text
AWS EBS Volume
        ↓
PersistentVolume (PV)
        ↓
PersistentVolumeClaim (PVC)
        ↓
Pod
````

---

# Environment Used

| Component    | Value         |
| ------------ | ------------- |
| Kubernetes   | EKS           |
| Region       | ap-south-1    |
| Cluster Name | eks-cdec-prod |
| Storage Type | AWS EBS       |
| Access Mode  | ReadWriteOnce |
| Filesystem   | ext4          |

---

# Important Understanding Before Starting

## What is PV?

PersistentVolume represents actual storage.

Example:

```text
AWS EBS Volume
```

---

## What is PVC?

PVC requests storage from PV.

Application uses PVC instead of directly using EBS.

---

## Why ReadWriteOnce?

AWS EBS supports:

| Access Mode         | Supported |
| ------------------- | --------- |
| ReadWriteOnce (RWO) | ✅         |
| ReadWriteMany (RWX) | ❌         |

So for EBS always use:

```yaml
accessModes:
  - ReadWriteOnce
```

---

# STEP 1 — Verify EKS Cluster

Check cluster:

```bash
eksctl get cluster --region ap-south-1
```

Expected:

```text
eks-cdec-prod
```

---

# STEP 2 — Install EBS CSI Driver

EKS needs EBS CSI Driver to communicate with AWS EBS volumes.

---

## 2.1 Create IAM Service Account

Run:

```bash
eksctl create iamserviceaccount \
  --name ebs-csi-controller-sa \
  --namespace kube-system \
  --cluster eks-cdec-prod \
  --region ap-south-1 \
  --attach-policy-arn arn:aws:iam::aws:policy/service-role/AmazonEBSCSIDriverPolicy \
  --approve \
  --override-existing-serviceaccounts
```

---

## 2.2 Install EBS CSI Addon

Run:

```bash
eksctl create addon \
  --name aws-ebs-csi-driver \
  --cluster eks-cdec-prod \
  --region ap-south-1 \
  --force
```

---

## 2.3 Verify CSI Driver

Run:

```bash
kubectl get pods -n kube-system | grep ebs
```

Expected:

```text
ebs-csi-controller   Running
ebs-csi-node         Running
```

If not running:

Check logs:

```bash
kubectl logs -n kube-system deployment/ebs-csi-controller -c ebs-plugin
```

---

# STEP 3 — Create EBS Volume

Go to AWS Console:

```text
EC2 → Elastic Block Store → Volumes
```

Create Volume:

| Setting | Value       |
| ------- | ----------- |
| Type    | gp3         |
| Size    | 20 GB       |
| AZ      | ap-south-1a |

IMPORTANT:

EBS volume and EKS worker node must be in SAME Availability Zone.

---

# STEP 4 — Verify EBS Volume AZ

Run:

```bash
aws ec2 describe-volumes \
--volume-ids vol-xxxxxxxx \
--query "Volumes[*].AvailabilityZone"
```

Expected:

```text
ap-south-1a
```

---

# STEP 5 — Attach EBS Volume to EC2 Temporarily

Purpose:

We need to create filesystem on volume.

Go to AWS Console:

```text
EC2 → Volumes → Attach Volume
```

Attach to temporary EC2 instance.

---

# STEP 6 — Verify Device Name

Login to EC2.

Run:

```bash
lsblk
```

Example:

```text
nvme1n1
```

IMPORTANT:

Device may appear as:

```text
/dev/nvme1n1
```

---

# STEP 7 — Create Filesystem

VERY IMPORTANT STEP.

Correct:

```bash
sudo mkfs.ext4 /dev/nvme1n1
```

Wrong:

```bash
sudo mkfs.ext4 /dev/nvme1n1p1
```

Reason:

Kubernetes mounts RAW disk directly.

If prompt appears:

```text
Proceed anyway? (y,N)
```

Type:

```text
y
```

---

# STEP 8 — Detach Volume from EC2

IMPORTANT.

Volume should NOT remain attached to EC2.

Go to AWS Console:

```text
Volumes → Detach Volume
```

Or use CLI:

```bash
aws ec2 detach-volume --volume-id vol-xxxxxxxx
```

Verify:

```bash
aws ec2 describe-volumes --volume-ids vol-xxxxxxxx
```

Expected:

```text
State = available
```

---

# STEP 9 — Create PV and PVC YAML

Create file:

```bash
vi pv-pvc.yaml
```

Paste:

```yaml
apiVersion: v1
kind: PersistentVolume

metadata:
  name: eks-ebs-pv

spec:
  capacity:
    storage: 10Gi

  volumeMode: Filesystem

  accessModes:
    - ReadWriteOnce

  persistentVolumeReclaimPolicy: Retain

  storageClassName: manual

  csi:
    driver: ebs.csi.aws.com
    volumeHandle: vol-0ef5e3f9ec060dcd0
    fsType: ext4

---
apiVersion: v1
kind: PersistentVolumeClaim

metadata:
  name: eks-ebs-pvc

spec:
  accessModes:
    - ReadWriteOnce

  storageClassName: manual

  resources:
    requests:
      storage: 10Gi
```

---

# STEP 10 — Apply PV and PVC

Run:

```bash
kubectl apply -f pv-pvc.yaml
```

---

# STEP 11 — Verify PV and PVC

Run:

```bash
kubectl get pv
kubectl get pvc
```

Expected:

```text
STATUS = Bound
```

Example:

```text
eks-ebs-pv    Bound
eks-ebs-pvc   Bound
```

---

# STEP 12 — Create Pod YAML

Create file:

```bash
vi pod.yaml
```

Paste:

```yaml
apiVersion: v1
kind: Pod

metadata:
  name: ebs-test-pod

spec:
  containers:
  - name: app
    image: nginx

    volumeMounts:
    - mountPath: /data
      name: ebs-storage

  volumes:
  - name: ebs-storage
    persistentVolumeClaim:
      claimName: eks-ebs-pvc
```

---

# STEP 13 — Create Pod

Run:

```bash
kubectl apply -f pod.yaml
```

---

# STEP 14 — Verify Pod Status

Run:

```bash
kubectl get po
```

Expected:

```text
STATUS = Running
```

If stuck in:

```text
ContainerCreating
```

Check:

```bash
kubectl describe pod ebs-test-pod
```

---

# STEP 15 — Verify Mounted Volume

Enter pod:

```bash
kubectl exec -it ebs-test-pod -- bash
```

Check filesystem:

```bash
df -h
```

You should see mounted storage.

---

# STEP 16 — Write Data

Create file:

```bash
echo "Hello from EBS Volume" > /data/test.txt
```

Verify:

```bash
cat /data/test.txt
```

Expected:

```text
Hello from EBS Volume
```

---

# STEP 17 — Verify Persistence

Exit:

```bash
exit
```

Delete pod:

```bash
kubectl delete pod ebs-test-pod
```

Recreate:

```bash
kubectl apply -f pod.yaml
```

Enter again:

```bash
kubectl exec -it ebs-test-pod -- bash
```

Check file:

```bash
cat /data/test.txt
```

Expected:

```text
Hello from EBS Volume
```

This proves:

* EBS mounted successfully
* Data persistence working
* PV and PVC working correctly

---

# Important Troubleshooting

---

## Problem 1 — Pod stuck in ContainerCreating

Check:

```bash
kubectl describe pod ebs-test-pod
```

---

## Problem 2 — EBS CSI Controller CrashLoopBackOff

Check:

```bash
kubectl get pods -n kube-system | grep ebs
```

Check logs:

```bash
kubectl logs -n kube-system deployment/ebs-csi-controller -c ebs-plugin
```

---

## Problem 3 — UnauthorizedOperation

Reason:

IAM permission missing.

Fix:

Attach policy:

```text
AmazonEBSCSIDriverPolicy
```

---

## Problem 4 — wrong fs type

Cause:

Filesystem created on partition.

Wrong:

```bash
/dev/nvme1n1p1
```

Correct:

```bash
/dev/nvme1n1
```

---

# Cleanup

Delete pod:

```bash
kubectl delete -f pod.yaml
```

Delete PV and PVC:

```bash
kubectl delete -f pv-pvc.yaml
```

Volume will remain because:

```yaml
persistentVolumeReclaimPolicy: Retain
```

---

# Final Understanding

## Flow

```text
AWS EBS Volume
      ↓
PersistentVolume
      ↓
PersistentVolumeClaim
      ↓
Pod
```

---

# Real World Usage

Used in:

* Jenkins
* SonarQube
* MySQL
* PostgreSQL
* Prometheus
* Stateful applications
* File upload applications

---

# Conclusion

In this practical we learned:

* Static provisioning
* PV and PVC concepts
* AWS EBS integration with EKS
* Persistent storage
* Data persistence
* EBS CSI Driver setup
* Troubleshooting storage issues

```
```
