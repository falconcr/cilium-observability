`helm repo add grafana https://grafana.github.io/helm-charts`

```
loki:
  auth_enabled: false
  commonConfig:
    replication_factor: 1
  storage:
    type: 'filesystem'
singleBinary:
  replicas: 1
  persistence:
    storageClass: 'standard'
monitoring:
  selfMonitoring:
    enabled: false
test:
  enabled: false
```

```
helm install --values values.yaml loki --namespace=monitoring grafana/loki
```

Create a `promtail.yaml` file and add the following:

```
initContainer:
  # # -- Specifies whether the init container for setting inotify max user instances is to be enabled
   - name: init
  #   # -- Docker registry, image and tag for the init container image
     image: docker.io/busybox:1.33
  #   # -- Docker image pull policy for the init container image
     imagePullPolicy: IfNotPresent
  #   # -- The inotify max user instances to configure
     command:
       - sh
       - -c
       - sysctl -w fs.inotify.max_user_instances=512
     securityContext:
       privileged: true
```

Install the promtail helm chart using the above configuration file:

`helm upgrade --install promtail --values promtail.yaml  grafana/promtail -n monitoring`

## Configure Data Source

Port Forward to Grafana (If you already have it, ignore this step):

```
kubectl -n monitoring port-forward service/grafana --address 0.0.0.0 --address :: 3000:3000
