# Kubernetes Init Containers | What they are and how to use them

```sh
apiVersion: v1
kind: Pod
metadata:
  name: newpod
  namespace: cfitech
spec:
  volumes:
  - name: shared-vol
  initContainers:
  - name: configurer
    image: busybox
    command:
    - wget
    - "-O"
    - "/usr/shared/app/config.json"
    - https://raw.githubusercontent.com/bmuschko/ckad-crash-course/master/exercises/07-init-container/app/config/config.json
    volumeMounts:
    - mountPath: /usr/shared/app
      name: shared-vol
  containers:
  - image: bmuschko/nodejs-read-config:1.0.0
    name: web
    ports:
    - containerPort: 8080
    volumeMounts:
    - mountPath: /usr/shared/app
      name: shared-vol
```
