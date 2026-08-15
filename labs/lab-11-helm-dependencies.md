# Lab 11 - Helm Dependencies

## Objective

Understand Helm chart dependencies

Add a dependency to a Helm chart

Update Helm dependencies

Build Helm dependencies

View dependency information

Understand the `charts` directory

---

### Step 1 - Create a Helm Chart

```bash
helm create mychart
```

### Step 2 - Enter the Chart Directory

```bash
cd mychart
```

### Step 3 - Check the Chart Structure

```bash
ls
```

Expected Output:

```text
Chart.yaml
charts
templates
values.yaml
```

### Step 4 - Check Current Dependencies

```bash
helm dependency list
```

Expected Output:

```text
NAME    VERSION    REPOSITORY    STATUS
```

The list may be empty because no dependencies have been added yet.

### Step 5 - Add a Dependency to Chart.yaml

```bash
cat >> Chart.yaml <<'EOF'

dependencies:
  - name: nginx
    version: "*"
    repository: "https://charts.bitnami.com/bitnami"
EOF
```

### Step 6 - Check Chart.yaml

```bash
cat Chart.yaml
```

You should see the dependency section:

```yaml
dependencies:
  - name: nginx
    version: "*"
    repository: "https://charts.bitnami.com/bitnami"
```

### Step 7 - Add the Bitnami Repository

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
```

### Step 8 - Update the Helm Repository

```bash
helm repo update
```

### Step 9 - Update Chart Dependencies

```bash
helm dependency update
```

Expected Output will show that the `nginx` dependency was downloaded.

### Step 10 - Check the Charts Directory

```bash
ls charts
```

You should see an NGINX chart package similar to:

```text
nginx-*.tgz
```

### Step 11 - Check Dependency Information

```bash
helm dependency list
```

Expected Output should contain:

```text
NAME    VERSION    REPOSITORY                              STATUS
nginx   ...        https://charts.bitnami.com/bitnami     ok
```

### Step 12 - Build Chart Dependencies

```bash
helm dependency build
```

Expected Output will indicate that the dependencies were built successfully.

### Step 13 - Check the Charts Directory Again

```bash
ls -lh charts
```

### Step 14 - Check the Chart

```bash
helm lint .
```

Expected Output:

```text
1 chart(s) linted, 0 chart(s) failed
```

### Step 15 - Render the Chart

```bash
helm template myapp .
```

This renders the parent chart together with its dependency.

### Step 16 - Install the Chart

```bash
helm install myapp .
```

### Step 17 - Check the Helm Release

```bash
helm list
```

### Step 18 - Check Kubernetes Resources

```bash
kubectl get all
```

### Step 19 - Check the Pods

```bash
kubectl get pods
```

### Step 20 - Check the Services

```bash
kubectl get svc
```

### Step 21 - Check Helm Release Status

```bash
helm status myapp
```

### Step 22 - Uninstall the Helm Release

```bash
helm uninstall myapp
```

### Step 23 - Verify Cleanup

```bash
helm list
```

```bash
kubectl get pods
```

### Step 24 - Remove the Bitnami Repository

```bash
helm repo remove bitnami
```

### Step 25 - Return to Parent Directory

```bash
cd ..
```

### Step 26 - Remove the Practice Chart

```bash
rm -rf mychart
```

### Step 27 - Verify Cleanup

```bash
ls
```

The `mychart` directory should no longer exist.

## What You Learned

Helm dependencies allow one Helm chart to use another Helm chart.

In this lab you learned how to:

- Add a chart dependency
- Define dependencies in `Chart.yaml`
- Update dependencies
- Build dependencies
- Check dependency status
- Use the `charts` directory
- Render a chart with dependencies
- Install a chart with dependencies
- Verify Kubernetes resources
- Remove the Helm release
- Clean up the practice chart

## Important Commands

```bash
helm dependency list
```

```bash
helm dependency update
```

```bash
helm dependency build
```

```bash
helm lint .
```

```bash
helm template myapp .
```

```bash
helm install myapp .
```

```bash
helm uninstall myapp
```

## Lab 11 Complete

Next Lab:

**Lab 12 - Helm Upgrade and Rollback**
