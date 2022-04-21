- Prerequisite :
  <br>
  Add DNS A record "pma.domain.com"
  <br>
  Edit host/hosts in ingress.yaml replace with domain

- Deploy :

```
$ kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml apply -f deployment.yaml
$ kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml apply -f ingress.yaml
```
