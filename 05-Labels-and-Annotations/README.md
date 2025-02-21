# Labels and Annotations

```sh
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-label
  namespace: cfitech
  labels:
    app: nginx
    owner: thierno
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
      owner: thierno
  template:
    metadata:
      labels:
        app: nginx
        owner: thierno
    spec:
      containers:
      - name: nginx
        image: nginx:1.24
        ports:
        - containerPort: 80
```

```sh
kubectl apply -f deployment.yaml
```

```sh
kubectl get po -n cfitech --show-labels
```

```sh
kubectl get po -n cfitech -l owner=thierno --show-labels
```

```sh
kubectl get po -n cfitech -l app=nginx
```

```sh
kubectl get pods -l 'env in (app,owner)' -n cfitech
```

```sh
kubectl get po -n kube-system -L app
```

```sh
kubectl get po -n cfitech -L app
```

```sh
kubectl get po -n cfitech -L owner
```

```sh
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-label-annotate
  namespace: cfitech
  annotations:
    config: prod
  labels:
    app: nginx
    owner: thierno
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
      owner: thierno
  template:
    metadata:
      labels:
        app: nginx
        owner: thierno
    spec:
      containers:
      - name: nginx
        image: nginx:1.24
        ports:
        - containerPort: 80
```
