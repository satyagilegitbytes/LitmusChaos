# Install Litmus using kubectl
Reference Documentation: 
    https://docs.litmuschaos.io/docs/getting-started/installation#prerequisites


# 1. Install the application  

    cd python_app
    kubectl apply -f .

# 2. Install and Configure Harness with EKS cluster

    kubectl create namespace hce
    kubectl apply -f https://raw.githubusercontent.com/chaosnative/hce-charts/main/hce-saas/hce-saas-crds.yaml
    kubectl apply -f eksnonprod-harness-chaos-enable.yml

# 3. Configure and Run Alloy on Minikube Cluster

Make sure to update the node_exporter service ip in alloy configuration in configmap.alloy file before creating configmap

    kubectl create configmap --namespace litmus alloy-config "--from-file=config_k8s_metrics_logs_tracing.alloy=./config_k8s_metrics_logs_tracing.alloy"

    helm  upgrade --install --namespace litmus alloy grafana/alloy -f alloy_k8s_app_monitoring.yaml