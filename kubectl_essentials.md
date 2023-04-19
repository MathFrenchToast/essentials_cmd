# Kubectl essentials

# rollout the latest version (or another image)
restart deployment in place. Note : this suppose that yaml use image:pull-always \
`kubectl rollout restart deployment $DEPLOYMENTNAME`
 
change image with a entry in history (record option) => this will lead to a rollout \
`kubectl set image deployments/$DEPLOYMENTNAME $CONTAINERNAME=$IMAGE --record`  
equivalent to :   
`kubectl patch deployment $DEPLOYMENTNAME --patch "spec:\n template:\n  spec:\n   containers:\n   - name: $CONTAINERNAME\n     image: $IMAGE"`  

check status of the update (CTRL-C to quit)  
`kubectl rollout status deployment/$DEPLOYMENTNAME`  
check rollout history  
`kubectl rollout history deployment/$DEPLOYMENTNAME`  


# rollback a deployment
`kubectl rollout undo deployment $DEPLOYMENTNAME`

# scale a deployment
See current size: \
`kubectl get deployments` \
`kubectl scale deployment $DEPLOYMENTNAME --replicas=3`

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
 
# simple templating with env var and envsubst

Suppose we have many envvar defined  
you start by loading an env file dedicated to your environment : `. myenvfile.env`  
Now you can use a parametrized deployment file : `envsubst < deployment.yml  | kubectl -n mynamespace apply -f -`
deployment.yml example (stored in your project git repo):
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myservice
  labels:
    app: myservice
spec:
  replicas: $NBREPLICA
  selector:
    matchLabels:
      app: myservice
  template:
    metadata:
      labels:
        app: myservice
    spec:
      containers:
      - name: formervaluessrv
        image: $ACR_FULLNAME/myservice:$TAG
        imagePullPolicy: Always  
```
