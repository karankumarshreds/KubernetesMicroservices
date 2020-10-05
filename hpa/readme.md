# Horizontal Pod Autoscaler 

The Horizontal Pod Autoscaler automatically scales the number of Pods in a replication controller, deployment, replica set or stateful set based on CPU utilization limit set by the administrator.

### How does the Horizontal Pod Autoscaler work? 

<p align="center"><img src="https://github.com/karankumarshreds/KubernetesMicroservices/blob/master/img/hpa.PNG" width="500"/></p>

The Horizontal Pod Autoscaler is implemented as a control loop, with a period controlled by the ***controller manager's*** ```--horizontal-pod-autoscaler-sync-period flag``` (with a default value of 15 seconds).

During each period, the controller manager queries the resource utilization of the pod against the value(limit) set by us.
The data is fetched either by : 

- Resource metrics API (default)
- Custom metrics API (like ```kube state metrics```)

#### NOTE: 
If some of the Pod's containers do not have the relevant resource request set, CPU utilization for the Pod will not be defined and the autoscaler will not take any action for that. **Which means you have to set resource limit on container level.**

**Also make sure you have metrics server configured**

## Commands 

We can use following commands to create , list and delete the hpa : 

```
$ kubectl create 
$ kubectl get hpa 
$ kubectl describe hpa 
```

There is a special command ```kubectl autoscale``` for easy creation of Horizontal Pod Autoscaler.

### Usage: 

Suppose we want to autoscale a deployment called ```client-depl``` : 

```
$ kubectl autoscale deployment client-depl --cpu-percent=50 --min=1 --max=10
```

#### PERCENT? 

Here, the percentage is of the "request" of the container. Suppose the container has requested 400m CPU, and we scale it at 80%, it would mean *as soon as the pod reaches 80% of the 400m, HPA will kick in and it will scale up*.

**You dont really need to put limit in resources in order to work with autoscaling, BUT YOU DO NEED TO MENTION 'REQUESTS'**:

#### BECAUSE : 

If the autoscaler starts scaling up, it needs to know *how much resource (cpu)* does the pod actually needs. So depending on the amount of the *resources left on the node* will the autoscaler scale up. If there are no resources left on the node, autoscaler will stop autoscaling at that point.



