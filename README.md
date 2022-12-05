# MollySocket for Kubernetes

## Helm chart for deploying MollySocket in Kubernetes cluster

Prepare your configuration of the MollySocket helm release by creating your values file e.g. `my_values.yml`
```yaml

# There is not yet any container image:
# https://github.com/orgs/MollySocket/packages?repo_name=mollysocket
# so genofire has one
image:
  repository: ghcr.io/genofire/mollysocket
  pullPolicy: Always # if the image under same tag is updated
  tag: "latest"

mollysocket:
  log: "warn"

## here is the sqlite file stored
persistence:
  enabled: true

## you should not change, till you have support for
## a. your cluster support accessMode=ReadWriteMany  or b. MollySocket support external databases
replicaCount: 1


ingress:
  enabled: true
  hosts:
    - host: chart-example.local
      paths:
        - path: /
          pathType: ImplementationSpecific
  tls:
    - secretName: chart-local-cert
      hosts:
        - chart-example.local
```

Installation with following commands:

```bash
helm repo add mollysocket-repo https://mollysocket.github.io/helm/
helm install --create-namespace --namespace mollysocket my-mollysocket mollysocket-repo/mollysocket -f my_values.yml
```

## TODO
- Working with [Prometheus Operator](https://prometheus-operator.dev/) ([Helmchart](https://artifacthub.io/packages/helm/prometheus-community/kube-prometheus-stack))
  - Waiting for Metric Support of MollySocket: https://github.com/MollySocket/mollysocket/issues/3
  - [ ] ServiceMonitor to scrape metrics
  - [ ] default set of PrometheusRules for Alerting on unespected behavour
  - [ ] deploy [Grafana](https://grafana.com/grafana/)-Dashboards

- Working with [Banzai Logging Operator](https://banzaicloud.com/docs/one-eye/logging-operator/)
  - Waiting for Serizable-Log Support for MollySocket: https://github.com/MollySocket/mollysocket/issues/5
  - [ ] Flow with Serilizer
  - [ ] optional Output (if not ClusterOutput)