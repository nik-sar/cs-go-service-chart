# cs-go-service-chart

Helm chart for deploying Go microservices to Kubernetes. Provides configurable Deployment, Service, Ingress, and CronJob resources.

## Features

- Configurable resource limits and requests
- Support for multiple replicas
- Environment variables via ConfigMaps, Secrets, and direct values
- HAProxy Ingress with URL rewrite support
- Optional CronJob workloads

## Installation

```bash
helm repo add cs-go-service-chart https://nik-sar.github.io/cs-go-service-chart/
helm repo update
helm install app-name cs-go-service-chart/cs-go-service-chart
```

## Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `replicaCount` | Number of pod replicas | `1` |
| `image.name` | Container image name | `sample.host/sample-svc` |
| `image.tag` | Image tag | `stable` |
| `image.pullPolicy` | Image pull policy | `IfNotPresent` |
| `resources.limits.memory` | Memory limit | `256Mi` |
| `resources.limits.cpu` | CPU limit | `200m` |
| `resources.requests.memory` | Memory request | `100Mi` |
| `resources.requests.cpu` | CPU request | `100m` |
| `service.type` | Service type | `ClusterIP` |
| `service.port` | Service port | `8080` |
| `service.targetPort` | Container port | `8080` |
| `ingress.enabled` | Enable Ingress | `false` |
| `ingress.rules` | Ingress rules with rewrite targets | `[]` |
| `secrets` | Environment variables from secrets | `[]` |
| `configMaps` | Environment variables from configmaps | `[]` |
| `environments` | Direct environment variables | `[]` |
| `crones` | CronJob configurations | `[]` |
| `imagePullSecrets` | Image pull secrets | `[]` |

### Ingress Configuration Example

```yaml
ingress:
  enabled: true
  rules:
    - rewriteTarget: "/api/rest"
      hosts:
        - host: "sample.host"
          paths:
            - path: "/api/rest"
              pathType: Prefix
```

### Environment Variables Example

```yaml
environments:
  - envName: APP_ENV
    envValue: "production"

secrets:
  - envName: DB_PASSWORD
    secretName: db-secret
    key: password

configMaps:
  - envName: LOG_LEVEL
    configName: app-config
    key: logLevel
```

### CronJob Example

```yaml
crones:
  - name: sample-crone
    enabled: true
    schedule: "0 */2 * * *"
    image:
      name: "curlimages/curl"
      tag: "1.0.3"
    args:
      - "-X"
      - "POST"
      - "sample/cron"
```