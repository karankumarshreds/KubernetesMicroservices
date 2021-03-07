# Rolling Updates

Ideal way of performing a rolling update it the `declarative approach`.

Consider the following example:

```yml
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
      maxSurge: 1 # <A>
      maxUnavailable: 1 # <B>
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
            readinessProbe: # <C>
              exec:
                command: ['stat', '/some_script']
```

### Watching rollout status of deployment

This command can be used for watching rollout status of deployments.

> kubectl rollout status deployment/DEPLOYMENT_NAME

### Rollout History of deployment

This command can be used for listing rollout history of the deployment.

> kubectl rollout history deployment/DEPLOYMENT_NAME

### Undo Last Rollout of deployment

This command can be used to undo last rollout of the deployment.

> kubectl rollout undo deployment/DEPLOYMENT_NAME
