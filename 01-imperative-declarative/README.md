# Aproche impérative et déclarative en Kubernetes

- Kubernetes permet de gérer ces ressources de deux manières principales : l'approche impérative et l'approche déclarative:

  - L'approche impérative consiste à donner des instructions directes à Kubernetes via des commandes, permettant une gestion rapide et ponctuelle des ressources.
  - L'approche déclarative, quant à elle, utilise des fichiers YAML pour définir l'état souhaité des ressources, facilitant ainsi la gestion à long terme et la reproductibilité des configurations.

- Les commandes essentielles pour gérer les pods et les namespaces dans Kubernetes.

##### Afficher l'aide de kubectl

```sh
kubectl --help
```

##### Lister les pods dans le namespace kube-system

```sh
kubectl get pod -n kube-system
kubectl get pod -n kube-system -v=6
kubectl get pod -n kube-system -v=9
```
