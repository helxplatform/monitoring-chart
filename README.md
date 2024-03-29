# monitoring

Monitoring stack for k8s clusters

![Version: 0.2.2](https://img.shields.io/badge/Version-0.2.2-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 0.2.2](https://img.shields.io/badge/AppVersion-0.2.2-informational?style=flat-square)

# Monitoring

This folder contains the monitoring stack for k8s clusters.

The following components are all included:

- https://www.kubecost.com/
- https://github.com/prometheus-community/helm-charts/blob/main/charts/kube-prometheus-stack/
- https://grafana.com/docs/loki/latest/
- https://grafana.com/docs/loki/latest/clients/promtail/
- https://github.com/falcosecurity/charts

## Installing

The monitoring chart needs to be deployed into the 'helx' namespace.

```
kubectl create ns helx

helm upgrade --install -n helx monitoring . -f your-values.yaml
```

(replace the values file with your environment if different)

## Creating a new monitored service

If you want to monitor a new service, create a ServiceMonitor in the templates folder similar to one of the existing ones: https://prometheus-operator.dev/docs/operator/api/#servicemonitor

After a `helm upgrade`, prometheus-operator will start scraping metrics from that service and storing them, visible in grafana. You can then create alerts based on changes in those metrics by creating an AlertmanagerConfig: https://prometheus-operator.dev/docs/operator/api/#alertmanagerconfig

## Requirements

| Repository | Name | Version |
|------------|------|---------|
| https://falcosecurity.github.io/charts | falco | 1.16.0 |
| https://grafana.github.io/helm-charts | loki | 2.6.0 |
| https://grafana.github.io/helm-charts | promtail | 3.8.1 |
| https://kubecost.github.io/cost-analyzer/ | cost-analyzer | 1.87.0 |
| https://prometheus-community.github.io/helm-charts | kube-prometheus-stack | 19.0.0 |

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| basicauth-creds | string | `nil` |  |
| cost-analyzer.enabled | bool | `true` |  |
| cost-analyzer.global.grafana.domainName | string | `"helx-grafana"` | If using a release name other than 'helx' then replace 'helx' with the release name. |
| cost-analyzer.global.grafana.enabled | bool | `false` |  |
| cost-analyzer.global.notifications.alertmanager.enabled | bool | `false` |  |
| cost-analyzer.global.notifications.alertmanager.fqdn | string | `"http://helx-kube-prometheus-alertmanager:9093"` | If using a release name other than 'helx' then replace 'helx' with the release name. |
| cost-analyzer.global.prometheus.enabled | bool | `false` |  |
| cost-analyzer.global.prometheus.fqdn | string | `"http://helx-kube-prometheus-prometheus:9090"` | If using a release name other than 'helx' then replace 'helx' with the release name. |
| cost-analyzer.prometheus.kube-state-metrics.disabled | bool | `true` |  |
| cost-analyzer.prometheus.kube-state-metrics.enabled | bool | `false` |  |
| cost-analyzer.prometheus.kube-state-metrics.rbac.create | bool | `false` |  |
| cost-analyzer.readonly | bool | `true` |  |
| cost-analyzer.reporting.errorReporting | bool | `false` |  |
| cost-analyzer.reporting.logCollection | bool | `false` |  |
| cost-analyzer.reporting.productAnalytics | bool | `false` |  |
| cost-analyzer.reporting.valuesReporting | bool | `false` | MUST BE FALSE! Otherwise ALL of values.yaml (including secrets) will be shipped to the kubecost people! |
| falco.enabled | bool | `true` |  |
| falco.falcosidekick.config.alertmanager.hostport | string | `"http://alertmanager-operated:9093"` |  |
| falco.falcosidekick.config.alertmanager.minimumpriority | string | `"error"` |  |
| falco.falcosidekick.enabled | bool | `true` |  |
| kube-prometheus-stack.alertmanager.enabled | bool | `true` |  |
| kube-prometheus-stack.enabled | bool | `true` |  |
| kube-prometheus-stack.grafana.additionalDataSources[0].access | string | `"proxy"` |  |
| kube-prometheus-stack.grafana.additionalDataSources[0].name | string | `"Loki"` |  |
| kube-prometheus-stack.grafana.additionalDataSources[0].type | string | `"loki"` |  |
| kube-prometheus-stack.grafana.additionalDataSources[0].url | string | `"http://helx-loki:3100"` | If using a release name other than 'helx' then replace 'helx' with the release name. |
| kube-prometheus-stack.grafana.additionalDataSources[0].version | int | `1` |  |
| kube-prometheus-stack.grafana.enabled | bool | `true` |  |
| kube-prometheus-stack.kubeEtcd.endpoints | object | `{}` | If your etcd is not deployed as a pod, specify IPs it can be found on |
| kube-prometheus-stack.prometheus-node-exporter.resources.limits.cpu | string | `"500m"` |  |
| kube-prometheus-stack.prometheus-node-exporter.resources.limits.memory | string | `"256Mi"` |  |
| kube-prometheus-stack.prometheus-node-exporter.resources.requests.cpu | string | `"250m"` |  |
| kube-prometheus-stack.prometheus-node-exporter.resources.requests.memory | string | `"128Mi"` |  |
| kube-prometheus-stack.prometheus.prometheusSpec.resources.limits.cpu | string | `"1500m"` |  |
| kube-prometheus-stack.prometheus.prometheusSpec.resources.limits.memory | string | `"2000Mi"` |  |
| kube-prometheus-stack.prometheus.prometheusSpec.resources.requests.cpu | string | `"1500m"` |  |
| kube-prometheus-stack.prometheus.prometheusSpec.resources.requests.memory | string | `"2000Mi"` |  |
| kube-prometheus-stack.prometheus.prometheusSpec.ruleSelector | object | `{}` |  |
| kube-prometheus-stack.prometheus.prometheusSpec.ruleSelectorNilUsesHelmValues | bool | `false` |  |
| kube-prometheus-stack.prometheus.prometheusSpec.serviceMonitorSelector | object | `{}` |  |
| kube-prometheus-stack.prometheus.prometheusSpec.serviceMonitorSelectorNilUsesHelmValues | bool | `false` |  |
| kube-prometheus-stack.prometheusOperator.enabled | bool | `true` |  |
| loki.config.limits_config.retention_period | string | `"90d"` |  |
| loki.enabled | bool | `true` |  |
| loki.persistence.accessModes[0] | string | `"ReadWriteOnce"` |  |
| loki.persistence.enabled | bool | `true` |  |
| loki.persistence.size | string | `"10Gi"` |  |
| loki.replicas | int | `3` |  |
| loki.serviceMonitor.enabled | bool | `true` |  |
| promtail.config.lokiAddress | string | `"http://helx-loki:3100/loki/api/v1/push"` | If using a release name other than 'helx' then replace 'helx' with the release name. |
| promtail.enabled | bool | `true` |  |
| promtail.rbac.pspEnabled | bool | `true` |  |

----------------------------------------------
Autogenerated from chart metadata using [helm-docs v1.5.0](https://github.com/norwoodj/helm-docs/releases/v1.5.0)
