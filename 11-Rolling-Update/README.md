# 99.99999% Uptime in Kubernetes | How to keep your workloads online during an application upgrade

```sh
kubectl set image -n cfitech deploy/NAME CONTAINER=IMAGE:TAG
```

```sh
curl http://172.18.0.3:31000
```

```sh
while true; do curl http://172.18.0.3:31000; sleep 0.5; done
```

```sh
kubectl rollout restart deploy NAME
```

```sh
kubectl rollout status deploy NAME
```

```sh
kubectl rollout pause deploy NAME
```

```sh
kubectl rollout resume deploy NAME
```

```sh
kubectl rollout undo deploy NAME
```

```sh
kubectl rollout undo deploy NAME --to-revision=X
```

```sh
kubectl rollout history deploy NAME
```
