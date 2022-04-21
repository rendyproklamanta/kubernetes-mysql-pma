# Run MySql kubernetes Single-Instance (1 pod only)

> Ref : https://kubernetes.io/docs/tasks/run-application/run-single-instance-stateful-application/

<br>

- Generate password using base64

```
echo -n 'your_password' | base64
> MTIzcXdl
```

- Open secret.yaml :

```
	name: mysql-secret
data:
	root_password: MTIzcXdl
```

- Open deployment.yaml and change to :

```
env:
	- name: MYSQL_ROOT_PASSWORD
		valueFrom:
			secretKeyRef:
			name: mysql-secret
			key: root_password
```

- Apply deployment :

```
$ kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml apply -f secret.yaml
$ kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml apply -f persistentvolume.yaml
$ kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml apply -f config.yaml
$ kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml apply -f deployment.yaml
```

- Exec mysql pod via ssh :

```
$ kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml get pods
$ kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml exec --stdin --tty <pods-name-xxxx> -- /bin/bash
```

- Using command to access mysql :

```
$ mysql -u root
```

- Check if root has a password :

```
SELECT User, Host, Password FROM mysql.user;
```

- If root has no password.Assign root password

```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'your_password';
```

- Create user root with host (%) to access from PMA

```
CREATE USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'your_password';
```

- Assign root privilege

```
GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' WITH GRANT OPTION;
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

```
mysql -u root -p
```

- Delete data

```
kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml delete deployment,svc mysql
kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml delete configmap mysql-config-map
kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml delete pvc mysql-pv-claim
kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml delete pv mysql-pv-volume
kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml delete secret mysql-secret
```

<br>
<b>Note:</b>

- If you editing config.yaml / ConfigMap, redeploy to make changes :

```
kubectl --kubeconfig=D:\kubeconfig\vultr\test.yaml rollout restart deployment mysql
```

- Show variable in mysql :

```
SHOW VARIABLES LIKE 'max_connections';
```
