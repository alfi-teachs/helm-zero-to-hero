# Lab 09 - Helm Loops

## Objective

Understand how loops work in Helm templates

Use the `range` function

Create multiple Kubernetes resources using a loop

Pass multiple values from `values.yaml`

Render and verify loop output

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

### Step 3 - Add List Values

```bash
cat >> values.yaml <<'EOF'

configMaps:
  - name: app-config
    environment: production
  - name: database-config
    environment: production
  - name: cache-config
    environment: production
EOF
```

---

### Step 4 - Check the Values

```bash
tail -15 values.yaml
```

Expected Output:

```text
configMaps:
  - name: app-config
    environment: production
  - name: database-config
    environment: production
  - name: cache-config
    environment: production
```

---

### Step 5 - Create a Loop Template

```bash
cat > templates/configmaps.yaml <<'EOF'
{{- range .Values.configMaps }}
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .name }}
data:
  environment: {{ .environment | quote }}
---
{{- end }}
EOF
```

---

### Step 6 - Render the Helm Template

```bash
helm template myapp .
```

The loop should create three ConfigMaps.

---

### Step 7 - Render Only the Loop Template

```bash
helm template myapp . --show-only templates/configmaps.yaml
```

Expected Output will contain:

```yaml
kind: ConfigMap
metadata:
  name: app-config
```

```yaml
kind: ConfigMap
metadata:
  name: database-config
```

```yaml
kind: ConfigMap
metadata:
  name: cache-config
```

---

### Step 8 - Count the ConfigMaps

```bash
helm template myapp . --show-only templates/configmaps.yaml | grep "kind: ConfigMap"
```

Expected Output:

```text
kind: ConfigMap
kind: ConfigMap
kind: ConfigMap
```

---

### Step 9 - Add Another ConfigMap

```bash
cat >> values.yaml <<'EOF'
  - name: frontend-config
    environment: production
EOF
```

---

### Step 10 - Check the Updated Values

```bash
tail -10 values.yaml
```

---

### Step 11 - Render the Template Again

```bash
helm template myapp . --show-only templates/configmaps.yaml
```

You should now see four ConfigMaps.

---

### Step 12 - Count the ConfigMaps Again

```bash
helm template myapp . --show-only templates/configmaps.yaml | grep "kind: ConfigMap"
```

Expected Output:

```text
kind: ConfigMap
kind: ConfigMap
kind: ConfigMap
kind: ConfigMap
```

---

### Step 13 - Create a List of Applications

```bash
cat >> values.yaml <<'EOF'

applications:
  - nginx
  - httpd
  - redis
EOF
```

---

### Step 14 - Create an Application Loop

```bash
cat > templates/applications.yaml <<'EOF'
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-applications
data:
{{- range .Values.applications }}
  {{ . }}: enabled
{{- end }}
EOF
```

---

### Step 15 - Render the Application Loop

```bash
helm template myapp . --show-only templates/applications.yaml
```

Expected Output:

```yaml
data:
  nginx: enabled
  httpd: enabled
  redis: enabled
```

---

### Step 16 - Test the Complete Chart

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

### Step 18 - Install the Helm Chart

```bash
helm install myapp .
```

---

### Step 19 - Check the Helm Release

```bash
helm list
```

---

### Step 20 - Check ConfigMaps

```bash
kubectl get configmap
```

---

### Step 21 - Check the Application ConfigMap

```bash
kubectl get configmap myapp-applications -o yaml
```

---

### Step 22 - Check Individual ConfigMaps

```bash
kubectl get configmap app-config
```

```bash
kubectl get configmap database-config
```

```bash
kubectl get configmap cache-config
```

```bash
kubectl get configmap frontend-config
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

Helm uses the `range` function to loop through lists and create or process multiple values.

In this lab you learned:

- How to create lists in `values.yaml`
- How to use the `range` function
- How to access values inside a loop
- How to create multiple ConfigMaps using a loop
- How to render loop output
- How to count generated resources
- How to validate a Helm chart
- How to install and verify a Helm release

## Important Commands

```bash
helm template myapp .
```

```bash
helm template myapp . --show-only templates/configmaps.yaml
```

```bash
helm lint .
```

```bash
helm install myapp .
```

```bash
helm status myapp
```

```bash
helm uninstall myapp
```

## Lab 09 Complete

Next Lab:

**Lab 10 - Helm Helper Templates**
