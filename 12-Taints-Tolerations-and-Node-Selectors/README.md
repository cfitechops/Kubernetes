# Kubernetes Taints, Tolerations and Node Selectors | How to make workloads target specific nodes

```sh
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
  namespace: cfitech
  labels:
    app: nginx
spec:
  replicas: 3
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 2
  revisionHistoryLimit: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      tolerations:
        - key: gpu
          operator: "Equal"
          value: yeah
          effect: NoSchedule
      containers:
      - name: nginx
        image: nginx:1.25.4
        ports:
        - containerPort: 80
          name: http
```

```sh
kubectl get nodes --show-labels
```

```sh
kubectl label node NODE KEY=VALUE

kubectl label node controlplane gpu=true
```

```sh
kubectl get nodes --show-labels
```

```sh
kubectl taint node controlplane gpu=true:NoSchedule
```

```sh
kubectl taint node node01 gpu=false:NoSchedule
```

```sh
kubectl taint node node02 gpu=yeah:NoSchedule
```

```sh
kubectl get po -w -o wide
```
