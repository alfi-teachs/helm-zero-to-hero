# Lab 02 - Helm Repositories
Objective

Understand what Helm repositories are

Add a Helm repository

Verify Helm repositories

Update Helm repository information

Search for Helm charts

View Helm chart information

### Step 1 - Check Existing Helm Repositories
```bash
helm repo list
```
Expected Output:

Error: no repositories to show
### Step 2 - Add Bitnami Helm Repository
```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
```
Expected Output:
```bash
"bitnami" has been added to your repositories
```
### Step 3 - Verify Helm Repository
```bash
helm repo list
```
Expected Output:

NAME      	URL
```bash
bitnami   	https://charts.bitnami.com/bitnami
```
### Step 4 - Update Helm Repository
```bash
helm repo update
```
Expected Output:

Successfully got an update from the "bitnami" chart repository
Update Complete.
### Step 5 - Search Helm Charts
```bash
helm search repo
```
### Step 6 - Search for NGINX
```bash
helm search repo nginx
```
### Step 7 - View NGINX Chart Information
```bash
helm show chart bitnami/nginx
```
### Step 8 - View NGINX Chart Values
```bash
helm show values bitnami/nginx
```
### Step 9 - Remove Helm Repository
```bash
helm repo remove bitnami
```
### Step 10 - Verify Repository Removal
```bash
helm repo list
```
What You Learned

Helm repositories contain Helm charts that can be searched and installed.

You learned how to:

- Add a Helm repository
- List Helm repositories
- Update a repository
- Search for charts
- View chart information
- View chart values
- Remove a repository
