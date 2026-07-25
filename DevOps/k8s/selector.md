In Kubernetes, **Selectors** are used to identify and filter objects (Pods, Deployments, Services, etc.) based on labels.

There are **2 types of Label Selectors**:

## 1. Equality-Based Selector

Uses:

* `=`
* `==`
* `!=`

### Example Labels

```yaml
metadata:
  labels:
    app: nginx
    env: prod
```

### Select Pods

```bash
kubectl get pods -l app=nginx
```

or

```bash
kubectl get pods -l app==nginx
```

### Not Equal

```bash
kubectl get pods -l env!=prod
```

This selects all pods whose `env` label is not `prod`.

### Service Example

```yaml
selector:
  app: nginx
```

This is an equality-based selector because Kubernetes internally matches:

```text
app = nginx
```

---

## 2. Set-Based Selector

Uses:

* `in`
* `notin`
* `exists`
* `!` (does not exist)

Useful when you want to match multiple values.

### Example Labels

```yaml
metadata:
  labels:
    app: nginx
    env: prod
```

### IN Operator

```bash
kubectl get pods -l 'env in (prod,dev)'
```

Matches:

```text
env=prod
env=dev
```

### NOT IN Operator

```bash
kubectl get pods -l 'env notin (test)'
```

Matches all pods except those with:

```text
env=test
```

### EXISTS Operator

```bash
kubectl get pods -l app
```

Matches all pods having the label:

```text
app=anything
```

### DOES NOT EXIST

```bash
kubectl get pods -l '!app'
```

Matches all pods that do not have an `app` label.

---

## Comparison

| Equality-Based        | Set-Based               |
| --------------------- | ----------------------- |
| `app=nginx`           | `app in (nginx,tomcat)` |
| `env!=prod`           | `env notin (prod)`      |
| Single value matching | Multiple value matching |
| Simpler               | More flexible           |

---

## Deployment Example (Set-Based)

```yaml
selector:
  matchExpressions:
    - key: env
      operator: In
      values:
        - prod
        - dev
```

This selects Pods where:

```text
env=prod OR env=dev
```

---
## Summery 

**Equality-based selectors** use `=`, `==`, and `!=` to match labels with a single value.

**Set-based selectors** use `in`, `notin`, `exists`, and `does not exist` operators to match one or more label values, providing more flexibility than equality-based selectors.

Example:

```bash
kubectl get pods -l app=nginx
```

(Equality-based)

```bash
kubectl get pods -l 'app in (nginx,tomcat)'
```

(Set-based)
