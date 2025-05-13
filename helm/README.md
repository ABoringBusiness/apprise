# Apprise Helm Chart

This Helm chart deploys [Apprise](https://github.com/caronc/apprise) on a Kubernetes cluster.

Apprise is a push notification library that allows you to send notifications to almost all of the most popular notification services available today such as: Telegram, Discord, Slack, Amazon SNS, Gotify, etc.

## Prerequisites

- Kubernetes 1.19+
- Helm 3.2.0+

## Installing the Chart

To install the chart with the release name `my-apprise`:

```bash
helm install my-apprise /path/to/apprise-chart
```

## Uninstalling the Chart

To uninstall/delete the `my-apprise` deployment:

```bash
helm delete my-apprise
```

## Parameters

### Global parameters

| Name                      | Description                                     | Value |
| ------------------------- | ----------------------------------------------- | ----- |
| `replicaCount`            | Number of replicas to deploy                    | `1`   |
| `image.repository`        | Apprise image repository                        | `caronc/apprise` |
| `image.tag`               | Apprise image tag (defaults to Chart appVersion)| `""`  |
| `image.pullPolicy`        | Apprise image pull policy                       | `IfNotPresent` |
| `imagePullSecrets`        | Image pull secrets                              | `[]`  |
| `nameOverride`            | String to partially override apprise.fullname   | `""`  |
| `fullnameOverride`        | String to fully override apprise.fullname       | `""`  |

### Apprise parameters

| Name                                 | Description                                                | Value |
| ------------------------------------ | ---------------------------------------------------------- | ----- |
| `apprise.persistence.enabled`        | Enable persistence using PVC                               | `false` |
| `apprise.persistence.storageClass`   | PVC Storage Class for Apprise data volume                  | `""` |
| `apprise.persistence.accessMode`     | PVC Access Mode for Apprise data volume                    | `ReadWriteOnce` |
| `apprise.persistence.size`           | PVC Storage Request for Apprise data volume                | `1Gi` |
| `apprise.env`                        | Environment variables to pass to the container              | `[]` |
| `apprise.configFiles`                | Config files to mount as ConfigMaps                        | `{}` |
| `apprise.args`                       | Command line arguments to pass to the apprise command      | `[]` |

### Service parameters

| Name                      | Description                                     | Value |
| ------------------------- | ----------------------------------------------- | ----- |
| `service.type`            | Kubernetes Service type                         | `ClusterIP` |
| `service.port`            | Service HTTP port                               | `8000` |

### Ingress parameters

| Name                      | Description                                     | Value |
| ------------------------- | ----------------------------------------------- | ----- |
| `ingress.enabled`         | Enable ingress controller resource              | `false` |
| `ingress.className`       | IngressClass that will be used                  | `""` |
| `ingress.annotations`     | Additional annotations for the Ingress resource | `{}` |
| `ingress.hosts`           | List of hosts for the ingress                   | See values.yaml |
| `ingress.tls`             | TLS configuration for the ingress               | `[]` |

### Other parameters

| Name                                 | Description                                                | Value |
| ------------------------------------ | ---------------------------------------------------------- | ----- |
| `serviceAccount.create`              | Specifies whether a ServiceAccount should be created       | `true` |
| `serviceAccount.annotations`         | Annotations to add to the ServiceAccount                   | `{}` |
| `serviceAccount.name`                | The name of the ServiceAccount to use                      | `""` |
| `podAnnotations`                     | Annotations for Apprise pods                               | `{}` |
| `podSecurityContext`                 | Security context for Apprise pods                          | `{}` |
| `securityContext`                    | Security context for Apprise container                     | `{}` |
| `resources`                          | CPU/Memory resource requests/limits                        | `{}` |
| `nodeSelector`                       | Node labels for pod assignment                             | `{}` |
| `tolerations`                        | Tolerations for pod assignment                             | `[]` |
| `affinity`                           | Affinity for pod assignment                                | `{}` |
| `autoscaling.enabled`                | Enable autoscaling for Apprise deployment                  | `false` |
| `autoscaling.minReplicas`            | Minimum number of replicas                                 | `1` |
| `autoscaling.maxReplicas`            | Maximum number of replicas                                 | `100` |
| `autoscaling.targetCPUUtilizationPercentage` | Target CPU utilization percentage                  | `80` |
| `autoscaling.targetMemoryUtilizationPercentage` | Target Memory utilization percentage            | `nil` |

## Configuration

### Using custom configuration

You can specify custom configuration files using the `apprise.configFiles` parameter:

```yaml
apprise:
  configFiles:
    config.yml: |
      # Your Apprise configuration here
```

These files will be mounted as a ConfigMap at `/config` in the container.

### Persistence

The chart mounts a Persistent Volume at `/data` if persistence is enabled. To enable persistence:

```yaml
apprise:
  persistence:
    enabled: true
    # storageClass: "-"
    accessMode: ReadWriteOnce
    size: 1Gi
```

## Usage

Once the chart is installed, you can use the Apprise service within your cluster to send notifications.