# Replication Controller & Replica Set

#### Affiche la documentation et les détails sur le ReplicationController

```sh
kubectl explain replicationcontroller
```

#### Crée un fichier YAML pour le ReplicationController

```sh
nano rc.yml
```

```sh
apiVersion: v1
kind: ReplicationController
metadata:
  name: rc
spec:
  replicas: 3  # Nombre de réplicas souhaités
  selector:
    env: stg  # Sélecteur pour identifier les pods à gérer
  template:
    metadata:
      name: pod1
      labels:
        app: nginx  # Label pour le pod
        env: stg
    spec:
      containers:
      - name: nginx
        image: nginx  # Image du conteneur à utiliser
```

#### Applique la configuration du ReplicationController

```sh
kubectl apply -f rc.yml
```

#### Affiche la liste des pods créés par le ReplicationController

```sh
kubectl get pods
```

#### 2 - Crée un fichier YAML pour le ReplicaSet

```sh
nano rs.yml
```

```sh
apiVersion: apps/v1  # Version API correcte pour ReplicaSet
kind: ReplicaSet
metadata:
  name: rs
spec:
  replicas: 3  # Nombre de réplicas souhaités
  selector:
    matchLabels:
      app: nginx  # Sélecteur pour identifier les pods à gérer
  template:
    metadata:
      name: pod1
      labels:
        app: nginx  # Label pour le pod
        env: prod   # Environnement du pod
    spec:
      containers:
      - name: nginx
        image: nginx  # Image du conteneur à utiliser
```

#### Applique la configuration du ReplicaSet

```sh
kubectl apply -f rs.yml
```

#### Affiche la liste des pods créés par le ReplicaSet

```sh
kubectl get pods
```

#### Affiche la liste des ReplicaSets disponibles dans le cluster

```sh
kubectl get rs
```

#### Tester la suppression d'un pod et sa recréation automatique

- Supprime un pod spécifique (remplacez <pod-name> par le nom du pod)

```sh
kubectl delete pod <pod-name> --force
```

#### Affiche la liste des pods après suppression pour vérifier la recréation automatique par le ReplicaSet

```sh
kubectl get pods
```

#### Inspecter les détails d'un pod et d'un ReplicaSet

- Affiche les détails d'un pod spécifique au format YAML (remplacez <pod-name> par le nom du pod)

```sh
kubectl get pod <pod-name> -o yaml
```

#### Affiche les détails du ReplicaSet nommé 'rs'

```sh
kubectl describe rs rs
```

#### Augmente le nombre de réplicas à 6 dans le ReplicaSet 'rs'

```sh
kubectl scale rs rs --replicas=6
```

#### Affiche la liste des pods après mise à l'échelle pour vérifier les nouveaux pods créés

```sh
kubectl get pods
```

#### Réduit le nombre de réplicas à 1 dans le ReplicaSet 'rs'

```sh
kubectl scale rs rs --replicas=1
```

#### Affiche la liste des pods après mise à l'échelle pour vérifier la réduction des pods créés

```sh
kubectl get pods
```

#### Ouvre l'éditeur pour modifier la configuration du ReplicaSet 'rs'

```sh
kubectl edit rs rs
```
