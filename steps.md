# To connect to your subscription and Tenant
az login --tenant ff5a6044-ffb8-488b-b31b-b039ef5df0d7

az account set -subscription 514cd396-f281-41a1-b376-6d348b606151

# AKS
#To merge AKS cluster to our context in ~/.kube/config
az aks get-credentials --resource-group XXXXX --name XXX

## AKS util commands
# List all deployments in all namespaces
kubectl get deployments --all-namespaces=true

# List all deployments in a specific namespace
# Format :kubectl get deployments --namespace <namespace-name>
kubectl get deployments --namespace kube-system
kubectl get deployments -n kube-system

# List details about a specific deployment
# Format :kubectl describe deployment <deployment-name> --namespace <namespace-name>
kubectl describe deployment my-dep --namespace kube-system

# List pods using a specific label
# Format :kubectl get pods -l <label-key>=<label-value> --all-namespaces=true
kubectl get pods -l app=nginx --all-namespaces=true

# Get logs for all pods with a specific label
# Format :kubectl logs -l <label-key>=<label-value>
kubectl logs -l app=nginx --namespace kube-system

# Get logs for all pods with a specific label
# Format :kubectl logs -l <label-key>=<label-value>
kubectl logs -l app=nginx --namespace kube-system
-------------------------------------------------------------------------
## Prometheus & Grafana instalation

# Create a namespace for Prometheus and Grafana resources
kubectl create ns prometheus
# Add prometheus and grafana repos to aks cluster:
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install prometheus prometheus-community/kube-prometheus-stack -n prometheus

# Check all resources in Prometheus Namespace
kubectl get all -n prometheus 

# TODO: if you want to delete these
kubectl delete deployment XXXX

# Port forward the Prometheus service
kubectl port-forward -n prometheus prometheus-prometheus-kube-prometheus-prometheus-0 9090

# Get the Username
kubectl get secret -n prometheus prometheus-grafana -o=jsonpath='{.data.admin-user}' |base64 -d# Get the Password
kubectl get secret -n prometheus prometheus-grafana -o=jsonpath='{.data.admin-password}' |base64 -d# Port forward the Grafana service
kubectl port-forward -n prometheus prometheus-grafana-5449b6ccc9-b4dv4 3000

# Create SP and assign role on Azure
az ad sp create-for-rbac --role="Monitoring Reader" --scopes="/subscriptions/514cd396-f281-41a1-b376-6d348b606151/resourceGroups/rg-monitoring"

{
  "appId": "bd0c47c0-65d7-4fc9-b2e9-8bf42e5e9d72",
  "displayName": "azure-cli-2023-04-27-14-59-41",
  "password": "pf~8Q~rG4y7GcdGSSz0FzdLq4TqkYeXWvsB1Ga9-",
  "tenant": "ff5a6044-ffb8-488b-b31b-b039ef5df0d7"
}
# Create a DataSource in Grafana