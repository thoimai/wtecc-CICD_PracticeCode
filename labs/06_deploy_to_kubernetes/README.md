# Deploy to Kubernetes / OpenShift

# Task 1. Create a Docker image and store the Dockerfile

## 1. Build image 

docker build -t my_python_app:v1 .

## 2. Run Image 

docker run -p 4000:80 --name thoi-app demo-app:v1


* Configure authentication 

`gcloud auth configure-docker us-central1-docker.pkg.dev`

`us-central1`: region of the repo 

+ Push the container to Artifact Registry


* Build a newer image 
`docker build -t us-central1-docker.pkg.dev/$PROJECT_ID/thoi-repo/thoi-app:v2 .`


## Start the Pipeline 

```shell
tkn pipeline start cd-pipeline \
    -p repo-url="https://github.com/thoimai/wtecc-CICD_PracticeCode.git" \
    -p branch=main \
    -p app-name=hitcounter \
    -p build-image=image-registry.openshift-image-registry.svc:5000/$SN_ICR_NAMESPACE/tekton-lab:latest \
    -w name=pipeline-workspace,claimName=pipelinerun-pvc \
    --showlog
```
