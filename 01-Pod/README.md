# Pods

#### Pour exécuter un pod avec un seul conteneur

```sh
kubectl run --help

kubectl run nginx --image=nginx
```

#### Exécuter un pod avec des labels

```sh
kubectl run nginx --image=nginx:latest --labels="app=fe"
```

#### Exécuter un pod avec un port spécifié

```sh
kubectl run nginx --image=nginx:latest --labels="app=fe" --port=80
```

#### Sauvegarder la configuration du pod au format YAML

```sh
kubectl run nginx --image=nginx:latest --labels="app=fe" --port=80 --dry-run=client -o yaml > 1.yml
```

#### Voir la sortie YAML pour les commandes impératives

```sh
kubectl run nginx --image=nginx:latest --labels="app=fe" --port=80 --dry-run=client -o yaml
```

#### Appliquer une configuration YAML

```sh
kubectl apply -f 1.yml
```

#### Décrire un pod

```sh
kubectl describe po busybox1
```

#### Générer un YAML pour un pod busybox sans le créer

```sh
kubectl run busybox2 --image=busybox --dry-run=client -o yaml
```

#### Générer un YAML pour un pod busybox avec une commande sleep et le sauvegarder

```sh
kubectl run busybox2 --image=busybox --dry-run=client -o yaml -- sleep 5000 > 2.yml
```
