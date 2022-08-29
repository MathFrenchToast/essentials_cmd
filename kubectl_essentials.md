# Kubectl essentials

# rollout the latest version
restart deployment in place. Note : this suppose that yaml use image:pull-always \
`kubectl rollout restart deployment {deploymentname}`
 
change image => this will lead to a rollout \
`kubectl set image deployments/{deploymentname} {containername}={newfullimgname}`

# rollback a deployment
`kubectl rollout undo deployment {deploymentname}`

# scale a deployment
See current size: \
`kubectl get deployments` \
`kubectl scale deployment {deploymentname} --replicas=3`

# delete all evicted pod
if list of pods show some pods with status `Evicted`, we can remove all this pod in one call: \
`kubectl get pod -n $NAMESPACENAME | grep Evicted | awk '{print $1}' | xargs kubectl delete pod -n $NAMESPACENAME`

# move configmap or secret from one namespace to another
```
CONFIGMAP=***configmap name**
FROMNS=**src ns***
TONS=***dest ns***

kubectl get cm $CONFIGMAP --namespace=$FROMNS -o yaml \
  | sed "s/namespace: $FROMNS/namespace: $TONS/" \
  | kubectl create -f -  
```

same for a secret : 
```
SECRETNAME=***secretname***
kubectl get secret $SECRETNAME --namespace=$FROMNS -o yaml \
  | sed "s/namespace: $FROMNS/namespace: $TONS/" \
  | kubectl create -f -
 ```
