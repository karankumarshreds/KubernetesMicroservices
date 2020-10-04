# Requests 

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
          cpu: 100m      # 0.1 cpu = 100m, 1/10th of the 1cpu
```
To check the resource of your node, run the following command and check the ```capacity``` block from the output.

```bash
$ kubectl get nodes 
$ kubectl describe node pool-8xrdtbleu-3al2x
```

