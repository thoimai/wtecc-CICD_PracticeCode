
# Mounted as a filesystem explained in plain english 

Sure, I'll explain it in simple terms.

When we say a volume is "mounted as a filesystem", we're talking about making the storage available for use. 

Think of it like this: you have a new external hard drive (which is like the volume in Kubernetes). Before you can start storing your photos, documents, or other data on it, you need to connect it to your computer. This connection process is similar to "mounting" in the world of computers.

So, when a volume is "mounted as a filesystem", it's like we're connecting that external hard drive to our computer (in this case, a node in the Kubernetes cluster). Once it's connected, we can start storing data on it, reading data from it, and so on.

In the case of your PersistentVolumeClaim, it's like saying: "I have this new external hard drive, and I want to connect it to my computer so I can start using it." 

I hope this helps! Let me know if you have any other questions.

---

# Run the pipeline 

```shell
tkn pipeline start cd-pipeline \
    -p repo-url="https://github.com/thoimai/wtecc-CICD_PracticeCode.git" \
    -p branch="main" \
    -w name=pipeline-workspace,claimName=pipelinerun-pvc \
    --showlog
```
+ Check the pipeline is running or not 

`tkn pipelinerun ls`

