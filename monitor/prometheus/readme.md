# Prometheus Operator 

This is a prerequisite to install *Grafana*.
*Prometheus is responsible for scraping all the data/metrics of the cluster fo Grafana*.

To install prometheus on our cluster follow the below steps:

1. Go to helm chart's repository: 
https://github.com/helm/charts/tree/master/stable
2. Install prometheus operator: 
https://github.com/helm/charts/tree/master/stable/prometheus-operator
3. Now follow the below commands:

```
$ kubectl create namespace monitoring
$ helm install --name monitoring --namespace monitoring stable/prometheus-operator
$ kubectl get all -n monitoring
```
Now select "service/monitoring-prometheus-oper-prometheus"
We need to change it's default type from ClusterIP to LoadBalancer

```
$ kubectl edit -n monitoring service/monitoring-prometheus-oper-prometheus
```
Make the above mentioned change (ClusterIp to LoadBalancer)

```
$ kubectl get all -n monitoring 
```
Load balancer will be created in some time. Copy the external port and run on browser with port 9090
