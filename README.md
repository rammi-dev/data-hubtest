# Create a Kind cluster
kind create cluster --config data-hub-kind-cluster.yaml

<pre><code>kubectl create namespace datahub</code></pre>

# Add the DataHub Helm chart repository
<pre><code>helm repo add datahub https://datahub-project.github.io/charts/</pre></code> 
<pre><code>helm repo update</pre></code>

# Install DataHub with Helm

<pre><code>kubectl create secret generic mysql-secrets --from-literal=mysql-root-password=datahub</pre></code>
<pre><code>kubectl create secret generic neo4j-secrets --from-literal=neo4j-password=datahub</pre></code>
<pre><code>kubectl create secret generic datahub-users-secret --from-file=user.props=./config/user.props</pre></code>



<pre><code>helm install prerequisites datahub/datahub-prerequisites -f values-prerequisites.yaml --namespace datahub</pre></code>

<pre><code>kubectl wait --for=condition=ready pod --all --namespace datahub --timeout=1m</pre></code>

<pre><code>helm install datahub datahub/datahub -f values-datahub.yaml --namespace datahub</pre></code>

# Port-forward DataHub frontend
<pre><code>kubectl port-forward svc/datahub-datahub-frontend 10002:9002</pre></code>

# Clean env
<pre><code>helm uninstall datahub --namespace datahub</pre></code>
<pre><code>helm uninstall prerequisites --namespace datahub</pre></code>

<pre><code>kubectl delete all --all  --grace-period=30 --namespace datahub</pre></code>

<pre><code>kind delete cluster --name datahub-cluster</pre></code>

# If you want to update 
<pre><code>helm upgrade datahub datahub/datahub --values <path_to_values.yaml></pre></code>

