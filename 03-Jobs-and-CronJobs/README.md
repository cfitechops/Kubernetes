# Jobs and CronJobs

#### Jobs

```sh
nano job.yaml
```

```sh
apiVersion: batch/v1
kind: Job
metadata:
  name: pi
  namespace: cfitech
spec:
  template:
    spec:
      containers:
      - name: pi
        image: perl:5.34.0
        command: ["perl",  "-Mbignum=bpi", "-wle", "print bpi(2000)"]
      restartPolicy: Never
  backoffLimit: 4
```

```sh
kubectl apply -f job.yaml
```

```sh
watch -n 1 kubectl get po,jobs -n cfitech
```

```sh
kubectl describe job -n cfitech pi
```

```sh
kubectl delete -f job.yaml
```

#### CronJobs

```sh
nano cronjob.yaml
```

```sh
apiVersion: batch/v1
kind: CronJob
metadata:
  name: hello
  namespace: cfitech
spec:
  schedule: "* * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: hello
            image: busybox:1.28
            imagePullPolicy: IfNotPresent
            command:
            - /bin/sh
            - -c
            - date; echo Hello from the Kubernetes cluster
          restartPolicy: OnFailure
```

```sh
|-----> minute (0 - 59)
| |-----> hour (0 - 23)
| | |-----> day of the month (1 - 31)
| | | |-----> month (1 - 12)
| | | | |-----> day of the week (0 - 6)
| | | | |       (Sunday to Saturday; 7 is also Sunday on some systems)
| | | | |        OR sun, mon, tue, wed, thu, fri, sat
```

```sh
kubectl apply -f cronjob.yaml
```

```sh
watch -n 1 kubectl get po,jobs,cronjobs -n cfitech
```
