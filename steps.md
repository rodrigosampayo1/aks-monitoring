# To connect to your subscription and Tenant
az login --tenant ff5a6044-ffb8-488b-b31b-b039ef5df0d7

az account set -subscription 514cd396-f281-41a1-b376-6d348b606151

# AKS
#To merge AKS cluster to our context in ~/.kube/config
az aks get-credentials --resource-group rg-aks-001 --name Cluster-aks-rs-001

# List all deployments in all namespaces
kubectl get deployments --all-namespaces=true

# List all deployments in a specific namespace
# Format :kubectl get deployments --namespace <namespace-name>
kubectl get deployments --namespace kube-system
kubectl get deployments -n kube-system

# List details about a specific deployment
# Format :kubectl describe deployment <deployment-name> --namespace <namespace-name>
kubectl describe deployment my-dep --namespace kube-system