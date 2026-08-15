# Lab 05 - Helm Values

## Objective

Understand how `values.yaml` works

Understand how values are passed to Helm templates

View default chart values

Override Helm values during installation

Override Helm values using a custom values file

Verify the changed values

---

### Step 1 - Create a Helm Chart

```bash
helm create mychart
```

---

### Step 2 - Enter the Chart Directory

```bash
cd mychart
```

---

### Step 3 - Check the Default Values

```bash
cat values.yaml
```

The `values.yaml` file contains the default configuration for the Helm chart.

---

### Step 4 - View Helm Chart Values

```bash
helm show values .
```

This displays the default values available in the chart.

---

### Step 5 - Check the Default Replica Count

```bash
grep "replicaCount" values.yaml
```

Expected Output:

```text
replicaCount: 1
```

---

### Step 6 - Check the Default Image Configuration

```bash
grep -A5 "^image:" values.yaml
```

You should see values similar to:

```text
image:
  repository: nginx
  pullPolicy: IfNotPresent
  tag: ""
```

---

### Step 7 - Render the Chart Using Default Values

```bash
helm template myapp .
```

This renders the Kubernetes YAML using the default values from `values.yaml`.

---

### Step 8 - Override Replica Count From the Command Line

```bash
helm template myapp . --set replicaCount=3
```

The Deployment should now contain:

```text
replicas: 3
```

---

### Step 9 - Check the Rendered Replica Count

```bash
helm template myapp . --set replicaCount=3 | grep "replicas:"
```

Expected Output:

```text
replicas: 3
```

---

### Step 10 - Override Image Repository

```bash
helm template myapp . --set image.repository=httpd
```

---

### Step 11 - Override Multiple Values

```bash
helm template myapp . --set replicaCount=3 --set image.repository=httpd
```

---

### Step 12 - Create a Custom Values File

```bash
touch custom-values.yaml
```

---

### Step 13 - Add Custom Values

```bash
cat > custom-values.yaml <<'EOF'
replicaCount: 3

image:
  repository: nginx
  pullPolicy: IfNotPresent
  tag: "latest"
EOF
```

---

### Step 14 - Check Custom Values File

```bash
cat custom-values.yaml
```

Expected Output:

```yaml
replicaCount: 3

image:
  repository: nginx
  pullPolicy: IfNotPresent
  tag: "latest"
```

---

### Step 15 - Render Chart Using Custom Values

```bash
helm template myapp . -f custom-values.yaml
```

---

### Step 16 - Verify Replica Count

```bash
helm template myapp . -f custom-values.yaml | grep "replicas:"
```

Expected Output:

```text
replicas: 3
```

---

### Step 17 - Verify Image

```bash
helm template myapp . -f custom-values.yaml | grep "image:"
```

---

### Step 18 - Install the Chart Using Custom Values

```bash
helm install myapp . -f custom-values.yaml
```

---

### Step 19 - Check Helm Release

```bash
helm list
```

---

### Step 20 - Check Kubernetes Deployment

```bash
kubectl get deployment
```

Expected Output should show:

```text
myapp-mychart
```

---

### Step 21 - Check Pods

```bash
kubectl get pods
```

You should have three pods because `replicaCount` was set to `3`.

---

### Step 22 - Check Helm Values Used by the Release

```bash
helm get values myapp
```

Expected Output:

```yaml
USER-SUPPLIED VALUES:
replicaCount: 3
image:
  repository: nginx
  pullPolicy: IfNotPresent
  tag: latest
```

---

### Step 23 - Check All Values

```bash
helm get values myapp --all
```

This displays both user-supplied and default values.

---

### Step 24 - Check Helm Release Status

```bash
helm status myapp
```

---

### Step 25 - Uninstall the Helm Release

```bash
helm uninstall myapp
```

Expected Output:

```text
release "myapp" uninstalled
```

---

### Step 26 - Verify Cleanup

```bash
helm list
```

```bash
kubectl get pods
```

---

### Step 27 - Return to Parent Directory

```bash
cd ..
```

---

### Step 28 - Remove the Practice Chart

```bash
rm -rf mychart
```

---

### Step 29 - Verify Cleanup

```bash
ls
```

The `mychart` directory should no longer exist.

---

## What You Learned

`values.yaml` stores the default configuration for a Helm chart.

You learned how to:

- View default Helm values
- Use values inside Helm templates
- Override values using `--set`
- Override values using a custom values file
- Render a chart with custom values
- Install a chart using custom values
- View values used by a Helm release
- Check all Helm values
- Uninstall the Helm release

## Important Commands

```bash
helm show values .
```

```bash
helm template myapp . --set replicaCount=3
```

```bash
helm template myapp . -f custom-values.yaml
```

```bash
helm install myapp . -f custom-values.yaml
```

```bash
helm get values myapp
```

```bash
helm get values myapp --all
```

## Lab 05 Complete

Next Lab:

**Lab 06 - Helm Templates**
