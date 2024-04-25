# Purpose:

Create a slurm cluster upon k8s


# Usage:
```
kubectl apply -f .
kubectl get po -w
```


```
kubectl exec -it $(kubectl get pods --selector=app=slurmmaster -o jsonpath='{.items[0].metadata.name}') -- scontrol show node
```


NOTE: just use local path as PV for now.


# Reference:

This is a kubernetes yaml translation for https://github.com/rancavil/slurm-cluster

its document is https://medium.com/analytics-vidhya/slurm-cluster-with-docker-9f242deee601
