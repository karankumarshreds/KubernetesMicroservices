# Helm 

Helm is a package manager for kubernetes cluster similar to *apt-get* or *yum*.

### Installation 

- Go to : https://github.com/helm/helm/releases
- Download the binary as per your OS
- Setup the binary

```
$ helm version
$ helm repo update
```

You can install multiple packages using helm incase you do not want to do that manually.
All the packages (*eg: mysql*) with their installation guide is listed below: 

https://github.com/helm/charts/tree/master/stable

example: 

```shell
$ helm install stable/mysql --name sql-installation
$ helm ls ## to list installed packages
$ helm delete sql-installtion
```