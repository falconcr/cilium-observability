## Installs tempo 
`helm install tempo grafana/tempo -n monitoring`

## Installs xk6-client-tracing
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: xk6-tracing
  namespace: monitoring
spec:
  minReadySeconds: 10
  replicas: 1
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      app: xk6-tracing
      name: xk6-tracing
  template:
    metadata:
      labels:
        app: xk6-tracing
        name: xk6-tracing
    spec:
      containers:
      - env:
        - name: ENDPOINT
          value: tempo:4317
        image: ghcr.io/grafana/xk6-client-tracing:v0.0.2
        imagePullPolicy: IfNotPresent
        name: xk6-tracing
```
