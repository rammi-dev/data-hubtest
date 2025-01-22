# Create a Kind cluster
kind create cluster --config kind-config.yaml

kubectl create namespace datahub

# Add the DataHub Helm chart repository
helm repo add datahub https://datahub-project.github.io/charts/ 
helm repo update

# Create a custom values.yaml file for DataHub (optional)
cat <<EOL > values.yaml
backend:
  elasticsearch:
    enabled: false
  kafka:
    enabled: true
frontend:
  enabled: true
  replicas: 1
EOL

# Install DataHub with Helm
helm install datahub datahub/datahub -f values.yaml --namespace datahub

# Port-forward DataHub frontend
kubectl port-forward svc/datahub-frontend 9001:9001


helm uninstall datahub