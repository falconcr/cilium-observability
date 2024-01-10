## Install hubble ui
```
helm upgrade cilium cilium/cilium --version 1.14.3 \
   --namespace kube-system  \
   --reuse-values \
   --set hubble.relay.enabled=true \
   --set hubble.ui.enabled=true
```
## Run Hubble UI

`cilium hubble ui`

## Test data

```
kubectl -n podinfo exec deployment/podinfo-client -c podinfod -- sh -c 'for i in $(seq 100) ; do \  
curl -S http://podinfo-frontend:9898/echo  
done'
```
