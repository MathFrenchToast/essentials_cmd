# Kubectl essentials

# rollout the latest version
note : suppose that yaml use image:pull-always
kubectl rollout restart deployment {deploymentname}
 
change image => this will lead to a rollout
kubectl set image deployments/{deploymentname} {containername}={newfullimgname}

# rollback a deployment
kubectl rollout undo deployment {deploymentname}

# scale a deployment
kubectl scale deployment {deploymentname} --replicas=3

# restart a deployment 
Update image of a deployment, with a entry in history (record option)
kubectl set image deployments/{deploymentname} {containername}={newfullimgname}  --record

# delete all evicted pod

kubectl get pod -n $NAMESPACENAME | grep Evicted | awk '{print $1}' | xargs kubectl delete pod -n $NAMESPACENAME


