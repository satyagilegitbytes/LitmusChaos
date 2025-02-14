# Install Litmus using kubectl
Reference Documentation: 
    https://docs.litmuschaos.io/docs/getting-started/installation#prerequisites


# 1. Install LitmusChaos via Helm  

    helm repo add litmuschaos https://litmuschaos.github.io/litmus-helm/
    helm repo list
    kubectl create ns litmus
    helm upgrade --install chaos litmuschaos/litmus --namespace=litmus --set portal.frontend.service.type=NodePort

# 2. Verify your installation  

    kubectl get pods -n litmus
    kubectl get svc -n litmus
    kubectl port-forward svc/litmusportal-frontend-service 9091:9091

    Login the Litmus GUI Portal 
    id: admin
    pass: litmus

# 3. Install the application  

    cd python_app
    kubectl apply -f .

# 4. Add the Minikube Environment in Litmus GUI  

Click on Environments
Add New Environment
Give a name and save it

    kubectl apply -f nonprod-litmus-chaos-enable.yml

# 5. Add the Chaos Experiments in Litmus GUI  

Click on New Experiment
Type Poddelete
Fill the remaining configurations
Add the http probe for the application pod.
Download the yaml and add the annotations as below:  

    annotations:
    probeRef: '[{"name":"delete","mode":"Continuous"}]'

Add the below probe under experiments  

    probe:
        - name: delete
        type: httpProbe
        httpProbe/inputs:
            url: http://single-app-single-collector.litmus.cluster.local
            insecureSkipVerufy: false
            method:
            get:
                criteria: "=="
                responseCode: "200"
        mode: Continuous

After modification Run the below command to trigger the Pod delete operation.  

    kubectl apply -f poddelete_3.yml

# 6. Verify the details on Cluster and in Litmus GUI  
