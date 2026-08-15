# Lab 12 - Helm Upgrade and Rollback

## Objective

Understand how Helm upgrades work

Upgrade an existing Helm release

Change Helm values during an upgrade

Check Helm release revisions

Understand Helm rollback

Rollback a release to a previous revision

Verify the application after rollback

---

### Step 1 - Create a Helm Chart

```bash
helm create mychart
```

### Step 2 - Enter the Chart Directory

```bash
cd mychart
```

### Step 3 - Check the Chart

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

### Step 4 - Check the Default Replica Count

```bash
grep "replicaCount:" values.yaml
```

Expected Output:

```text
replicaCount: 1
```

### Step 5 - Install the Helm Release

```bash
helm install myapp .
```

Expected Output:

```text
NAME: myapp
STATUS: deployed
```

### Step 6 - Check the Helm Release

```bash
helm list
```

### Step 7 - Check the Release Status

```bash
helm status myapp
```

### Step 8 - Check the Deployment

```bash
kubectl get deployment
```

### Step 9 - Check the Pods

```bash
kubectl get pods
```

### Step 10 - Check the Current Revision

```bash
helm history myapp
```

Expected Output:

```text
REVISION    STATUS
1           deployed
```

### Step 11 - Upgrade the Release

Change the replica count from `1` to `3`:

```bash
helm upgrade myapp . --set replicaCount=3
```

Expected Output:

```text
Release "myapp" has been upgraded.
```

### Step 12 - Check Helm History

```bash
helm history myapp
```

Expected Output should now contain two revisions:

```text
REVISION    STATUS
1           superseded
2           deployed
```

### Step 13 - Check the Deployment

```bash
kubectl get deployment
```

### Step 14 - Check the Replica Count

```bash
kubectl get deployment -o wide
```

### Step 15 - Check the Pods

```bash
kubectl get pods
```

You should now have three application pods.

### Step 16 - Check the Helm Values

```bash
helm get values myapp
```

Expected Output:

```yaml
replicaCount: 3
```

### Step 17 - Upgrade the Release Again

Change the replica count to `2`:

```bash
helm upgrade myapp . --set replicaCount=2
```

### Step 18 - Check Helm History

```bash
helm history myapp
```

You should now have three revisions.

### Step 19 - Check the Pods

```bash
kubectl get pods
```

You should now have two application pods.

### Step 20 - Check the Current Release

```bash
helm status myapp
```

### Step 21 - Check Current Values

```bash
helm get values myapp
```

Expected Output:

```yaml
replicaCount: 2
```

### Step 22 - Roll Back to Revision 2

```bash
helm rollback myapp 2
```

Expected Output:

```text
Rollback was a success! Happy Helming!
```

### Step 23 - Check Helm History

```bash
helm history myapp
```

The latest revision should now show the rollback as `deployed`.

### Step 24 - Check the Replica Count

```bash
kubectl get deployment
```

### Step 25 - Check the Pods

```bash
kubectl get pods
```

The deployment should now have three replicas again because revision 2 used `replicaCount=3`.

### Step 26 - Check Helm Values

```bash
helm get values myapp
```

Expected Output:

```yaml
replicaCount: 3
```

### Step 27 - Check Release Status

```bash
helm status myapp
```

### Step 28 - View Release History

```bash
helm history myapp
```

The history should show the original installation, upgrades, and rollback.

### Step 29 - Uninstall the Helm Release

```bash
helm uninstall myapp
```

Expected Output:

```text
release "myapp" uninstalled
```

### Step 30 - Verify Cleanup

```bash
helm list
```

```bash
kubectl get pods
```

### Step 31 - Return to Parent Directory

```bash
cd ..
```

### Step 32 - Remove the Practice Chart

```bash
rm -rf mychart
```

### Step 33 - Verify Cleanup

```bash
ls
```

The `mychart` directory should no longer exist.

## What You Learned

Helm upgrades allow you to modify an existing Helm release without uninstalling it.

You learned how to:

- Install a Helm release
- Upgrade a Helm release
- Change values during an upgrade
- Check Helm release history
- Check release revisions
- Roll back to a previous revision
- Verify the application after rollback
- View Helm release values
- Uninstall a Helm release

## Important Commands

```bash
helm install myapp .
```

```bash
helm upgrade myapp . --set replicaCount=3
```

```bash
helm history myapp
```

```bash
helm get values myapp
```

```bash
helm rollback myapp 2
```

```bash
helm status myapp
```

```bash
helm uninstall myapp
```

## Lab 12 Complete

Next Lab:

**Lab 13 - Helm Package and Repository**
