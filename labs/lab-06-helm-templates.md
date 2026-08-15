# Lab 06 - Helm Templates

## Objective

Understand how Helm templates work

Understand how Helm uses `values.yaml`

Render Kubernetes manifests using Helm

Use Helm template commands

Inspect generated Kubernetes YAML

Validate Helm templates

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

### Step 4 - Check the Templates Directory

```bash
ls templates
```

You should see files similar to:

```text
NOTES.txt
_helpers.tpl
deployment.yaml
hpa.yaml
ingress.yaml
service.yaml
serviceaccount.yaml
tests
```

---

### Step 5 - View the Deployment Template

```bash
cat templates/deployment.yaml
```

The Deployment template contains Helm expressions such as:

```text
{{ .Values.replicaCount }}
{{ .Values.image.repository }}
{{ .Values.image.tag }}
```

These values are taken from `values.yaml`.

---

### Step 6 - View the Values File

```bash
cat values.yaml
```

---

### Step 7 - Render the Helm Templates

```bash
helm template myapp .
```

This renders the Helm templates into Kubernetes YAML without installing anything.

---

### Step 8 - Save Rendered Templates to a File

```bash
helm template myapp . > rendered.yaml
```

---

### Step 9 - View the Rendered YAML

```bash
cat rendered.yaml
```

---

### Step 10 - Search for the Deployment

```bash
grep -A20 "kind: Deployment" rendered.yaml
```

---

### Step 11 - Search for the Service

```bash
grep -A20 "kind: Service" rendered.yaml
```

---

### Step 12 - Change Replica Count

```bash
helm template myapp . --set replicaCount=3
```

---

### Step 13 - Verify Replica Count

```bash
helm template myapp . --set replicaCount=3 | grep "replicas:"
```

Expected Output:

```text
replicas: 3
```

---

### Step 14 - Change the Container Image

```bash
helm template myapp . --set image.repository=httpd
```

---

### Step 15 - Verify the Container Image

```bash
helm template myapp . --set image.repository=httpd | grep "image:"
```

---

### Step 16 - Render the Chart Using a Custom Values File

```bash
cat > custom-values.yaml <<'EOF'
replicaCount: 2

image:
  repository: nginx
  pullPolicy: IfNotPresent
  tag: "latest"
EOF
```

---

### Step 17 - Check the Custom Values File

```bash
cat custom-values.yaml
```

---

### Step 18 - Render Using Custom Values

```bash
helm template myapp . -f custom-values.yaml
```

---

### Step 19 - Save Custom Rendered YAML

```bash
helm template myapp . -f custom-values.yaml > custom-rendered.yaml
```

---

### Step 20 - View Custom Rendered YAML

```bash
cat custom-rendered.yaml
```

---

### Step 21 - Validate the Helm Chart

```bash
helm lint .
```

Expected Output:

```text
1 chart(s) linted, 0 chart(s) failed
```

---

### Step 22 - Render With Debug Information

```bash
helm template myapp . --debug
```

---

### Step 23 - Install the Helm Chart

```bash
helm install myapp .
```

---

### Step 24 - Check the Helm Release

```bash
helm list
```

---

### Step 25 - Check Kubernetes Resources

```bash
kubectl get all
```

---

### Step 26 - Check the Pods

```bash
kubectl get pods
```

---

### Step 27 - Check the Deployment

```bash
kubectl get deployment
```

---

### Step 28 - Check the Service

```bash
kubectl get svc
```

---

### Step 29 - Check the Helm Release Status

```bash
helm status myapp
```

---

### Step 30 - View the Installed Manifest

```bash
helm get manifest myapp
```

This displays the Kubernetes YAML that Helm installed.

---

### Step 31 - Uninstall the Helm Release

```bash
helm uninstall myapp
```

---

### Step 32 - Verify Cleanup

```bash
helm list
```

```bash
kubectl get pods
```

---

### Step 33 - Return to Parent Directory

```bash
cd ..
```

---

### Step 34 - Remove the Practice Chart

```bash
rm -rf mychart
```

---

### Step 35 - Verify Cleanup

```bash
ls
```

The `mychart` directory should no longer exist.

---

## What You Learned

Helm templates are Kubernetes YAML files containing Helm template expressions.

You learned how to:

- Create a Helm chart
- Understand the `templates` directory
- Understand how `values.yaml` is used
- Render Helm templates
- Use `helm template`
- Override values using `--set`
- Use a custom values file
- Save rendered YAML
- Validate templates using `helm lint`
- Use `--debug`
- Install a chart
- View installed manifests
- Uninstall a Helm release

## Important Commands

```bash
helm template myapp .
```

```bash
helm template myapp . --debug
```

```bash
helm template myapp . --set replicaCount=3
```

```bash
helm template myapp . -f custom-values.yaml
```

```bash
helm lint .
```

```bash
helm get manifest myapp
```

## Lab 06 Complete

Next Lab:

**Lab 07 - Helm Template Functions**
