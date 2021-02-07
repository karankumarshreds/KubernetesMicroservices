# Kubelet

Kubelet understands only one language, and that is `podspec` which defines the definition/configuration of the pod.

`podspec` is defined in `yaml` || `json`.

<br />
The **job** of kubelet is to make sure that the containers described in those PodSpecs are running and healthy.

## NOTE:

The kubelet doesn't manage containers which were not created by Kubernetes.
