# Cert Manager & External DNS | How to automate your DNS and TLS Certificates in Kubernetes

- [Cloudflare DNS](https://github.com/kubernetes-sigs/external-dns/blob/master/docs/tutorials/cloudflare.md)

- [Cert manager](https://cert-manager.io/docs/installation/kubectl/)

- [Cloudflare](https://cert-manager.io/docs/configuration/acme/dns01/cloudflare/)

- [DNS01](https://cert-manager.io/docs/configuration/acme/dns01/)

- [Securing NGINX-ingress](https://cert-manager.io/docs/tutorials/acme/nginx-ingress/)

```sh
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.17.0/cert-manager.yaml
```

```sh
kubectl get ns
```

```sh
kubectl get pod -n cert-manager
```

```sh
kubectl -n cert-manager create secret generic cloudflare-api-token-secret --from-literal=api-token
```

```sh
kubectl -n cert-manager logs -f deploy/cert-manager
```

```sh
watch kubectl get ing,cert,orders,certificaterequest -n cfitech
```

```sh
#kubectl create secret generic NAME --from-literal=api-token=TOKEN

kubectl -n cert-manager create secret generic cloudflare-api-token-secret --from-literal=api-token=TOKEN
```

```sh
#kubectl get secret NAME -o json | jq -r '.data."tls.crt"' | base64 -d | openssl x509 -noout -text

kubectl get secret -n cfitech cfitech-tls-ingress -o json | jq -r '.data."tls.crt"' | base64 -d | openssl x509 -noout -text
```
