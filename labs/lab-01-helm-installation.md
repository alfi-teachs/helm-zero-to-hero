# Lab 01 - Helm Installation
## Objective
Understand what Helm is

Verify Helm installation

Verify Kubernetes connectivity

Explore basic Helm commands

### Step 1 - Check Kubernetes Cluster
```bash
kubectl get nodes
```
Expected Output:

NAME           STATUS   ROLES           AGE   VERSION
```bash
minikube       Ready    control-plane   XXd   v1.xx.x
```
### Step 2 - Check kubectl
```bash
kubectl version --client
```
Check current Kubernetes context:
```bash
kubectl config current-context
```
### Step 3 - Check if Helm is Installed
```bash
helm version
```
If Helm is installed, you should see a version similar to:

version.BuildInfo{Version:"v3.x.x"}
### Step 4 - Install Helm (Linux/Ubuntu)

If Helm is not installed:
```bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```
Verify installation:
```bash
helm version
```
### Step 5 - Explore Helm Commands

Display Helm help:
```bash
helm help
```
Display Helm environment variables:

helm env

Check install command help:

helm install --help

Check repository command help:

helm repo --help

Check chart creation help:

helm create --help
Step 6 - Verify Helm Connectivity

List Helm releases:

helm list

List Helm releases across all namespaces:

helm list -A

If no releases exist, an empty list is expected.

Step 7 - Final Verification

Run all verification commands:

kubectl get nodes
helm version
helm list
helm list -A
Cleanup

No cleanup is required for this lab because no Helm charts were installed.
