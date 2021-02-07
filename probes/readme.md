# Liveness, Readiness and Startup Probes

<p align="center"><img src="https://github.com/karankumarshreds/KubernetesMicroservices/blob/master/img/probes.PNG"/></p>

## Readiness Probes (mostly used)

Suppose the horizontal pod autoscaler kicks in as soon as there is high load on the running pod.
<br />
The HPA makes a new pod and **AS SOON AS THE CONTAINER STARTS**, the SERVICE attached to the deployment thinks/believes that the
container is ready to serve.
<br />
**Which is not true, because no application starts instantaneously**. This will cause issues like `timeout` to the client accessing the service
from the BROWSER or an APP.

So we need some sort of mechanism to add a `delay` before the pod is considered to be **ready**.
This is done via **Readiness Probes**

Configuration:

In your deployment configuration, inside the containers block:

```yaml
readinessProbe:
  exec:
    command:
      - cat
      - /tmp/healthy
  initialDelaySeconds: 5 ## waits 5 seconds first time
  periodSeconds: 5 ## sends requests in periods
```

This will run the command and mark it ready as soon as it runs successfully.

**BETTER WAY OF DOING APPLICATION LEVEL TEST**

```yaml
# send an internal request to the application
readinessProbe:
  httpGet:
    path: /api/currentuser
    port: 3000
  initialDelaySeconds: 10
  periodSeconds: 5
```

## Liveliness Probes

Suppose your container is stuck while processing a request from the client. There is a bug which makes the code go in an infinite loop or is unable to process the request.

Here, we use `liveliness probe`, which helps us to catch **such deadlocks** in the application. We can use these to restart the pod to kill the hung process and make it available for other requests.

This will _not hinder the application despite the bugs_ by at least making it available for the other users.

## Startup Probes

As the name suggests, the kubelet uses startup probes **to know when the application has started**.
<br />
It disables liveness and readiness checks until it succeeds, making sure those probes don't interfere with the application startup.
This means that **liveliness probe would NOT interfere and tell kubelet to restart the pod until the check for startup probe has been completed**
