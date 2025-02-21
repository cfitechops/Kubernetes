# Services Types | How to access your apps with Services

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
        image: nginx:1.25
        ports:
        - containerPort: 80
```

```sh
kubectl apply -f deployment.yaml
```

```sh
kubectl expose deploy/nginx-deployment --help
```

```sh
kubectl expose deploy/nginx-deployment --name nginx-svc --type ClusterIP --port 8080 --target-port 80 --namespace=cfitech --dry-run=client -o yaml > svc.yaml
```

```sh
watch -n 1 kubectl get po,svc -n cfitech
```

```sh
nano dnsutils.yaml
```

```sh
apiVersion: v1
kind: Pod
metadata:
  name: dnsutils
  namespace: default
spec:
  containers:
  - name: dnsutils
    image: ubuntu:22.04
    command:
    - sleep
    - "infinity"
    imagePullPolicy: IfNotPresent
  restartPolicy: Always
```

```sh
kubectl apply -f dnsutils.yaml
```

```sh
kubectl get po -n cfitech
```

```sh
kubectl get svc -n cfitech
```

```sh
kubectl get po
```

```sh
kubectl exec -it dnsutils -- bash
```

```sh
apt update

apt install curl dnsutils dig -y

curl http://<IP SVC>

curl http://nginx-svc.cfitech
```

```sh
kubectl get po -n kube-system
```

```sh
nano svc.yaml
```

```sh
apiVersion: v1
kind: Service
metadata:
  labels:
    app: nginx
  name: nginx-svc
  namespace: cfitech
spec:
  ports:
  - port: 8080
    protocol: TCP
    targetPort: 80
  selector:
    app: nginx
  type: ClusterIP
```

```sh
kubectl apply -f svc.yaml
```

```sh
kubectl get svc -n cfitech
```

```sh
curl http://nginx-svc.cfitech:8080
```

```sh
kubectl delete -f svc.yaml
```

```sh
apiVersion: v1
kind: Service
metadata:
  labels:
    app: nginx
  name: nginx-svc
  namespace: cfitech
spec:
  ports:
  - port: 8080
    protocol: TCP
    targetPort: 80
    #nodePort: 30000 - 32767
  selector:
    app: nginx
  type: NodePort
```

```sh
kubectl apply -f svc.yaml
```

```sh
kubectl get nodes -o wide
```

```sh
kubectl get svc -n cfitech
```

```sh
curl http://<IP>:31274
```

```sh
kubectl delete -f svc.yaml
```

```sh
apiVersion: v1
kind: Service
metadata:
  labels:
    app: nginx
  name: nginx-svc
  namespace: cfitech
spec:
  ports:
  - port: 8080
    protocol: TCP
    targetPort: 80
  selector:
    app: nginx
  type: LoadBalancer
```
