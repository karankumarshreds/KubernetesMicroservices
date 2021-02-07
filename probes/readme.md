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
