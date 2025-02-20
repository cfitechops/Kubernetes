# Labels & Selectors

- Création de pods avec différentes étiquettes, l'utilisation de sélecteurs d'étiquettes et la manipulation d'étiquettes sur des ressources existantes

#### Créez et appliquez le premier fichier YAML

```sh
cat <<EOF > 1.yml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - image: nginx
    name: nginx
---
apiVersion: v1
kind: Pod
metadata:
  name: nginx1
spec:
  containers:
  - image: nginx
    name: nginx
EOF

kubectl apply -f 1.yml
```

#### Afficher les pods et leurs étiquettes

```sh
kubectl get pods
kubectl get pods --show-labels
```

#### Créez et appliquez le deuxième fichier YAML avec les pods étiquetés

```sh
cat <<EOF > 2.yml
apiVersion: v1
kind: Pod
metadata:
  name: nginx2
  labels:
    app: frontend
    env: prod
spec:
  containers:
  - image: nginx
    name: nginx
---
apiVersion: v1
kind: Pod
metadata:
  name: nginx3
  labels:
    app: frontend
    env: dev
spec:
  containers:
  - image: nginx
    name: nginx
EOF

kubectl apply -f 2.yml
```

#### Utilisez des sélecteurs pour filtrer les pods

```sh
kubectl get pods --show-labels
kubectl get pods -l env=prod
kubectl get pods -l env=dev
```

#### Supprimer un pod et vérifier son absence

```sh
kubectl delete pod nginx3 --force
kubectl get pods -l env=dev
```

#### Afficher les étiquettes et les détails des nœuds

```sh
kubectl get nodes --show-labels
kubectl describe node <node-name>
```

#### Ajouter des étiquettes aux ressources existantes

```sh
kubectl label pod nginx env=stg
kubectl label node <node-name> k8s=control-master
```

#### Créez et appliquez le troisième fichier YAML avec des étiquettes supplémentaires

```sh
cat <<EOF > 3.yml
apiVersion: v1
kind: Pod
metadata:
  name: nginx4
  labels:
    app: frontend
    env: prod
    version: v1.24
    team: Admin
spec:
  containers:
  - image: nginx
    name: nginx
---
apiVersion: v1
kind: Pod
metadata:
  name: nginx5
  labels:
    app: frontend
    env: dev
    team: dev-team
spec:
  containers:
  - image: nginx
    name: nginx
EOF

kubectl apply -f 3.yml
```

#### Utiliser des sélecteurs d’étiquettes basés sur des ensembles

```sh
kubectl get pods --show-labels
kubectl get pods -l 'env in (prod,dev)'
kubectl get pods -l 'env in (stg)'
```
