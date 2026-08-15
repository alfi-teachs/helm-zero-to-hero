# Lab 08 - Helm Conditionals

## Objective

Understand Helm conditional statements

Use `if` conditions in Helm templates

Control Kubernetes resources using values

Use `if` and `else` conditions

Enable or disable resources using `values.yaml`

Render and verify conditional templates

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

### Step 4 - Add a Conditional Value

```bash
cat >> values.yaml <<'EOF'

configMap:
  enabled: true

environment: production
EOF
```

---

### Step 5 - Check the Values

```bash
tail -10 values.yaml
```

Expected Output:

```text
configMap:
  enabled: true

environment: production
```

---

### Step 6 - Create a Conditional ConfigMap

```bash
cat > templates/configmap.yaml <<'EOF'
{{- if .Values.configMap.enabled }}
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-config
data:
  environment: {{ .Values.environment | quote }}
  message: "ConfigMap enabled"
{{- end }}
EOF
```

---

### Step 7 - Render the Chart

```bash
helm template myapp .
```

Because `configMap.enabled` is `true`, the ConfigMap should appear in the rendered output.

---

### Step 8 - Render Only the ConfigMap

```bash
helm template myapp . --show-only templates/configmap.yaml
```

Expected Output:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: myapp-config
data:
  environment: "production"
  message: "ConfigMap enabled"
```

---

### Step 9 - Disable the ConfigMap

```bash
helm template myapp . --set configMap.enabled=false
```

The ConfigMap should not appear in the rendered output.

---

### Step 10 - Verify the ConfigMap Is Disabled

```bash
helm template myapp . --set configMap.enabled=false --show-only templates/configmap.yaml
```

The output should be empty because the `if` condition is false.

---

### Step 11 - Create an If-Else Conditional

```bash
cat > templates/environment.yaml <<'EOF'
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-environment
data:
  {{- if eq .Values.environment "production" }}
  message: "Production environment"
  {{- else }}
  message: "Non-production environment"
  {{- end }}
EOF
```

---

### Step 12 - Render the Production Condition

```bash
helm template myapp . --show-only templates/environment.yaml
```

Expected Output:

```yaml
message: "Production environment"
```

---

### Step 13 - Test the Else Condition

```bash
helm template myapp . --set environment=development --show-only templates/environment.yaml
```

Expected Output:

```yaml
message: "Non-production environment"
```

---

### Step 14 - Test Another Environment

```bash
helm template myapp . --set environment=staging --show-only templates/environment.yaml
```

Expected Output:

```yaml
message: "Non-production environment"
```

---

### Step 15 - Check the Complete Chart

```bash
helm template myapp .
```

---

### Step 16 - Validate the Chart

```bash
helm lint .
```

Expected Output:

```text
1 chart(s) linted, 0 chart(s) failed
```

---

### Step 17 - Install the Chart

```bash
helm install myapp .
```

---

### Step 18 - Check the Helm Release

```bash
helm list
```

---

### Step 19 - Check ConfigMaps

```bash
kubectl get configmap
```

---

### Step 20 - Check the Conditional ConfigMap

```bash
kubectl get configmap myapp-config
```

---

### Step 21 - View the ConfigMap

```bash
kubectl get configmap myapp-config -o yaml
```

---

### Step 22 - Check the Environment ConfigMap

```bash
kubectl get configmap myapp-environment -o yaml
```

---

### Step 23 - Check Helm Release Status

```bash
helm status myapp
```

---

### Step 24 - Uninstall the Helm Release

```bash
helm uninstall myapp
```

---

### Step 25 - Verify Cleanup

```bash
helm list
```

```bash
kubectl get configmap
```

---

### Step 26 - Return to Parent Directory

```bash
cd ..
```

---

### Step 27 - Remove the Practice Chart

```bash
rm -rf mychart
```

---

### Step 28 - Verify Cleanup

```bash
ls
```

The `mychart` directory should no longer exist.

---

## What You Learned

Helm conditionals allow you to create Kubernetes resources only when a specific condition is true.

You learned how to:

- Use `if` conditions
- Use `else` conditions
- Compare values using `eq`
- Enable or disable resources
- Control templates using `values.yaml`
- Override conditional values using `--set`
- Render conditional templates
- Validate conditional templates
- Install and verify a Helm chart

## Important Commands

```bash
helm template myapp .
```

```bash
helm template myapp . --set configMap.enabled=false
```

```bash
helm template myapp . --set environment=development
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

## Lab 08 Complete

Next Lab:

**Lab 09 - Helm Loops**
