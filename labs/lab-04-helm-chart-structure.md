# Lab 04 - Helm Chart Structure

## Objective

Understand the structure of a Helm chart

Create a Helm chart

Explore the important Helm chart files and directories

Understand the purpose of `Chart.yaml`

Understand the purpose of `values.yaml`

Understand the purpose of the `templates` directory

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

### Step 2 - Check the Chart Directory

```bash
ls
```

You should see:

```text
mychart
```

---

### Step 3 - Enter the Chart Directory

```bash
cd mychart
```

---

### Step 4 - Check Chart Files

```bash
ls
```

You should see files and directories similar to:

```text
Chart.yaml
charts
templates
values.yaml
```

---

### Step 5 - View the Complete Chart Structure

```bash
find . -maxdepth 2 -type f
```

Expected structure:

```text
./Chart.yaml
./values.yaml
./templates/NOTES.txt
./templates/_helpers.tpl
./templates/deployment.yaml
./templates/hpa.yaml
./templates/ingress.yaml
./templates/service.yaml
./templates/serviceaccount.yaml
./templates/tests/test-connection.yaml
```

---

### Step 6 - Check Chart.yaml

```bash
cat Chart.yaml
```

`Chart.yaml` contains information about the Helm chart.

Example:

```yaml
apiVersion: v2
name: mychart
description: A Helm chart for Kubernetes
type: application
version: 0.1.0
appVersion: "1.16.0"
```

---

### Step 7 - Check values.yaml

```bash
cat values.yaml
```

`values.yaml` contains the default configuration values used by the Helm templates.

---

### Step 8 - Check the Templates Directory

```bash
ls templates
```

The `templates` directory contains Kubernetes YAML templates.

---

### Step 9 - Check Deployment Template

```bash
cat templates/deployment.yaml
```

---

### Step 10 - Check Service Template

```bash
cat templates/service.yaml
```

---

### Step 11 - Check Helper Templates

```bash
cat templates/_helpers.tpl
```

The `_helpers.tpl` file contains reusable template definitions.

---

### Step 12 - Check the Charts Directory

```bash
ls charts
```

The `charts` directory is used to store chart dependencies.

It may be empty because this chart does not currently have any dependencies.

---

### Step 13 - Check Chart Information

```bash
helm show chart .
```

---

### Step 14 - Check Chart Default Values

```bash
helm show values .
```

---

### Step 15 - Validate the Chart

```bash
helm lint .
```

Expected Output:

```text
1 chart(s) linted, 0 chart(s) failed
```

---

### Step 16 - Render the Kubernetes YAML

```bash
helm template mychart .
```

This command generates Kubernetes YAML from the Helm templates without installing the chart.

---

### Step 17 - Save the Rendered YAML

```bash
helm template mychart . > rendered.yaml
```

---

### Step 18 - View the Rendered YAML

```bash
cat rendered.yaml
```

---

### Step 19 - Check the Chart Directory Again

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

### Step 20 - Return to the Parent Directory

```bash
cd ..
```

---

### Step 21 - Remove the Practice Chart

```bash
rm -rf mychart
```

---

### Step 22 - Verify Cleanup

```bash
ls
```

The `mychart` directory should no longer exist.

---

## What You Learned

A Helm chart has several important components.

### Chart.yaml

Contains chart metadata such as:

```text
Chart name
Chart version
Application version
Chart description
Chart type
```

### values.yaml

Contains default configuration values used by the chart.

### templates

Contains Kubernetes resource templates.

Examples:

```text
deployment.yaml
service.yaml
ingress.yaml
serviceaccount.yaml
_helpers.tpl
```

### charts

Stores chart dependencies.

### Helm Chart Structure

```text
mychart/
├── Chart.yaml
├── values.yaml
├── charts/
└── templates/
    ├── _helpers.tpl
    ├── deployment.yaml
    ├── service.yaml
    ├── ingress.yaml
    └── serviceaccount.yaml
```

## Lab 04 Complete

Next Lab:

**Lab 05 - Helm Values**
