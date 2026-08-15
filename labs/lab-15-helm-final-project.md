# Lab 15 - Helm Final Project

## Objective

Build a complete Helm project from scratch

Create a custom Helm chart

Configure the application using `values.yaml`

Use Helm templates

Use Helm helper templates

Use Helm conditionals

Use Helm loops

Validate the Helm chart

Install the application using Helm

Upgrade the Helm release

Rollback the Helm release

Package the Helm chart

Clean up all Kubernetes resources

---

### Step 1 - Check Kubernetes Cluster

```bash
kubectl get nodes
```

Expected Output:

```text
NAME       STATUS   ROLES           AGE   VERSION
minikube   Ready    control-plane   XXd   v1.xx.x
```

---

### Step 2 - Check Helm

```bash
helm version
```

---

### Step 3 - Create the Project Directory

```bash
mkdir helm-final-project
```

```bash
cd helm-final-project
```

---

### Step 4 - Create the Helm Chart

```bash
helm create myapp
```

Expected Output:

```text
Creating myapp
```

---

### Step 5 - Enter the Helm Chart

```bash
cd myapp
```

---

### Step 6 - Check the Chart Structure

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

### Step 7 - Check Chart.yaml

```bash
cat Chart.yaml
```

---

### Step 8 - Update Chart.yaml

```bash
cat > Chart.yaml <<'EOF'
apiVersion: v2
name: myapp
description: Helm Final Project
type: application
version: 1.0.0
appVersion: "1.0"
EOF
```

---

### Step 9 - Create values.yaml

```bash
cat > values.yaml <<'EOF'
replicaCount: 2

image:
  repository: nginx
  pullPolicy: IfNotPresent
  tag: "latest"

service:
  type: NodePort
  port: 80
  nodePort: 30080

app:
  name: helm-final-project
  environment: production

configMap:
  enabled: true

applications:
  - frontend
  - backend
  - database
EOF
```

---

### Step 10 - Check values.yaml

```bash
cat values.yaml
```

---

### Step 11 - Create Helper Templates

```bash
cat > templates/_helpers.tpl <<'EOF'
{{/*
Application name
*/}}
{{- define "myapp.name" -}}
{{ .Values.app.name }}
{{- end }}

{{/*
Full application name
*/}}
{{- define "myapp.fullname" -}}
{{ .Release.Name }}-{{ include "myapp.name" . }}
{{- end }}

{{/*
Application labels
*/}}
{{- define "myapp.labels" -}}
app: {{ include "myapp.name" . }}
environment: {{ .Values.app.environment }}
{{- end }}
EOF
```

---

### Step 12 - Create Deployment Template

```bash
cat > templates/deployment.yaml <<'EOF'
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ include "myapp.fullname" . }}
  labels:
    {{- include "myapp.labels" . | nindent 4 }}
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      app: {{ include "myapp.name" . }}
  template:
    metadata:
      labels:
        app: {{ include "myapp.name" . }}
    spec:
      containers:
        - name: {{ include "myapp.name" . }}
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
          imagePullPolicy: {{ .Values.image.pullPolicy }}
          ports:
            - containerPort: 80
EOF
```

---

### Step 13 - Create Service Template

```bash
cat > templates/service.yaml <<'EOF'
apiVersion: v1
kind: Service
metadata:
  name: {{ include "myapp.fullname" . }}
spec:
  type: {{ .Values.service.type }}
  selector:
    app: {{ include "myapp.name" . }}
  ports:
    - port: {{ .Values.service.port }}
      targetPort: 80
      nodePort: {{ .Values.service.nodePort }}
EOF
```

---

### Step 14 - Create Conditional ConfigMap

```bash
cat > templates/configmap.yaml <<'EOF'
{{- if .Values.configMap.enabled }}
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ include "myapp.fullname" . }}-config
data:
  application: {{ include "myapp.name" . | quote }}
  environment: {{ .Values.app.environment | quote }}
{{- end }}
EOF
```

---

### Step 15 - Create Loop Template

```bash
cat > templates/applications.yaml <<'EOF'
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ include "myapp.fullname" . }}-applications
data:
{{- range .Values.applications }}
  {{ . }}: enabled
{{- end }}
EOF
```

---

### Step 16 - Remove Unused Default Templates

```bash
rm -f templates/ingress.yaml templates/hpa.yaml templates/httproute.yaml templates/NOTES.txt
```

---

### Step 17 - Check Templates

```bash
ls templates
```

Expected Output:

```text
_helpers.tpl
applications.yaml
configmap.yaml
deployment.yaml
service.yaml
serviceaccount.yaml
tests
```

---

### Step 18 - Validate the Helm Chart

```bash
helm lint .
```

Expected Output:

```text
1 chart(s) linted, 0 chart(s) failed
```

---

### Step 19 - Render the Helm Chart

```bash
helm template myapp .
```

---

### Step 20 - Save Rendered Kubernetes YAML

```bash
helm template myapp . > rendered.yaml
```

---

### Step 21 - Check Rendered YAML

```bash
cat rendered.yaml
```

---

### Step 22 - Check Deployment

```bash
helm template myapp . | grep -A10 "kind: Deployment"
```

---

### Step 23 - Check Service

```bash
helm template myapp . | grep -A10 "kind: Service"
```

---

### Step 24 - Check ConfigMap

```bash
helm template myapp . | grep -A10 "kind: ConfigMap"
```

---

### Step 25 - Test Replica Override

```bash
helm template myapp . --set replicaCount=3 | grep "replicas:"
```

Expected Output:

```text
replicas: 3
```

---

### Step 26 - Test Image Override

```bash
helm template myapp . --set image.repository=httpd
```

---

### Step 27 - Test ConfigMap Condition

```bash
helm template myapp . --set configMap.enabled=false
```

The conditional ConfigMap should not be generated.

---

### Step 28 - Test the Loop

```bash
helm template myapp . | grep -A10 "name: myapp-applications"
```

The generated ConfigMap should contain:

```text
frontend: enabled
backend: enabled
database: enabled
```

---

### Step 29 - Install the Helm Application

```bash
helm install myapp .
```

Expected Output:

```text
NAME: myapp
STATUS: deployed
```

---

### Step 30 - Check Helm Release

```bash
helm list
```

---

### Step 31 - Check Helm Status

```bash
helm status myapp
```

---

### Step 32 - Check Kubernetes Resources

```bash
kubectl get all
```

---

### Step 33 - Check Pods

```bash
kubectl get pods
```

Expected Output should show two pods.

```text
NAME                                      READY   STATUS
myapp-helm-final-project-xxxxxxxxxx-xxxxx 1/1     Running
myapp-helm-final-project-xxxxxxxxxx-xxxxx 1/1     Running
```

---

### Step 34 - Check Deployment

```bash
kubectl get deployment
```

---

### Step 35 - Check Service

```bash
kubectl get svc
```

Expected Output should contain:

```text
myapp-helm-final-project
```

---

### Step 36 - Check ConfigMaps

```bash
kubectl get configmap
```

---

### Step 37 - Check Application ConfigMap

```bash
kubectl get configmap myapp-helm-final-project-config -o yaml
```

---

### Step 38 - Check Application List ConfigMap

```bash
kubectl get configmap myapp-helm-final-project-applications -o yaml
```

---

### Step 39 - Check Service URL

```bash
minikube service myapp-helm-final-project --url
```

Copy the URL displayed by Minikube and open it in your browser.

---

### Step 40 - Upgrade the Application

Change the replica count from `2` to `3`:

```bash
helm upgrade myapp . --set replicaCount=3
```

Expected Output:

```text
Release "myapp" has been upgraded.
```

---

### Step 41 - Check Helm History

```bash
helm history myapp
```

You should see multiple revisions.

---

### Step 42 - Check Pods After Upgrade

```bash
kubectl get pods
```

You should now have three application pods.

---

### Step 43 - Check Current Values

```bash
helm get values myapp
```

Expected Output:

```yaml
replicaCount: 3
```

---

### Step 44 - Upgrade Again

Change the replica count to `1`:

```bash
helm upgrade myapp . --set replicaCount=1
```

---

### Step 45 - Check Pods

```bash
kubectl get pods
```

You should now have one application pod.

---

### Step 46 - Check Helm History

```bash
helm history myapp
```

---

### Step 47 - Roll Back to the Previous Revision

```bash
helm rollback myapp 2
```

Expected Output:

```text
Rollback was a success! Happy Helming!
```

---

### Step 48 - Check Helm History

```bash
helm history myapp
```

---

### Step 49 - Check Pods After Rollback

```bash
kubectl get pods
```

The application should return to the replica count from the selected revision.

---

### Step 50 - Check Helm Status

```bash
helm status myapp
```

---

### Step 51 - Package the Helm Chart

First return to the project directory:

```bash
cd ..
```

Then package the chart:

```bash
helm package myapp
```

Expected Output:

```text
Successfully packaged chart and saved it to: ...
```

---

### Step 52 - Check the Package

```bash
ls
```

Expected Output:

```text
myapp
myapp-1.0.0.tgz
```

---

### Step 53 - Inspect the Packaged Chart

```bash
helm show chart myapp-1.0.0.tgz
```

---

### Step 54 - View Packaged Chart Values

```bash
helm show values myapp-1.0.0.tgz
```

---

### Step 55 - Install From the Packaged Chart

```bash
helm install packaged-app ./myapp-1.0.0.tgz
```

---

### Step 56 - Check Helm Releases

```bash
helm list
```

You should see:

```text
myapp
packaged-app
```

---

### Step 57 - Check Kubernetes Resources

```bash
kubectl get all
```

---

### Step 58 - Uninstall the Original Release

```bash
helm uninstall myapp
```

---

### Step 59 - Uninstall the Packaged Release

```bash
helm uninstall packaged-app
```

---

### Step 60 - Verify Helm Cleanup

```bash
helm list
```

---

### Step 61 - Verify Kubernetes Cleanup

```bash
kubectl get pods
```

```bash
kubectl get svc
```

```bash
kubectl get configmap
```

---

### Step 62 - Return to the Parent Directory

```bash
cd ..
```

---

### Step 63 - Remove the Final Project

```bash
rm -rf helm-final-project
```

---

### Step 64 - Verify Final Cleanup

```bash
ls
```

The `helm-final-project` directory should no longer exist.

---

## What You Learned

In this final project you used the major Helm concepts learned throughout the labs.

You learned how to:

- Create a Helm chart
- Configure `Chart.yaml`
- Configure `values.yaml`
- Create Helm templates
- Use helper templates
- Use `include`
- Use conditional statements
- Use `range` loops
- Override values using `--set`
- Render Kubernetes manifests
- Validate a Helm chart
- Install a Helm release
- Upgrade a Helm release
- Check Helm release history
- Roll back a Helm release
- Package a Helm chart
- Install a packaged Helm chart
- Verify Kubernetes resources
- Clean up the complete environment

## Important Commands

```bash
helm create myapp
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
helm list
```

```bash
helm status myapp
```

```bash
helm upgrade myapp . --set replicaCount=3
```

```bash
helm history myapp
```

```bash
helm rollback myapp 2
```

```bash
helm package myapp
```

```bash
helm install packaged-app ./myapp-1.0.0.tgz
```

```bash
helm uninstall myapp
```

```bash
helm uninstall packaged-app
```

## Helm Learning Series Complete

You have completed:

```text
Lab 01 - Helm Installation
Lab 02 - Helm Repositories
Lab 03 - Install Helm Chart
Lab 04 - Helm Chart Structure
Lab 05 - Helm Values
Lab 06 - Helm Templates
Lab 07 - Helm Template Functions
Lab 08 - Helm Conditionals
Lab 09 - Helm Loops
Lab 10 - Helm Helper Templates
Lab 11 - Helm Dependencies
Lab 12 - Helm Upgrade and Rollback
Lab 13 - Helm Package and Repository
Lab 14 - Helm Lint and Package
Lab 15 - Helm Final Project
```

## Lab 15 Complete
