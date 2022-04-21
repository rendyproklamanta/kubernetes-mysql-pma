- Prerequisite :
  <br>
  Add DNS A record "pma.domain.com"
  <br>
  Edit host/hosts in ingress.yaml replace with domain

- Deployment with single instance:

```
$ kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml apply -f deployment.yaml
```

- Deployment with cluster :

Ref => https://github.com/rendyproklamanta/kubernetes-mysql-cluster
```
$ kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml apply -f deployment-cluster.yaml
```

- Deploy ingress :
```
$ kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml apply -f ingress.yaml
```