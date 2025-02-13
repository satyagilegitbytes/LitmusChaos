# Install Litmus using kubectl
Reference Documentation: 
    https://docs.litmuschaos.io/docs/getting-started/installation#prerequisites


# 1. Install mongo

    helm repo add bitnami https://charts.bitnami.com/bitnami
    helm install my-release bitnami/mongodb --values mongo-values.yaml -n litmus --create-namespace

# 2. Install LitmusChaos

 kubectl apply -f https://raw.githubusercontent.com/litmuschaos/litmus/master/mkdocs/docs/3.12.0/litmus-getting-started.yaml 

# 3. Verify your installation  

    kubectl get pods -n litmus
    kubectl get svc -n litmus
    kubectl port-forward svc/litmusportal-frontend-service 9091:9091

    Login the Litmus GUI Portal 
    id: admin
    pass: litmus

# 4. Install the application  

    cd python_app
    kubectl apply -f .

# 5. Add the Minikube Environment in Litmus GUI  

Click on Environments
Add New Environment
Give a name and save it

    kubectl apply -f nonprod-litmus-chaos-enable.yml

# 6. Add the Chaos Experiments in Litmus GUI  

Click on New Experiment
Type Poddelete
Fill the remaining configurations
Add the http probe for the application pod.
Download the yaml and add the annotations as below:  

    annotations:
    probeRef: '[{"name":"delete","mode":"Continuous"}]'

Run the below command to trigger the Pod delete operation.  

    kubectl apply -f pod_delete.yml

# 7. Verify the details on Cluster and in Litmus GUI  
