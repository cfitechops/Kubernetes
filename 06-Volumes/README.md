# Kubernetes Volumes | How to mount Volumes in the pods

```sh
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: nginx-data-pvc
  namespace: cfitech
spec:
  storageClassName: longhorn
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
```

```sh
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: nginx-data-pvc
  namespace: cfitech
spec:
  storageClassName: longhorn
  accessModes:
  - ReadWriteMany
  resources:
    requests:
      storage: 5Gi
```

```sh
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: nginx-data-pvc
  namespace: cfitech
spec:
  storageClassName: longhorn
  accessModes:
  - ReadOnlyMany
  resources:
    requests:
      storage: 5Gi
```

```sh
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment-pvc
  namespace: cfitech
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.24
        ports:
        - containerPort: 80
        volumeMounts:
        - mountPath: /data
          name: nginx-data
      volumes:
      - name: nginx-data
        persistentVolumeClaim:
          claimName: nginx-data-pvc
```

```sh
kubectl apply -f deployment.yaml
```

```sh
kubectl apply -f pvc.yaml
```

```sh
watch -n 1 kubectl get po,pv,pvc -n cfitech
```

```sh
kubectl describe po <POD_NAME> -n cfitech
```
