## Create the cluster without CNI
```
kind create cluster --config - <<EOF  
kind: Cluster
name: cilium-observability 
apiVersion: kind.x-k8s.io/v1alpha4  
networking:  
  disableDefaultCNI: true 
nodes:  
- role: control-plane  
- role: worker  
- role: worker  
- role: worker  
EOF
```

## Check that you are using the current kubernetes context:

```
kubectl cluster-info --context kind-cilium-observability
```

## Once created the cluster, run the following command:

`kubectl get nodes`
