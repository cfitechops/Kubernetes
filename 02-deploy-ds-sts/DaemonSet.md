# DaemonSet

- Un DaemonSet est un contrôleur Kubernetes qui garantit qu'une copie d'un pod s'exécute sur tous les nœuds (ou un sous-ensemble spécifié) d'un cluster.

#### Créez un fichier daemonset.yaml avec la configuration suivante

```sh
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: fluentd-elasticsearch
  namespace: cfitech
  labels:
    k8s-app: fluentd-logging
spec:
  selector:
    matchLabels:
      name: fluentd-elasticsearch
  template:
    metadata:
      labels:
        name: fluentd-elasticsearch
    spec:
      tolerations:
      - key: node-role.kubernetes.io/control-plane
        operator: Exists
        effect: NoSchedule
      - key: node-role.kubernetes.io/master
        operator: Exists
        effect: NoSchedule
      containers:
      - name: fluentd-elasticsearch
        image: quay.io/fluentd_elasticsearch/fluentd:v2.5.2
        resources:
          limits:
            memory: 200Mi
          requests:
            cpu: 100m
            memory: 200Mi
        volumeMounts:
        - name: varlog
          mountPath: /var/log
      volumes:
      - name: varlog
        hostPath:
          path: /var/log
```

#### Appliquez la configuration du DaemonSet

```sh
kubectl apply -f daemonset.yaml
```

#### Vérifiez les pods créés par le DaemonSet

```sh
kubectl get po -n cfitech
```

#### Affichez les informations sur le DaemonSet

```sh
kubectl get ds -n cfitech
```

#### Pour obtenir les détails complets du DaemonSet au format YAML

```sh
kubectl get ds -n cfitech <NAME> -o yaml
```

- Ce DaemonSet déploie un pod Fluentd sur chaque nœud du cluster pour la collecte de logs, avec des tolérances pour s'exécuter également sur les nœuds maîtres et de contrôle.
