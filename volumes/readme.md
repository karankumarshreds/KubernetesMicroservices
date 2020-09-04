<h1>Kubernetes Volumes, PV and PVCS</h1>

<h3>Volumes</h3>

It is just an **object** (just like Deployment, Service etc) that allows the **container** to store the data at the **POD** level. That's it! 

**Disadvantage**: IF the **pod dies**, there is no persistency of the data which was stored in that **volume saved in the pod**. So the container will lose all of it's data.
To overcome this, we make use of the **Persistent Volume**.


