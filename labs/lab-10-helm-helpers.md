# Lab 10 - Helm Helper Templates

## Objective

Understand Helm helper templates

Understand the purpose of `_helpers.tpl`

Create reusable template definitions

Use helper templates inside Kubernetes manifests

Use `include` with helper templates

Render and verify helper template output

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

### Step 3 - Check the Templates Directory

```bash
ls templates
```

You should see:

```text
_helpers.tpl
deployment.yaml
service.yaml
serviceaccount.yaml
ingress.yaml
hpa.yaml
NOTES.txt
tests
```

---

### Step 4 - View the Helper Template File

```bash
cat templates/_helpers.tpl
```

The `_helpers.tpl` file contains reusable template definitions.

---

### Step 5 - Check the Chart Name

```bash
grep -A5 "define" templates/_helpers.tpl
```

You should see helper definitions similar to:

```text
{{- define "mychart.name" -}}
{{- define "mychart.fullname" -}}
{{- define "mychart.chart" -}}
```

---

### Step 6 - Create a Custom Helper

```bash
cat >> templates/_helpers.tpl <<'EOF'

{{/*
Create a custom application label.
*/}}
{{- define "mychart.appLabel" -}}
helm-demo
{{- end }}

{{/*
Create a custom environment label.
*/}}
{{- define "mychart.environment" -}}
production
{{- end }}
EOF
```

---

### Step 7 - Create a ConfigMap Using Helpers

```bash
cat > templates/helper-configmap.yaml <<'EOF'
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ include "mychart.fullname" . }}-config
  labels:
    app: {{ include "mychart.appLabel" . }}
    environment: {{ include "mychart.environment" . }}
data:
  application: {{ include "mychart.name" . | quote }}
  release: {{ .Release.Name | quote }}
EOF
```

---

### Step 8 - Render the Helper Template

```bash
helm template myapp . --show-only templates/helper-configmap.yaml
```

Expected Output will contain:

```yaml
kind: ConfigMap
metadata:
  name: myapp-mychart-config
  labels:
    app: helm-demo
    environment: production
```

---

### Step 9 - Check the Application Name

```bash
helm template myapp . --show-only templates/helper-configmap.yaml | grep "application:"
```

---

### Step 10 - Check the Release Name

```bash
helm template myapp . --show-only templates/helper-configmap.yaml | grep "release:"
```

Expected Output:

```text
release: "myapp"
```

---

### Step 11 - Check the Custom Helper

```bash
helm template myapp . --show-only templates/helper-configmap.yaml | grep "app:"
```

Expected Output:

```text
app: helm-demo
```

---

### Step 12 - Check the Environment Helper

```bash
helm template myapp . --show-only templates/helper-configmap.yaml | grep "environment:"
```

Expected Output:

```text
environment: production
```

---

### Step 13 - Validate the Chart

```bash
helm lint .
```

Expected Output:

```text
1 chart(s) linted, 0 chart(s) failed
```

---

### Step 14 - Render the Complete Chart

```bash
helm template myapp .
```

---

### Step 15 - Install the Helm Chart

```bash
helm install myapp .
```

---

### Step 16 - Check the Helm Release

```bash
helm list
```

---

### Step 17 - Check the ConfigMap

```bash
kubectl get configmap
```

---

### Step 18 - Check the Helper ConfigMap

```bash
kubectl get configmap myapp-mychart-config
```

---

### Step 19 - View the Helper ConfigMap

```bash
kubectl get configmap myapp-mychart-config -o yaml
```

---

### Step 20 - Check Helm Release Status

```bash
helm status myapp
```

---

### Step 21 - Uninstall the Helm Release

```bash
helm uninstall myapp
```

---

### Step 22 - Verify Cleanup

```bash
helm list
```

```bash
kubectl get configmap
```

---

### Step 23 - Return to Parent Directory

```bash
cd ..
```

---

### Step 24 - Remove the Practice Chart

```bash
rm -rf mychart
```

---

### Step 25 - Verify Cleanup

```bash
ls
```

The `mychart` directory should no longer exist.

---

## What You Learned

Helper templates are reusable template definitions stored in `_helpers.tpl`.

You learned how to:

- Find `_helpers.tpl`
- Create custom helper templates
- Use `define`
- Use `include`
- Reuse helper templates
- Use helper templates in Kubernetes resources
- Combine helpers with Helm built-in objects
- Validate helper templates
- Render helper templates
- Install and verify a Helm release

## Important Commands

```bash
cat templates/_helpers.tpl
```

```bash
helm template myapp . --show-only templates/helper-configmap.yaml
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

## Lab 10 Complete

Next Lab:

**Lab 11 - Helm Dependencies**
