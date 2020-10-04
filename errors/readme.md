# Error handling 

## --namespaces

- To check which resources are terminating after deleting a namescpace

```
$ kubectl api-resources --verbs=list --namespaced -o name | xargs -n 1 kubectl get --show-kind --ignore-not-found -n monitoring
```

- To forcefully delete the resources(like pods) stuck in terminating space 

```
$ kubectl delete pod <PODNAME> --grace-period=0 --force --namespace <NAMESPACE>
```