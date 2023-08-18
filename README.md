# Cluster Create


    gcloud container clusters create hello-world-5uvz --zone=us-central1-a --release-channel=regular --cluster-version=1.25.6-gke.200 --      enable-autoscaling --num-nodes=3 --min-nodes=2 --max-nodes=6


# Pod Monitor


    kubectl create namespace gmp-3r1f<br>
    gsutil cp gs://spls/gsp510/prometheus-app.yaml . 
    vim prometheus-app.yaml 
    kubectl apply -f prometheus-app.yaml -n gmp-3r1f
    gsutil cp gs://spls/gsp510/pod-monitoring.yaml . 
    vim pod-monitoring.yaml 
    kubectl apply -f pod-monitoring.yaml -n gmp-3r1f 


# Deploy

    gsutil cp -r gs://spls/gsp510/hello-app/ .
    cd hello-app/manifests
    kubectl apply -f helloweb-deployment.yaml -n gmp-3r1f 
    kubectl get deployments -n gmp-3r1f
    kubectl get pods -n gmp-3r1f


# Alert
    jsonPayload.message:"Failed to apply default image tag"
    severity=WARNING
    resource.type="k8s_pod"

# Docker Loadbalancer

    gcloud auth configure-docker us-central1-docker.pkg.dev
    export PROJECT_ID=$(gcloud config get-value project)
    docker build -t us-central1-docker.pkg.dev/$PROJECT_ID/sandbox-repo/hello-app:v2 .
    docker push us-central1-docker.pkg.dev/$PROJECT_ID/sandbox-repo/hello-app:v2
    kubectl create deployment helloweb --image=us-central1-docker.pkg.dev/$PROJECT_ID/sandbox-repo/hello-app:v2
    kubectl set image deployment/helloweb hello-app=us-central1-docker.pkg.dev/$PROJECT_ID/sandbox-repo/hello-app:v2 -n gmp-3r1f
    kubectl expose deployment helloweb -n gmp-3r1f --type=LoadBalancer --port=80 --target-port=8080 --name=helloweb-service-34ti





