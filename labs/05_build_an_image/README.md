# Build a Docker image and push it to the image registry in OpenShift.

1. Know how to modify the pipeline to reference the task as a clusterTask 
    * Config its parameters 
2. Know how to pass additional parameters to a pipeline 
3. Know how to run the pipeline to build an image 
4. Know how to push the image to an image registry in OpenShift 


## Prerequisites



## Step 6: Apply changes and run the Pipeline 

### Apply Tasks 



### Start the Pipeline 

1. Apply the pipeline 
`kubectl apply -f pipeline.yaml`
2. Apply the PVC 
`kubectl apply -f pvc.yaml`

3. Start the pipeline 

```shell
tkn pipeline start cd-pipeline \
    -p repo-url="https://github.com/thoimai/wtecc-CICD_PracticeCode.git" \
    -p branch=main \
    -p build-image=image-registry.openshift-image-registry.svc:5000/$SN_ICR_NAMESPACE/tekton-lab:latest \
    -w name=pipeline-workspace,claimName=pipelinerun-pvc \
    --showlog
```


4. Check run status 
`tkn pipelinerun ls`

```log  
NAME                    STARTED         DURATION     STATUS
cd-pipeline-run-fbxbx   1 minute ago    59 seconds   Succeeded
```


## Debug the issue 

List all Pipelinerun 
`tkn pipelinerun list`

1. Describe the PipelineRun

```log 
[theiaopenshift-u1513319: 05_build_an_image]$ tkn pipelinerun describe cd-pipeline-run-6nzz8 
Name:                  cd-pipeline-run-6nzz8
Namespace:             sn-labs-u1513319
Pipeline Ref:          cd-pipeline
Service Account:       pipeline
Timeout(Deprecated):   1h0m0s
Labels:
 tekton.dev/pipeline=cd-pipeline

🌡️  Status

STARTED          DURATION   STATUS
20 minutes ago   50s        Failed

💌 Message

Tasks Completed: 1 (Failed: 1, Cancelled 0), Skipped: 5
TaskRun(s) cancelled: cd-pipeline-run-6nzz8-init

⚓ Params

 NAME            VALUE
 ∙ branch        main
 ∙ build-image   image-registry.openshift-image-registry.svc:5000/sn-labs-u1513319/tekton-lab:latest
 ∙ repo-url      https://github.com/thoimai/wtecc-CICD_PracticeCode.git

📂 Workspaces

 NAME                   SUB PATH   WORKSPACE BINDING
 ∙ pipeline-workspace   ---        PersistentVolumeClaim (claimName=pipelinerun-pvc)

🗂  Taskruns

 NAME                           TASK NAME   STARTED          DURATION   STATUS
 ∙ cd-pipeline-run-6nzz8-init   init        20 minutes ago   50s        Failed(TaskRunImagePullFailed)

⏭️  Skipped Tasks

 NAME
 ∙ clone
 ∙ lint
 ∙ tests
 ∙ build
 ∙ deploy
```

2. Check the run status 

`tkn pipelinerun ls`

3. Check logs of the last run 

`tkn pipelinerun logs --last`

4. Check installed tasks 

`tkn task ls`

    + Install all the required Tasks for <cd-pipeline>
        * Install git-clone task 
            `tkn hub install task git-clone`
        * Install flake8 task 
        ```shell
        tkn hub install task flake8
        kubectl apply -f tasks.yaml
        ```
    
    + Expected installed Tasks listing below: 
    ```log
    NAME               DESCRIPTION              AGE
    cleanup            This task will clean...  2 minutes ago
    git-clone          These Tasks are Git...   2 minutes ago
    flake8             This task will run ...   1 minute ago
    echo                                        46 seconds ago
    nose                                        46 seconds ago
    ```








