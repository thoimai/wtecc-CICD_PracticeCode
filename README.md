# Contents

- Lab 1: [Build an empty Pipeline](labs/01_base_pipeline/README.md)
- Lab 2: [Adding GitHub Triggers](labs/02_add_git_trigger/README.md)
- Lab 3: [Use Tekton CD Catalog](labs/03_use_tekton_catalog/README.md)
- Lab 4: [Integrate Unit Test Automation](labs/04_unit_test_automation/README.md)
- Lab 5: [Building an Image](labs/05_build_an_image/README.md)
- Lab 6: [Deploy to Kubernetes](labs/06_deploy_to_kubernetes/README.md)

# Basic Commands

+ Shorten the terminal 

export PS1="[\[\033[01;32m\]\u\[\033[00m\]: \[\033[01;34m\]\W\[\033[00m\]]\$ "

+ Apply pipeline to the cluster:

`kubectl apply -f pipeline.yaml`

+ Apply the new task definition to the cluster:

`kubectl apply -f tasks.yaml`
    
+ Run the cd-pipeline 

```shell 
tkn pipeline start cd-pipeline \
    --showlog \
    -p repo-url="https://github.com/ibm-developer-skills-network/wtecc-CICD_PracticeCode.git" \
    -p branch="main"
``` 

# Set up a Tekton Trigger 

https://github.com/thoimai/wtecc-CICD_PracticeCode/blob/main/labs/02_add_git_trigger/README.md
