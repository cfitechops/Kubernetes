# Replication Controller & Replica Set

#### Découvrir ReplicationController

```sh
kubectl explain replicationcontroller
```

#### Créer et appliquer ReplicationController

```sh
cat <<EOF > rc.yml
apiVersion: v1
kind: ReplicationController
metadata:
  name: rc
spec:
  replicas: 3
  selector:
    env: stg
  template:
    metadata:
      name: pod1
      labels:
        app: nginx
        env: stg
    spec:
      containers:
      - name: nginx
        image: nginx
EOF
```

```sh
kubectl apply -f rc.yml
kubectl get pods
```

#### Créer et appliquer ReplicaSet

```sh
cat <<EOF > rs.yml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: rs
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      name: pod1
      labels:
        app: nginx
        env: prod
    spec:
      containers:
      - name: nginx
        image: nginx
EOF
```

```sh
kubectl apply -f rs.yml
kubectl get pods
kubectl get rs
```

#### Suppression du pod de test et recréation automatique

```sh
kubectl delete pod <pod-name> --force
kubectl get pods
```

#### Inspecter les détails du pod et du ReplicaSet

```sh
kubectl get pod <pod-name> -o yaml
kubectl describe rs rs
```

#### Ensemble de répliques à l'échelle

```sh
kubectl scale rs rs --replicas=6
kubectl get pods
kubectl scale rs rs --replicas=1
kubectl get pods
```

#### Modifier le ReplicaSet

```sh
kubectl edit rs rs
```
