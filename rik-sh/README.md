# RIK prototype

Implemented flow:

1. Install helm chart which schedule single run job - init container + application container
1. Wait for job to be scheduled
1. Upload current directory to init container volume
1. Mark init container that content is uploaded
1. Init container is existing and starting main app

```
NAME=app1 NAMESPACE=dev ./rik-sh
NAME=app1 NAMESPACE=dev HELM_CHART=https://github.com/wintermute-core/prototypes/releases/download/rik-helm-0.0.1/rik-0.1.0.tgz ./rik-sh
```
