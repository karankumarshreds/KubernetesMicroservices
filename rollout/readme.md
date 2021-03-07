# Rolling Updates

Ideal way of performing a rolling update it the `declarative approach`.

Consider the following example:

```yml
# ** random-depl.yaml **
apiVersion: apps/v1
kind: Deployment
metadata:
  app: random
spec:
  replicas: 3 # this is necessary
  # fun starts here
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1 # ==> A
      maxUnavailable: 1 # ==> B
    selector:
      matchLabels:
        app: random
    # pod
    template:
      metadata:
        labels:
          app: random
      spec:
        containers:
          - image: nginx
            name: nginx
            readinessProbe: # ==> C
              exec:
                command: ['stat', '/some_script']
```

**To trigger the update**:

> kubectl patch -f random-depl.yaml

**To force the update:**

> kubeclt replace -f random-depl.yaml

**Explanation**:

- **A**: The number of _extra_ pods that can run temporarily to perform the rolling update. This means, in our example, _one new pod will be made, and one pod will be killed post that, which means maximum **4 pods** can run at a time_.

- **B**: Out of the 3 old pods, how many can be unavailable at a time during the rollout. It means, it will kill 1 pod at a time out of 3 to perform the update. _Means, 2 out of 3 pods will be available during the update_.

- **C**: Readiness probes are very important for kubernetes SVCs to know new pods are healthy and for them to start sending the traffic to new pods.

### Watching rollout status of deployment

This command can be used for watching rollout status of deployments.

> kubectl rollout status deployment/DEPLOYMENT_NAME

### Rollout History of deployment

This command can be used for listing rollout history of the deployment.

> kubectl rollout history deployment/DEPLOYMENT_NAME

### Undo Last Rollout of deployment

This command can be used to undo last rollout of the deployment.

> kubectl rollout undo deployment/DEPLOYMENT_NAME
