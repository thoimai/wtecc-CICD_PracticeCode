# Build a Docker image and push it to the image registry in OpenShift.

1. Know how to modify the pipeline to reference the task as a clusterTask 
    * Config its parameters 
2. Know how to pass additional parameters to a pipeline 
3. Know how to run the pipeline to build an image 
4. Know how to push the image to an image registry in OpenShift 


## Prerequisites



## Start the Pipeline 

```shell
tkn pipeline start cd-pipeline \
    -p repo-url="https://github.com/thoimai/wtecc-CICD_PracticeCode.git" \
    -p branch=main \
    -p build-image=image-registry.openshift-image-registry.svc:5000/$SN_ICR_NAMESPACE/tekton-lab:0.1 \
    -w name=pipeline-workspace,claimName=pipelinerun-pvc \
    --showlog
```
