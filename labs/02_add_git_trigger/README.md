# Set up Tekton Triggers

## visualize how Trigger works in Tekton

Here is a diagram that visualizes the interconnection between the EventListener, Trigger, TriggerBinding, and TriggerTemplate in Tekton:

![Diagram](https://showme.redstarplugin.com/d/s5CJsTeI)

- The EventListener triggers a Trigger when an event is received.
- The Trigger refers to a TriggerBinding and a TriggerTemplate.
- The TriggerBinding extracts data from the event payload and stores it as parameters.
- The TriggerTemplate uses these parameters and specifies the PipelineRun or TaskRun to be executed.

[You can view this diagram in a new tab.](https://showme.redstarplugin.com/d/s5CJsTeI)

[You can edit this diagram online if you want to make any changes.](https://showme.redstarplugin.com/s/c2mkRQ1X)

The type of the diagram is a graph in Mermaid language. 

To view ideas for improving the diagram, use the key phrase "*show ideas*"

To view other types of diagram and languages, use the key phrase "*explore diagrams*"

---

## How to create a Github Trigger event

After creating an EventListener, the next step is to create a TriggerBinding and a TriggerTemplate. These two components work together to define how the event data should be mapped to your pipeline parameters and what resources should be created when an event is received.

Here's a step-by-step guide:

1. **Create a TriggerBinding**: This will extract data from the event payload and store it as parameters.

```yaml
apiVersion: triggers.tekton.dev/v1alpha1
kind: TriggerBinding
metadata:
  name: my-triggerbinding
spec:
  params:
    - name: gitrevision
      value: $(body.head_commit.id)
    - name: gitrepositoryurl
      value: $(body.repository.url)
```

In this example, `my-triggerbinding` is the name of the TriggerBinding. It extracts the `gitrevision` and `gitrepositoryurl` from the event payload.

2. **Create a TriggerTemplate**: This will specify what Pipeline or Task to run and how to map the parameters to the Pipeline or Task.
 + The TriggerTemplate takes the parameters passed in from the TriggerBinding and creates a PipelineRun to start the pipeline.

```yaml
apiVersion: triggers.tekton.dev/v1alpha1
kind: TriggerTemplate
metadata:
  name: my-triggertemplate
spec:
  params:
    - name: gitrevision
      description: The git revision
      default: master
    - name: gitrepositoryurl
      description: The git repository url
  resourcetemplates:
    - apiVersion: tekton.dev/v1beta1
      kind: PipelineRun
      metadata:
        generateName: my-pipeline-run-
      spec:
        pipelineRef:
          name: my-pipeline
        params:
          - name: gitrevision
            value: $(tt.params.gitrevision)
          - name: gitrepositoryurl
            value: $(tt.params.gitrepositoryurl)
```

In this example, `my-triggertemplate` is the name of the TriggerTemplate. It defines a PipelineRun for `my-pipeline` and maps the `gitrevision` and `gitrepositoryurl` parameters to the pipeline parameters.

3. **Update the EventListener**: Finally, update your EventListener to reference the TriggerBinding and TriggerTemplate.

```yaml
apiVersion: triggers.tekton.dev/v1alpha1
kind: EventListener
metadata:
  name: my-eventlistener
spec:
  serviceAccountName: tekton-triggers-example-sa
  triggers:
    - name: my-trigger
      bindings:
        - ref: my-triggerbinding
      template:
        ref: my-triggertemplate
```

In this example, `my-trigger` is the name of the trigger. It references `my-triggerbinding` and `my-triggertemplate`.

Remember to apply these yaml files with `kubectl apply -f <filename.yaml>` command.

For more details, you can refer to the [official Tekton Triggers documentation](https://tekton.dev/docs/triggers/triggerbindings/) and [TriggerTemplates](https://tekton.dev/docs/triggers/triggertemplates/).

## Start a pipeline run 

* List all running services in k8s cluster 

```log
[theia: 02_add_git_trigger]$ kubectl get services 
NAME                    TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)             AGE
el-cd-listener          ClusterIP   172.21.31.158   <none>        8080/TCP,9000/TCP   43m
openshift-web-console   ClusterIP   172.21.133.39   <none>        8000/TCP            96m
```

* Pipeline running logs 

```log
[theia: project]$ tkn pipelinerun ls
NAME                    STARTED          DURATION   STATUS
cd-pipeline-run-d4d7n   57 seconds ago   31s        Succeeded
[theia: project]$ tkn pipelinerun logs --last
[clone : checkout] Cloning into 'wtecc-CICD_PracticeCode'...

[lint : echo-message] Calling Flake8 linter...

[tests : echo-message] Running unit tests with PyUnit...

[build : echo-message] Building image for https://github.com/thoimai/wtecc-CICD_PracticeCode ...

[deploy : echo-message] Deploying main branch of https://github.com/thoimai/wtecc-CICD_PracticeCode ...
```

# Take-away lessions: 
+ Learn to create a Tekton trigger to cause a pipeline run from external events like changes made to a repo in Github 
+ Know how to create EventListener, TriggerTemplates, TriggerBindings 
+ Know how to start a pipeline run on a port 

