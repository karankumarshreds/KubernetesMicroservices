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

