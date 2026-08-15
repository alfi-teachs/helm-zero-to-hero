# Lab 03 - Install Helm Chart

## Objective

Understand how to install a Helm chart

Search for available Helm charts

Install an application using Helm

Verify the Helm release

Verify Kubernetes resources

Uninstall the Helm release

Clean up the Helm repository

---

### Step 1 - Check Kubernetes Cluster

```bash
kubectl get nodes
```

Expected Output:

```text
NAME       STATUS   ROLES           AGE   VERSION
minikube   Ready    control-plane   XXd   v1.xx.x
```

---

### Step 2 - Add Bitnami Helm Repository

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
```

Expected Output:

```text
"bitnami" has been added to your repositories
```

---

### Step 3 - Verify Helm Repository

```bash
helm repo list
```

Expected Output:

```text
NAME       URL
bitnami    https://charts.bitnami.com/bitnami
```

---

### Step 4 - Update Helm Repository

```bash
helm repo update
```

Expected Output:

```text
Update Complete.
```

---

### Step 5 - Search for NGINX Helm Chart

```bash
helm search repo nginx
```

Expected Output will contain:

```text
bitnami/nginx
```

---

### Step 6 - View NGINX Chart Information

```bash
helm show chart bitnami/nginx
```

This displays information such as:

- Chart name
- Chart version
- Application version
- Description

---

### Step 7 - View NGINX Chart Values

```bash
helm show values bitnami/nginx
```

This displays the default configuration values available for the chart.

---

### Step 8 - Install NGINX Helm Chart

```bash
helm install my-nginx bitnami/nginx
```

Expected Output:

```text
NAME: my-nginx
STATUS: deployed
```

---

### Step 9 - Check Helm Release

```bash
helm list
```

Expected Output will contain:

```text
NAME       NAMESPACE   STATUS
my-nginx   default     deployed
```

---

### Step 10 - Check Helm Release Status

```bash
helm status my-nginx
```

This displays the current status and information about the `my-nginx` release.

---

### Step 11 - Check Kubernetes Pods

```bash
kubectl get pods
```

Expected Output will show the NGINX pod:

```text
NAME                         READY   STATUS    RESTARTS   AGE
my-nginx-xxxxxxxxxx-xxxxx    1/1     Running   0          XXs
```

---

### Step 12 - Check Kubernetes Services

```bash
kubectl get svc
```

Expected Output will contain the NGINX service:

```text
NAME       TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)
my-nginx   ClusterIP   xxx.xxx.xxx.x   <none>        80/TCP
```

---

### Step 13 - Check All Kubernetes Resources

```bash
kubectl get all
```

This shows the Pods, Services, Deployments, ReplicaSets, and other resources created by the Helm chart.

---

### Step 14 - Check Helm Release Again

```bash
helm list
```

---

### Step 15 - Check Release History

```bash
helm history my-nginx
```

This shows the revision history of the Helm release.

---

### Step 16 - Uninstall NGINX Helm Release

```bash
helm uninstall my-nginx
```

Expected Output:

```text
release "my-nginx" uninstalled
```

---

### Step 17 - Verify Helm Release Was Removed

```bash
helm list
```

The `my-nginx` release should no longer appear.

---

### Step 18 - Verify Kubernetes Resources Were Removed

```bash
kubectl get pods
```

```bash
kubectl get svc
```

```bash
kubectl get all
```

The resources created by the `my-nginx` Helm release should be removed.

---

### Step 19 - Remove Bitnami Repository

```bash
helm repo remove bitnami
```

Expected Output:

```text
"bitnami" has been removed from your repositories
```

---

### Step 20 - Verify Repository Cleanup

```bash
helm repo list
```

The Bitnami repository should no longer appear.

---

## What You Learned

Helm charts allow us to install and manage Kubernetes applications using a single Helm command.

In this lab you learned how to:

- Add a Helm repository
- List Helm repositories
- Update Helm repositories
- Search for Helm charts
- View chart information
- View chart values
- Install a Helm chart
- Check a Helm release
- Check Helm release status
- Check Kubernetes resources
- Check Helm release history
- Uninstall a Helm release
- Remove a Helm repository

---

## Lab 03 Complete

Next Lab:

**Lab 04 - Helm Chart Structure**
