# Liveness, Readiness and Startup Probes

## Readiness Probes

Suppose the horizontal pod autoscaler kicks in as soon as there is high load on the running pod.
<br />
The HPA makes a new pod and **AS SOON AS THE CONTAINER STARTS**, the SERVICE attached to the deployment thinks/believes that the
container is ready to serve.
<br />
**Which is not true, because no application starts instantaneously**. This will cause issues like `timeout` to the client accessing the service
from the BROWSER or an APP.

So we need some sort of mechanism to add a `delay` before the pod is considered to be **ready**.
This is done via **Readiness Probes**

## Liveliness Probes

Suppose your container is stuck while processing a request from the client. There is a bug which makes the code go in an infinite loop or is unable to process the request.

Here, we use `liveliness probe`, which helps us to catch **such deadlocks** in the application. We can use these to restart the pod to kill the hung process and make it available for other requests.

This will _not hinder the application despite the bugs_ by at least making it available for the other users.