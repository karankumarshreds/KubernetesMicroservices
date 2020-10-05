# Requests 

Requests specify the minimum number of CPU/RAM required by a pod to run normally. This is necessary for HPA to autoscale, because in case we don't specify minimum resources needed for pod using *requests*, the HPA will not be able to check the amount of resources left on the node ```vs``` the amount of resources required by the pod.

Let us consider we have a deployment file: 

```yaml
apiVersion: apps/v1
kind: Deployment 
metadata: 
    name: client-depl 
spec: 
    replicas: 1 
    selector: 
        matchLabels: 
            app: client 
    template: 
        metadata:
            labels: 
                app: client 
        spec: 
            containers: 
                - name: client 
                  image: karanshreds/client
```
In this deployment, we can make use of something called ***resource request***.

- memory request 
- cpu request 

Let us *assume* that the pod in the above deployment needs around ```300 mb``` ram to run comfortably without crashing. 

In order to set it up, in the ```container``` block in your ```deployment``` config, add a ```resources``` block: 

```yaml
containers: 
    - name: client 
      image: karanshreds/client
      resources: 
        # defines initial request of resource for pod to run
        requests: 
          memory: 300Mi  # 1Mi = 1024Ki 1M = 1000K
          cpu: 100m      # 0.1 cpu = 100m, 1/10th of the 1cpu, 1 cpu = 1000
```
To check the resource of your node, run the following command and check the ```capacity``` block from the output.

```bash
$ kubectl get nodes 
$ kubectl describe node pool-8xrdtbleu-3al2x
```
<br />
<br />

# Limits 

It has nothing to do with HPA. 

- *memory limit* : kills and restarts the container if reaches limit 

Status that it gives when you ```describe``` that pod : **OOMKilled** << OutOfMemoryKilled

- *cpu limit* : forces the container to not request for more cpu if reaches limit 


In order to set it up, in the ```container``` block in your ```deployment``` config, add a ```resources``` block: 

```yaml
containers: 
    - name: client 
      image: karanshreds/client
      resources: 
        # defines initial request of resource for pod to run
        requests: 
          memory: 300Mi  # 1Mi = 1024Ki 1M = 1000K
          cpu: 100m      # 0.1 cpu = 100m, 1/10th of the 1cpu, 1 cpu = 1000
        # defines threshold value for hpa to start scaling 
        limits: 
          # memory limits are usually used to handle issues with memory leaks 
          memory: 500Mi # kills and restarts the container if reaches limit 
          cpu: 170m     # forces the container to not request for more cpu if reaches limit 
```