<h1>Kubernetes Volumes, PV and PVCS</h1>

<h3>Volumes</h3>

It is just an **object** (just like Deployment, Service etc) that allows the **container** to store the data at the **POD** level. That's it! 

<p align="center"><img src="https://github.com/karankumarshreds/KubernetesMicroservices/blob/master/img/volume.PNG"/></p>

**Disadvantage**: IF the **pod dies**, there is no persistency of the data which was stored in that **volume saved in the pod**. So the container will lose all of it's data.
To overcome this, we make use of the **Persistent Volume**.

<h3>Persistent Volumes</h3>

```javascript
$ kubectl get pv
// this lists out all the persistent volumes configured
$ kubectl get pvc 
// lists out all the pvc's which are currently in use 
```

It is similar to volumes but these are tied to the NODE level(by default, it can be changed though). These are **NOT** tied to any specific pod or any specific container.

So doesn't matter if the *container* dies or the *pod* dies, the data **will** be persisted.

<h3>Persistent Volume Claim</h3>

It is a config which describes the **way** how the Persitent Volume will be mounted.

It refers a configuration of the type/size of volume that can be used by any container.
The configuration can be called from inside the deployment configuration.
It is always a good idea to configure the PVC first separately so that it is later on easy for us to change it's configuration globally from one single place.

These can have differenct access modes: 

1. ReadWriteOnce : The volume can be mounted with read-write by a single node.
2. ReadOnlyMany  : The volume can be mounted with read only by a multiple nodes at the same time.
3. ReadWriteMany : The volume can be mounted with read-write by multiple nodes at the same time.

*Command to check the storage volume for your kubernetes volume in your environment:*

```javascript
$ kubectl get storageclass 
// default option is: 'standard'
$ kubectl describe storageclass
```
<h4>Storage options in cloud environment:</h4>

- Google cloud persistent disk 
- Azure file 
- Azure disk 
- AWS EBS 

If you don't specify the **storageClassName** in your PVC config file, the default storage option will be provided to the user for the Persistent Volume.

More on storage options: https://kubernetes.io/docs/concepts/storage/volumes/#hostpath