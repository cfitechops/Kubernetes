# Scaling your Kubernetes Apps | How to scale your Pods and Nodes

#### Resources

[horizontal pod autoscale walkthrough](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale-walkthrough/)
[metrics server](https://github.com/kubernetes-sigs/metrics-server#readme)
[troubleshooting kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/troubleshooting-kubeadm/)

```sh
wget https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml > metrics.yaml
```

```sh
kubectl -n cfitech autoscale deploy/php-apache --cpu-percent=50 --min=1 --max=5 $do > hpa.yaml
```

```sh
watch -n 2 kubectl get po,deploy,rs,hpa -n cfitech
```

```sh
kubectl -n cfitech run -i --tty load-generator --rm --image=busybox:1.28 --restart=Never -- /bin/sh -c "while sleep 0.01; do wget -q -O- http://php-apache; done"
```
