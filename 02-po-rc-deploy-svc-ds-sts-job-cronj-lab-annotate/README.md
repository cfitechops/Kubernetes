# Kubernetes Deployments, DaemonSets & StatefulSets

## 1 - Deployments

- Un **Deployment** est un objet qui gère le cycle de vie des applications dans un cluster Kubernetes. Il permet de :

  - Décrire l'état souhaité d'une application via un fichier YAML déclaratif.
  - Effectuer des mises à jour progressives (rolling updates) sans temps d'arrêt.
  - Gérer automatiquement les ReplicaSets et les Pods associés.
  - Effectuer des rollbacks vers des versions précédentes en cas de problème.
  - Mettre à l'échelle l'application en ajustant le nombre de réplicas.

- Les Deployments offrent plusieurs stratégies de déploiement comme le rolling update (par défaut), recreate, blue/green et canary.

- Ces commandes permettent de gérer efficacement les Deployments dans Kubernetes, y compris les mises à jour et les rollbacks.

#### Créer un fichier de configuration pour le Deployment

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
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

#### Appliquer le Deployment

```sh
kubectl apply -f deployment.yaml
```

#### Vérifier l'état du Deployment

```sh
kubectl get po -n cfitech
kubectl get po -n cfitech -w
kubectl get rs -n cfitech -w
kubectl describe po -n cfitech
```

#### Modifier l'image dans le fichier deployment.yaml

```sh
spec:
  containers:
  - name: nginx
    image: nginx:1.24
```

#### Appliquer les modifications

```sh
kubectl apply -f deployment.yaml
```

#### Gérer le rollout

```sh
kubectl rollout status deploy/nginx-deployment -n cfitech
kubectl get pod -n cfitech -l app=nginx -w
kubectl rollout pause deploy/nginx-deployment -n cfitech
kubectl rollout resume deploy/nginx-deployment -n cfitech
```

#### Annuler le dernier déploiement

```sh
kubectl rollout undo deploy/nginx-deployment -n cfitech
```

#### Vérifier l'historique des révisions

```sh
kubectl rollout history deploy/nginx-deployment -n cfitech
```

#### Revenir à une révision spécifique

```sh
kubectl rollout undo deploy/nginx-deployment -n cfitech --to-revision=2
```

- Les Deployments simplifient considérablement la gestion des applications dans Kubernetes par rapport à la gestion directe des Pods.

## 2 - DaemonSet

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

## 3 - StatefulSets

- Liens vers [longhorn.](https://github.com/longhorn/longhorn/blob/master/deploy/longhorn.yaml)

```sh
kubectl apply -f https://github.com/longhorn/longhorn/blob/master/deploy/longhorn.yaml
```

```sh
kubectl get po -n longhorn-system -w
```

```sh
nano sts.yaml
```

```sh
apiVersion: v1
kind: Service
metadata:
  labels:
    app: cassandra
  name: cassandra
  namespace: cfitech
spec:
  clusterIP: None
  ports:
  - port: 9042
  selector:
    app: cassandra
---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: cassandra
  namespace: cfitech
  labels:
    app: cassandra
spec:
  serviceName: cassandra
  replicas: 3
  selector:
    matchLabels:
      app: cassandra
  template:
    metadata:
      labels:
        app: cassandra
    spec:
      terminationGracePeriodSeconds: 1800
      containers:
      - name: cassandra
        image: gcr.io/google-samples/cassandra:v13
        imagePullPolicy: Always
        ports:
        - containerPort: 7000
          name: intra-node
        - containerPort: 7001
          name: tls-intra-node
        - containerPort: 7199
          name: jmx
        - containerPort: 9042
          name: cql

        securityContext:
          capabilities:
            add:
              - IPC_LOCK
        lifecycle:
          preStop:
            exec:
              command:
              - /bin/sh
              - -c
              - nodetool drain
        env:
          - name: MAX_HEAP_SIZE
            value: 512M
          - name: HEAP_NEWSIZE
            value: 100M
          - name: CASSANDRA_SEEDS
            value: "cassandra-0.cassandra.cfitech.svc.cluster.local"
          - name: CASSANDRA_CLUSTER_NAME
            value: "K8Demo"
          - name: CASSANDRA_DC
            value: "DC1-K8Demo"
          - name: CASSANDRA_RACK
            value: "Rack1-K8Demo"
          - name: POD_IP
            valueFrom:
              fieldRef:
                fieldPath: status.podIP
        readinessProbe:
          exec:
            command:
            - /bin/bash
            - -c
            - /ready-probe.sh
          initialDelaySeconds: 15
          timeoutSeconds: 5
        # These volume mounts are persistent. They are like inline claims,
        # but not exactly because the names need to match exactly one of
        # the stateful pod volumes.
        volumeMounts:
        - name: cassandra-data
          mountPath: /cassandra_data
  # These are converted to volume claims by the controller
  # and mounted at the paths mentioned above.
  # do not use these in production until ssd GCEPersistentDisk or other ssd pd
  volumeClaimTemplates:
  - metadata:
      name: cassandra-data
    spec:
      accessModes: [ "ReadWriteOnce" ]
      storageClassName: longhorn
      resources:
        requests:
          storage: 1Gi
```

```sh
kubectl get storageclass
```

```sh
kubectl apply -f sts.yaml
```

```sh
watch -n 2 kubectl get po,deploy,ds,sts -n cfitech
```

```sh
kubectl logs -f cassandra-1 -f -n cfitech
```

```sh
kubectl describe po -n cfitech cassandra-1
```

```sh
kubectl delete po -n cfitech cassandra-1
```
