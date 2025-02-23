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
