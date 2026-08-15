# helm-zero-to-hero
# Helm Labs - Zero to Hero

A hands-on Helm learning series covering Helm installation, repositories, charts, templates, values, conditionals, loops, dependencies, upgrades, rollbacks, packaging, and a final project.

## Labs

| Lab | Topic |
|---|---|
| [Lab 01 - Helm Installation](labs/lab-01-helm-installation/README.md) | Install Helm and verify Kubernetes connectivity |
| [Lab 02 - Helm Repositories](labs/lab-02-helm-repositories/README.md) | Add, update, search, and manage Helm repositories |
| [Lab 03 - Install Helm Chart](labs/lab-03-install-helm-chart/README.md) | Install and manage a Helm chart |
| [Lab 04 - Helm Chart Structure](labs/lab-04-helm-chart-structure/README.md) | Understand Helm chart structure |
| [Lab 05 - Helm Values](labs/lab-05-helm-values/README.md) | Work with `values.yaml` and override values |
| [Lab 06 - Helm Templates](labs/lab-06-helm-templates/README.md) | Understand and render Helm templates |
| [Lab 07 - Helm Template Functions](labs/lab-07-helm-template-functions/README.md) | Use Helm template functions |
| [Lab 08 - Helm Conditionals](labs/lab-08-helm-conditionals/README.md) | Use `if` and `else` conditions |
| [Lab 09 - Helm Loops](labs/lab-09-helm-loops/README.md) | Use `range` loops in Helm |
| [Lab 10 - Helm Helper Templates](labs/lab-10-helm-helper-templates/README.md) | Create and use helper templates |
| [Lab 11 - Helm Dependencies](labs/lab-11-helm-dependencies/README.md) | Manage Helm chart dependencies |
| [Lab 12 - Helm Upgrade and Rollback](labs/lab-12-helm-upgrade-rollback/README.md) | Upgrade and rollback Helm releases |
| [Lab 13 - Helm Package and Repository](labs/lab-13-helm-package-repository/README.md) | Package charts and create a Helm repository |
| [Lab 14 - Helm Lint and Package](labs/lab-14-helm-lint-package/README.md) | Validate and package Helm charts |
| [Lab 15 - Helm Final Project](labs/lab-15-helm-final-project/README.md) | Complete Helm project |

---

## Learning Path

```text
Lab 01
   ↓
Lab 02
   ↓
Lab 03
   ↓
Lab 04
   ↓
Lab 05
   ↓
Lab 06
   ↓
Lab 07
   ↓
Lab 08
   ↓
Lab 09
   ↓
Lab 10
   ↓
Lab 11
   ↓
Lab 12
   ↓
Lab 13
   ↓
Lab 14
   ↓
Lab 15
```

## Helm Topics Covered

```text
Helm Installation
       ↓
Helm Repositories
       ↓
Helm Charts
       ↓
Chart Structure
       ↓
values.yaml
       ↓
Templates
       ↓
Template Functions
       ↓
Conditionals
       ↓
Loops
       ↓
Helper Templates
       ↓
Dependencies
       ↓
Upgrade & Rollback
       ↓
Package & Repository
       ↓
Lint & Package
       ↓
Final Project
```

## Repository Structure

```text
helm-labs/
│
├── README.md
│
└── labs/
    │
    ├── lab-01-helm-installation/
    │   └── README.md
    │
    ├── lab-02-helm-repositories/
    │   └── README.md
    │
    ├── lab-03-install-helm-chart/
    │   └── README.md
    │
    ├── lab-04-helm-chart-structure/
    │   └── README.md
    │
    ├── lab-05-helm-values/
    │   └── README.md
    │
    ├── lab-06-helm-templates/
    │   └── README.md
    │
    ├── lab-07-helm-template-functions/
    │   └── README.md
    │
    ├── lab-08-helm-conditionals/
    │   └── README.md
    │
    ├── lab-09-helm-loops/
    │   └── README.md
    │
    ├── lab-10-helm-helper-templates/
    │   └── README.md
    │
    ├── lab-11-helm-dependencies/
    │   └── README.md
    │
    ├── lab-12-helm-upgrade-rollback/
    │   └── README.md
    │
    ├── lab-13-helm-package-repository/
    │   └── README.md
    │
    ├── lab-14-helm-lint-package/
    │   └── README.md
    │
    └── lab-15-helm-final-project/
        └── README.md
```

## Prerequisites

Before starting the labs, make sure you have:

```bash
kubectl version --client
```

```bash
helm version
```

```bash
kubectl get nodes
```

A running Kubernetes cluster is required for the labs that install Helm charts.

## Start Learning

Start here:

**[Lab 01 - Helm Installation](labs/lab-01-helm-installation/README.md)**

Then continue through the labs in order.
