# Kubernetes Cron job termination

Solution to run cronjob of 2 containers - main and sidecar, when main exit, sidecar will be terminated.

Deployment:
```
kubectl create namespace jobfix
kubectl -n jobfix apply -f cronjob.yaml
```

Example:
```
$ kubectl -n jobfix get pods
NAME                    READY   STATUS      RESTARTS   AGE
job1-1634073180-ztkmc   0/2     Completed   0          21s
```