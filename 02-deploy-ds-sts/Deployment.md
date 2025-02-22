# Deployments

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
