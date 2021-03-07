# Kubelet

*The kubelet is responsible for maintaining a set of pods.*
*Kubelet is the primary "node agent" that runs on each node*


Kubelet understands only one language, and that is `podspec` which defines the definition/configuration of the pod.

It keeps watching for ```podspec``` requests coming in via **Kubernetes API server**

`podspec` is defined in `yaml` || `json`.

<br />
The **job** of kubelet is to make sure that the containers described in those PodSpecs are running and healthy.

## NOTE:

The kubelet doesn't manage containers which were not created by Kubernetes.
