# Lab 13 - Helm Package and Repository

## Objective

Understand how to package a Helm chart

Create a Helm chart package

Validate a Helm chart before packaging

Package a Helm chart into a `.tgz` file

Inspect the packaged chart

Create a local Helm repository

Add the local repository to Helm

Search for the packaged chart

Install the chart from the local repository

---

### Step 1 - Create a Helm Chart

```bash
helm create mychart
```

Expected Output:

```text
Creating mychart
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

### Step 4 - Validate the Helm Chart

```bash
helm lint .
```

Expected Output:

```text
1 chart(s) linted, 0 chart(s) failed
```

### Step 5 - Check Chart Information

```bash
helm show chart .
```

### Step 6 - Return to the Parent Directory

```bash
cd ..
```

### Step 7 - Package the Helm Chart

```bash
helm package mychart
```

Expected Output:

```text
Successfully packaged chart and saved it to: ...
```

### Step 8 - Check the Packaged Chart

```bash
ls
```

You should see a file similar to:

```text
mychart-0.1.0.tgz
```

### Step 9 - Check the Package File

```bash
ls -lh mychart-0.1.0.tgz
```

### Step 10 - Inspect the Packaged Chart

```bash
helm show chart mychart-0.1.0.tgz
```

### Step 11 - View Packaged Chart Values

```bash
helm show values mychart-0.1.0.tgz
```

### Step 12 - Create a Local Helm Repository Directory

```bash
mkdir helm-repo
```

### Step 13 - Copy the Chart Package to the Repository

```bash
cp mychart-0.1.0.tgz helm-repo/
```

### Step 14 - Enter the Helm Repository Directory

```bash
cd helm-repo
```

### Step 15 - Create the Helm Repository Index

```bash
helm repo index .
```

This creates:

```text
index.yaml
```

### Step 16 - Check the Repository Files

```bash
ls
```

Expected Output:

```text
index.yaml
mychart-0.1.0.tgz
```

### Step 17 - View the Repository Index

```bash
cat index.yaml
```

### Step 18 - Return to the Parent Directory

```bash
cd ..
```

### Step 19 - Add the Local Helm Repository

```bash
helm repo add local-repo file://$(pwd)/helm-repo
```

Expected Output:

```text
"local-repo" has been added to your repositories
```

### Step 20 - Check Helm Repositories

```bash
helm repo list
```

### Step 21 - Update Helm Repositories

```bash
helm repo update
```

### Step 22 - Search the Local Repository

```bash
helm search repo local-repo
```

Expected Output should contain:

```text
local-repo/mychart
```

### Step 23 - Show the Chart From the Repository

```bash
helm show chart local-repo/mychart
```

### Step 24 - Show the Chart Values From the Repository

```bash
helm show values local-repo/mychart
```

### Step 25 - Install the Chart From the Repository

```bash
helm install myapp local-repo/mychart
```

### Step 26 - Check the Helm Release

```bash
helm list
```

### Step 27 - Check Kubernetes Resources

```bash
kubectl get all
```

### Step 28 - Check the Pods

```bash
kubectl get pods
```

### Step 29 - Check the Helm Release Status

```bash
helm status myapp
```

### Step 30 - Uninstall the Helm Release

```bash
helm uninstall myapp
```

Expected Output:

```text
release "myapp" uninstalled
```

### Step 31 - Remove the Local Helm Repository

```bash
helm repo remove local-repo
```

### Step 32 - Verify Repository Cleanup

```bash
helm repo list
```

### Step 33 - Remove the Practice Files

```bash
rm -rf mychart helm-repo mychart-0.1.0.tgz
```

### Step 34 - Verify Cleanup

```bash
ls
```

The practice chart, package, and local repository should no longer exist.

---

## What You Learned

Helm charts can be packaged into `.tgz` files and stored in Helm repositories.

In this lab you learned how to:

- Create a Helm chart
- Validate a chart with `helm lint`
- Package a Helm chart
- Create a Helm repository
- Generate `index.yaml`
- Add a local Helm repository
- Search a Helm repository
- Install a chart from a repository
- Remove a Helm repository
- Clean up the practice environment

## Important Commands

```bash
helm lint mychart
```

```bash
helm package mychart
```

```bash
helm repo index .
```

```bash
helm repo add local-repo file://$(pwd)/helm-repo
```

```bash
helm repo update
```

```bash
helm search repo local-repo
```

```bash
helm install myapp local-repo/mychart
```

```bash
helm uninstall myapp
```

## Lab 13 Complete

Next Lab:

**Lab 14 - Helm Lint and Package**
