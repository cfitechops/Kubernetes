# Kubernetes Security - Security Contexts | How to use them to make your Containers and Pods secure

```sh
apiVersion: v1
kind: Pod
metadata:
  name: sec-context
  namespace: cfitech
spec:
  securityContext:
    runAsUser: 1000
    runAsGroup: 3000
    fsGroup: 2000
  volumes:
  - name: sec-ctx
    emptyDir: {}
  containers:
  - name: security-demo
    image: busybox:1.36.1
    command:
      - sh
      - -c
      - sleep 1h
    volumeMounts:
    - mountPath: /data/demo
      name: sec-ctx
    securityContext:
      allowPrivilegeEscalation: false
  - name: security-demo2
    image: busybox:1.36.1
    command:
      - sh
      - -c
      - sleep 1h
    volumeMounts:
    - mountPath: /data/demo
      name: sec-ctx
    securityContext:
      runAsUser: 2000
      allowPrivilegeEscalation: false
```

```sh
kubectl apply -f securitycontext.yaml
```

```sh
kubectl exec -it sec-context -c security-demo -n cfitech -- bash
```

```sh
ls -l /data
id
```
