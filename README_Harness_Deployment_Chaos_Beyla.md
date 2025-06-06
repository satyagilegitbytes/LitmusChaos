
# 1. Install Prometheus Node Exporter on Minikube Cluster

    helm repo add prometheus-community https://prometheus-community.github.io/helm-charts 
    helm repo update
    helm upgrade --install node-exporter prometheus-community/prometheus-node-exporter

# 2. Configure and Run Alloy on Minikube Cluster

Make sure to update the node_exporter service ip in alloy configuration in configmap.alloy file before creating configmap

    kubectl create configmap --namespace metrics alloy-config "--from-file=config_k8s_metrics_logs_tracing.alloy=./config_k8s_metrics_logs_tracing.alloy"

    helm  upgrade --install --namespace metrics alloy grafana/alloy -f alloy_k8s_app_monitoring.yaml

# 3. Install and Configure Grafana Beyla on cluster for Auto Instrumentation

    helm repo add grafana https://grafana.github.io/helm-charts
    helm upgrade --install beyla grafana/beyla -f helm-beyla.yaml

# 4. Validate the metrics on Grafana
To Search all of the time series data points grouping by job  in query  

    count({__name__=~".+"}) by (job)

# 5. Add the Beyla Dashboard  

Add the below dashboard id
    19923