# Purpose:

Create a slurm cluster upon k8s


# Usage:
```
export NS=slurm
kubectl create ns $NS
sed -i "s/YOURNAMESPACE/$NS/g" slurmmaster.yaml
kubectl -n $NS apply -f .
kubectl -n $NS get po -w
```


```
kubectl -n $NS  exec -it $(kubectl -n $NS get pods --selector=app=slurmmaster -o jsonpath='{.items[0].metadata.name}') -- sinfo

kubectl -n $NS  exec -it $(kubectl -n $NS get pods --selector=app=slurmmaster -o jsonpath='{.items[0].metadata.name}') -- scontrol show node


# debug error for slurmctld log, as needed
kubectl -n $NS  exec -it $(kubectl -n $NS get pods --selector=app=slurmmaster -o jsonpath='{.items[0].metadata.name}') -- tail -f  /var/log/slurm-llnl/slurmctld.log

```




```
kubectl -n $NS  exec -it $(kubectl -n $NS  get pods --selector=app=slurmmaster -o jsonpath='{.items[0].metadata.name}') -- bash
```
all below commands are running inside above pod:


```
vim a.sh
```

create a slurm script as below
```

#!/bin/bash
#SBATCH --job-name=test-job             # Job name
##SBATCH --mem 2048                      #  where X is the maximum amount of memory your job will use per node, in MB.
#SBATCH --ntasks=2                      # Number of ranks
#SBATCH --nodes=2                      # Number of nodes
#SBATCH --ntasks-per-node=1             # How many tasks on each node
#SBATCH --output=test-job-%j.log     # Standard output and error log
sleep 5
pwd; hostname; date
env|grep HOSTNAME

```


then run the script and inspect result
```
sbatch  a.sh
squeue

```

NOTE: just use local path as PV for now.


# Reference:

This is a kubernetes yaml translation for https://github.com/rancavil/slurm-cluster

its document is https://medium.com/analytics-vidhya/slurm-cluster-with-docker-9f242deee601
