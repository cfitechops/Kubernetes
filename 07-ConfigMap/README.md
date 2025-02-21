# ConfigMap

```sh
apiVersion: v1
kind: ConfigMap
metadata:
  name: nginx-conf
  namespace: cfitech
data:
  website: |
    server: {
      port 80
      location / {
        root /usr/share/nginx/html
      }
    }
```

```sh
kubectl apply -f config.yaml
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
        - mountPath: /etc/nginx/conf.d/
          name: nginx-data
      volumes:
      - name: nginx-data
        configMap:
          name: nginx-conf
          items:
          - key: website
            path: website
```

```sh
kubectl apply -f deployment.yaml
```

```sh
kubectl get cm -n cfitech
```

```sh
kubectl get po -n cfitech
```

```sh
kubectl exec -it <POD_NAME> -n cfitech -- ls /etc/nginx/conf.d/
```

```sh
kubectl exec -it <POD_NAME> -n cfitech -- cat /etc/nginx/conf.d/website
```
