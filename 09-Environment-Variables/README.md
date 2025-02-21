# Kubernetes Environment Variables | How to configure EnvVars in your Pods

```sh
---
apiVersion: v1
kind: Secret
metadata:
  name: mysql-passwd
  namespace: cfitech
data:
  passwd: UEBzc3cwcmQxMjM=

---
apiVersion: v1
kind: ConfigMap
metadata:
  name: mysql-config
  namespace: cfitech
data:
  cnf: |
    [mysqld]
    slow-query-log = 1

---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql
  namespace: cfitech
  labels:
    app: mysql
spec:
  serviceName: mysql
  replicas: 1
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - name: database
        image: mysql:5.7
        ports:
        - containerPort: 3306
          name: sql
        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-passwd
              key: passwd
        - name: MYSQL_CONFIGURATION
          valueFrom:
            configMapKeyRef:
              name: mysql-config
              key: cnf

---
apiVersion: v1
kind: Service
metadata:
  name: mysql
  namespace: cfitech
spec:
  type: NodePort
  ports:
  - name: sql
    port: 3306
    protocol: TCP
    targetPort: sql
  selector:
    app: mysql
```

```sh
echo -n P@ssw0rd123 | base64 -w 0
echo P@ssw0rd123 | base64 -d
```

```sh
kubectl apply -f deployment.yaml
```

```sh
kubectl get po,svc,cm,secret -n cfitech
```

```sh
kubectl get po mysql -o yaml -n cfitech
```
