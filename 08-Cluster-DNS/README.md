# Kubernetes Cluster DNS | How does DNS work in a Kubernetes Cluster

```sh
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
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
```

```sh
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment-again
  namespace: cfitech
  annotations:
    config: prod
  labels:
    app: nginx
    owner: myOwner
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
      owner: myOwner
  template:
    metadata:
      labels:
        app: nginx
        owner: myOwner
    spec:
      containers:
      - name: nginx
        image: nginx:1.24
        ports:
        - containerPort: 80
```

```sh
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: cfitech-again
  annotations:
    config: prod
  labels:
    app: nginx
    owner: myOwner
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
      owner: myOwner
  template:
    metadata:
      labels:
        app: nginx
        owner: myOwner
    spec:
      containers:
      - name: nginx
        image: nginx:1.24
        ports:
        - containerPort: 80
```

```sh
apiVersion: v1
kind: Pod
metadata:
  name: dnsutils
  namespace: cfitech
spec:
  containers:
  - image: ubuntu:22.04
    name: dnsutils
    command:
    - sleep
    - "infinity"
    imagePullPolicy: IfNotPresent
  restartPolicy: Always
```

## kubectl get cm -n kube-system coredns -o yaml

```sh
kubectl apply -f <FILES.YAML>
```

```sh
kubectl expose -n cfitech deploy/nginx-deployment --port 80
```

```sh
kubectl expose -n cfitech-again deploy/nginx-deployment --port 80
```

```sh
kubectl get nodes -o wide
```

```sh
kubectl get svc -n kube-system
```

```sh
kubectl get po,svc -n cfitech -o wide
```

```sh
kubectl get po,svc -n cfitech-again -o wide
```

```sh
kubectl exec -it dnsutils -- bash
```

```sh
apt update && apt install -y curl dnsutils dig

nslookup nginx-deployment.cfitech.svc.cluster.local
nslookup nginx-deployment.cfitech-again

nslookup <IP_NODE>.cfitech-again
nslookup <IP_NODE>.cfitech.pod
nslookup <IP_NODE>

dig nginx-deployment.cfitech.svc
dig nginx-deployment.cfitech-again
```
