# Lab 14 - Helm Lint and Package

## Objective

Understand Helm chart validation

Use `helm lint` to check a Helm chart

Fix common Helm chart issues

Package a Helm chart

Verify the packaged chart

Inspect the packaged chart

---

### Step 1 - Create a Helm Chart

```bash
helm create mychart
```

Expected Output:

```text
Creating mychart
```

---

### Step 2 - Enter the Chart Directory

```bash
cd mychart
```

---

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

---

### Step 4 - Check Chart Information

```bash
cat Chart.yaml
```

---

### Step 5 - Check Chart Values

```bash
cat values.yaml
```

---

### Step 6 - Run Helm Lint

```bash
helm lint .
```

Expected Output:

```text
1 chart(s) linted, 0 chart(s) failed
```

---

### Step 7 - Check the Chart With Debug Information

```bash
helm lint . --debug
```

---

### Step 8 - Render the Chart

```bash
helm template myapp .
```

This checks whether Helm can successfully render the Kubernetes manifests.

---

### Step 9 - Save the Rendered Output

```bash
helm template myapp . > rendered.yaml
```

---

### Step 10 - Check the Rendered File

```bash
ls
```

You should now see:

```text
Chart.yaml
charts
templates
values.yaml
rendered.yaml
```

---

### Step 11 - View the Rendered Kubernetes YAML

```bash
cat rendered.yaml
```

---

### Step 12 - Package the Helm Chart

First return to the parent directory:

```bash
cd ..
```

Then package the chart:

```bash
helm package mychart
```

Expected Output:

```text
Successfully packaged chart and saved it to: ...
```

---

### Step 13 - Check the Package

```bash
ls
```

You should see:

```text
mychart
mychart-0.1.0.tgz
```

---

### Step 14 - Check the Package File

```bash
ls -lh mychart-0.1.0.tgz
```

---

### Step 15 - Inspect the Packaged Chart

```bash
helm show chart mychart-0.1.0.tgz
```

---

### Step 16 - View Packaged Chart Values

```bash
helm show values mychart-0.1.0.tgz
```

---

### Step 17 - Verify the Package

```bash
helm verify mychart-0.1.0.tgz
```

If the chart has no provenance file, Helm may report that no provenance file exists. This is expected because we have not signed the chart.

---

### Step 18 - Install the Packaged Chart

```bash
helm install myapp ./mychart-0.1.0.tgz
```

---

### Step 19 - Check the Helm Release

```bash
helm list
```

Expected Output should contain:

```text
myapp
```

---

### Step 20 - Check Kubernetes Resources

```bash
kubectl get all
```

---

### Step 21 - Check Pods

```bash
kubectl get pods
```

---

### Step 22 - Check the Helm Release Status

```bash
helm status myapp
```

---

### Step 23 - Uninstall the Helm Release

```bash
helm uninstall myapp
```

Expected Output:

```text
release "myapp" uninstalled
```

---

### Step 24 - Verify Cleanup

```bash
helm list
```

```bash
kubectl get pods
```

---

### Step 25 - Remove the Practice Files

```bash
rm -rf mychart mychart-0.1.0.tgz
```

---

### Step 26 - Verify Final Cleanup

```bash
ls
```

The practice chart and package should no longer exist.

---

## What You Learned

`helm lint` checks a Helm chart for possible problems before you install or package it.

You learned how to:

- Create a Helm chart
- Validate a Helm chart
- Use `helm lint`
- Use `helm lint --debug`
- Render Helm templates
- Save rendered Kubernetes YAML
- Package a Helm chart
- Inspect a packaged chart
- View packaged chart values
- Install a packaged chart
- Verify Kubernetes resources
- Uninstall the Helm release
- Clean up the practice environment

## Important Commands

```bash
helm lint .
```

```bash
helm lint . --debug
```

```bash
helm template myapp .
```

```bash
helm package mychart
```

```bash
helm show chart mychart-0.1.0.tgz
```

```bash
helm show values mychart-0.1.0.tgz
```

```bash
helm install myapp ./mychart-0.1.0.tgz
```

```bash
helm uninstall myapp
```

## Lab 14 Complete

Next Lab:

**Lab 15 - Helm Final Project**
