# Kubernetes Affinity and Anti-Affinity | How can you use them to attract or repel workloads

```sh
kubectl get nodes --show-labels
```

```sh
kubectl label nodes controlplane topology.kubernetes.io/zone=zone1 --overwrite
```

```sh
kubectl label nodes node01 topology.kubernetes.io/zone=zone2 --overwrite
```

```sh
kubectl label nodes node02 topology.kubernetes.io/zone=zone3 --overwrite
```

```sh
kubectl get po -n cfitech -o wide
```
