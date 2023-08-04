# Debug Tekton Pipeline
If you have trouble running your pipeline, you might want to investigate the logs for the pipeline and its tasks. Tekton provides several ways to view these logs.

Here's how you can get logs and debug the issue:

1. **Use the `tkn` CLI tool:** 

- View the logs for the most recent PipelineRun:

    ```
    tkn pipelinerun logs -f
    ```

- View the logs for a specific PipelineRun:

    ```
    tkn pipelinerun logs <pipelinerun-name> -f
    ```

- View the logs for a specific task within a PipelineRun:

    ```
    tkn pipelinerun logs <pipelinerun-name> -t <task-name>
    ```

2. **Use `kubectl` to inspect the pods:**

- Get the list of pods involved in the pipeline:

    ```
    kubectl get pods
    ```

- Check the logs of a specific pod:

    ```
    kubectl logs <pod-name>
    ```

- If a pod has multiple containers (which is likely as each step in a task is a different container), you can check the logs of a specific container:

    ```
    kubectl logs <pod-name> -c <container-name>
    ```

3. **Check the Tekton Dashboard:** If you have the Tekton Dashboard installed, you can use it to view the logs of your PipelineRuns and TaskRuns.

4. **Describe the PipelineRun or TaskRun:** The `describe` command gives you the details of the run and its events which might give a clue about the failure:

    ```
    tkn pipelinerun describe <pipelinerun-name>
    ```

    ```
    tkn taskrun describe <taskrun-name>
    ```

5. **Check Events:** You can check events related to a particular PipelineRun or TaskRun using `kubectl`:

    ```
    kubectl describe pipelinerun <pipelinerun-name>
    ```

    ```
    kubectl describe taskrun <taskrun-name>
    ```

Remember to replace `<pipelinerun-name>`, `<taskrun-name>`, `<pod-name>`, and `<container-name>` with your actual run, pod, and container names.

You can start by looking at the logs for the entire PipelineRun, and if the error is not clear from those logs, you can examine the logs for each TaskRun or pod involved in the pipeline execution. The error messages from these logs will guide you to the specific issue, which could be a configuration error, a problem with the resources, or other issues.

---

In Tekton, you can use the `tkn` command-line tool to list all `PipelineRuns` and `TaskRuns`. Here's how you can do it:

**To list all PipelineRuns:**

```shell
tkn pipelinerun list
```

This command lists all `PipelineRuns` in the default namespace. If you want to list `PipelineRuns` in a specific namespace, you can specify the namespace with the `-n` or `--namespace` option:

```shell
tkn pipelinerun list -n <namespace>
```

Replace `<namespace>` with your actual namespace.

**To list all TaskRuns:**

```shell
tkn taskrun list
```

This command lists all `TaskRuns` in the default namespace. If you want to list `TaskRuns` in a specific namespace, you can specify the namespace with the `-n` or `--namespace` option:

```shell
tkn taskrun list -n <namespace>
```

Replace `<namespace>` with your actual namespace.

These commands will display a list of `PipelineRuns` or `TaskRuns`, along with their status and age. This can be particularly useful when you're debugging or inspecting the state of your Tekton pipelines.