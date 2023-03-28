# Common K8s Errors

## CreateContainerConfigError

This error occurs when Kubernetes tries to create a container in a pod and fails before the container could enter the running state.

This error can be caused if the ConfigMap or Secret of the container is missing or is not provided.

- **ConfigMaps:** These are Kuberenets objects that let the user store configs for other objects to use. It is a directory of key-value pairs and is helpful storing data that is not encrypted.

- **Missing Secrets:** Kubernetes Secrets are secure information about the container such as passwords, Auth tokens, and SSH keys. If Kubernetes fails to create or find the secret the deployment will fail.

You can start debugging the cause of the error by running `kubectl describe pod <pod name> -n <namespace>` If it gives you an error like `Secret not found` or `ConfigMap not found` then run `kubectl get configmap/secret -n <namespace>`to learn more about the error.

## CreateContainerError

This error occurs when Kubernets tries to create a container but fsils, There are a couple of reasons why this issue can happen.

- **Container runtime:** The container runtime can cause issues with the container not being able to be created. Cleaning up old containers, changing the container name, and changing the kubelet logs are ways to debug this error.

- **Container not starting up:** There could be issues with the startup command, the application inside the container or the image not being available to create the container. These are all common issues as to why the container is not being created.

## FailedScheduling

This is an error that occurs when a pod fails to schedule.

There are a few reasons why this issue can occur.

- **Not enough resources:** The Kubernetes scheduler will try to find a node that can allocate enough resources to meet the pod's requirements. If this doesn't happen the pod will stay in the pending state untill resources are available.

- **Unshcedulable node** If you cardon a node then Kubernetes will mark that node as unschedulable. This is done mostly when a node requires a reboot or maintenance.

- **Taints:** You can add taints to nodes and tolerations to pods in Kubernetes for scheduling descisions. These work together to make sure each pod is scheduled at the right node.

- **Lables:** If you want to specify which node the pod deploys to then you can use the `nodeSelector` to specify constraints. It works using a key-value pair system and both the node and the pod must have the same key-value pair for the deployment.

## NonZeroExitCode

This error occurs when a containers exists with a non-healthy exit code. There are different exit codes that each tell you the exact reason why the container stopped working.

- **Exit-code-1:** This code means that the application inside the container stooped working or there are problems with the container image.

- **Exit-code-127:** This code indicates that one of the command specified to the container does not exist in the container image.

There are many other types of exit-codes that each have a meaning of their own. You can use the command `kubectl get pod <pod name> -n <namespace> -o json | grep exitCode` to find more info about the exit code. 

>  Komodor the one's that these notes are referenced from provide a useful list about Kubernetes exit codes that is available [here](https://komodor.com/learn/exit-codes-in-containers-and-kubernetes-the-complete-guide/)

## OOMKilled

This error occurs when a pod reaches it's memory limit and gets killed. The killed pods status will be defined as OOMKilled.

This is error is pretty easy to configure, you just need to check the memory limits were changed or not configured for the pod.

Set the memory limits according to your needs and then restart the pod.

#DevOps #Kubernetes
