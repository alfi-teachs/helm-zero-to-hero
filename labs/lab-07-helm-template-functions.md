# Lab 07 - Helm Template Functions

## Objective

Understand Helm template functions

Use template functions inside Helm charts

Use `default` function

Use `quote` function

Use `upper` and `lower` functions

Use `replace` function

Render templates and verify the output

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

---

### Step 4 - Check Current Values

```bash
cat values.yaml
```

---

### Step 5 - Create a Custom Template

```bash
cat > templates/functions.yaml <<'EOF'
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-functions
data:
  appName: {{ .Values.appName | default "my-application" }}
  appNameQuoted: {{ .Values.appName | default "my-application" | quote }}
  appNameUpper: {{ .Values.appName | default "my-application" | upper | quote }}
  appNameLower: {{ .Values.appName | default "MY-APPLICATION" | lower | quote }}
  environment: {{ .Values.environment | default "development" | quote }}
EOF
```

---

### Step 6 - Add Custom Values

```bash
cat >> values.yaml <<'EOF'

appName: "helm-demo"
environment: "production"
EOF
```

---

### Step 7 - Check the Values

```bash
tail -10 values.yaml
```

Expected Output should contain:

```text
appName: "helm-demo"
environment: "production"
```

---

### Step 8 - Render the Template

```bash
helm template myapp .
```

---

### Step 9 - Render Only the Functions Template

```bash
helm template myapp . --show-only templates/functions.yaml
```

---

### Step 10 - Verify the Default Function

```bash
helm template myapp . --show-only templates/functions.yaml | grep "environment:"
```

Expected Output:

```text
environment: "production"
```

---

### Step 11 - Test the `quote` Function

```bash
helm template myapp . --show-only templates/functions.yaml | grep "appNameQuoted:"
```

Expected Output:

```text
appNameQuoted: "helm-demo"
```

---

### Step 12 - Test the `upper` Function

```bash
helm template myapp . --show-only templates/functions.yaml | grep "appNameUpper:"
```

Expected Output:

```text
appNameUpper: "HELM-DEMO"
```

---

### Step 13 - Test the `lower` Function

```bash
helm template myapp . --show-only templates/functions.yaml | grep "appNameLower:"
```

Expected Output:

```text
appNameLower: "helm-demo"
```

---

### Step 14 - Test the `default` Function

Override the application name from the command line:

```bash
helm template myapp . --show-only templates/functions.yaml --set appName=""
```

The `default` function will use:

```text
my-application
```

---

### Step 15 - Test Multiple Values

```bash
helm template myapp . --show-only templates/functions.yaml --set appName="KUBERNETES" --set environment="DEV"
```

The output should contain values processed by the template functions.

---

### Step 16 - Check the Complete Rendered Chart

```bash
helm template myapp .
```

---

### Step 17 - Validate the Chart

```bash
helm lint .
```

Expected Output:

```text
1 chart(s) linted, 0 chart(s) failed
```

---

### Step 18 - Install the Chart

```bash
helm install myapp .
```

---

### Step 19 - Check the Helm Release

```bash
helm list
```

---

### Step 20 - Check the ConfigMap

```bash
kubectl get configmap
```

---

### Step 21 - View the ConfigMap

```bash
kubectl get configmap myapp-functions -o yaml
```

---

### Step 22 - Check Helm Release Status

```bash
helm status myapp
```

---

### Step 23 - Uninstall the Helm Release

```bash
helm uninstall myapp
```

---

### Step 24 - Verify Cleanup

```bash
helm list
```

```bash
kubectl get configmap
```

---

### Step 25 - Return to Parent Directory

```bash
cd ..
```

---

### Step 26 - Remove the Practice Chart

```bash
rm -rf mychart
```

---

### Step 27 - Verify Cleanup

```bash
ls
```

The `mychart` directory should no longer exist.

---

## What You Learned

Helm template functions allow you to transform and process values before they are placed into Kubernetes manifests.

In this lab you learned:

- `default` function
- `quote` function
- `upper` function
- `lower` function
- How to use functions with `.Values`
- How to render template functions
- How to override values using `--set`
- How to validate a chart with `helm lint`
- How to install and verify the chart

## Important Commands

```bash
helm template myapp .
```

```bash
helm template myapp . --show-only templates/functions.yaml
```

```bash
helm template myapp . --set appName="KUBERNETES"
```

```bash
helm lint .
```

```bash
helm install myapp .
```

```bash
helm uninstall myapp
```

## Lab 07 Complete

Next Lab:

**Lab 08 - Helm Conditionals**
