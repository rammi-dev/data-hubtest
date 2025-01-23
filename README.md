# Create a Kind cluster
kind create cluster --config data-hub-kind-cluster.yaml

`kubectl create namespace datahub`

# Add the DataHub Helm chart repository
`helm repo add datahub https://datahub-project.github.io/charts/` 
`helm repo update`

# Install DataHub with Helm

`kubectl create secret generic mysql-secrets --from-literal=mysql-root-password=datahub`
`kubectl create secret generic neo4j-secrets --from-literal=neo4j-password=datahub`
`kubectl create secret generic datahub-users-secret --from-file=user.props=./config/user.props`



`helm install prerequisites datahub/datahub-prerequisites -f values-prerequisites.yaml --namespace datahub`

`kubectl wait --for=condition=ready pod --all --namespace datahub --timeout=1m`

`helm install datahub datahub/datahub -f values-datahub.yaml --namespace datahub`

# Port-forward DataHub frontend
`kubectl port-forward svc/datahub-datahub-frontend 10002:9002`

# Clean env
`helm uninstall datahub --namespace datahub`
`helm uninstall prerequisites --namespace datahub`

`kubectl delete all --all  --grace-period=30 --namespace datahub`

`kind delete cluster --name datahub-cluster`

# If you want to update 
`helm upgrade datahub datahub/datahub --values <path_to_values.yaml>`

