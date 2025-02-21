# Kubernetes Ingresses And Loadbalancer Services | How to make your apps accessible from the internet

- [Cloud provider](https://kube-vip.io/docs/usage/cloud-provider/)

```sh
kubectl apply -f https://raw.githubusercontent.com/kube-vip/kube-vip-cloud-provider/main/manifest/kube-vip-cloud-controller.yaml
```

```sh
kubectl get po -n kube-system
```

```sh
kubectl edit configmap -n kube-system kubevip
```

```sh
  resourceVersion: "715920"
  uid: <>
data:
  cidr-global: 192.168.0.240/28  # Techically 240-255
```

```sh
kubectl apply -f database.yaml
```

```sh
kubectl get po -n cfitech
```

```sh
kubectl get svc -n cfitech -w
```

- [Ingress nginx](https://kubernetes.github.io/ingress-nginx/deploy/)

```sh
wget https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.12.0/deploy/static/provider/baremetal/deploy.yaml
```

```sh
vi deploy.yaml
```

```sh
NodePort ---> LoadBalalancer
```

```sh
kubectl apply -f deploy.yaml
```

```sh
kubectl get ingressclass
```

```sh
kubectl get svc,po -n ingress-nginx
```

```sh
kubectl logs -f <POD_NAME> -n ingress-nginx
```

```sh
kubectl apply -f web.yaml
```

```sh
kubectl apply -f ingress.yaml
```

```sh
curl http://cfitech.diarabaka.com
```

```sh
kubectl logs -f <POD_NAME> -n ingress-nginx
```

```sh
kubectl get ing -n cfitech
```

```sh
kubectl get svc -n ingress-nginx
```
