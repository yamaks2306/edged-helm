# GEL Helm Chart

> ⚠️ **Important Update**: Starting from version 6.0, EdgeDB has been renamed to GEL. This helm chart has been updated to support the new version and name. All references to EdgeDB in code and documentation have been replaced with GEL.

Helm chart for deploying GEL in a Kubernetes cluster.

## Description

This Helm chart allows you to install and configure GEL - the next-generation object-relational database. GEL can be deployed in standalone mode, with existing external PostgreSQL backend, or PostgreSQL can be deployed with the GEL cluster.

## Prerequisites

- Kubernetes 1.19+
- Helm 3.0+
- PV provisioner support in the cluster (for persistent storage)

## Configuration Parameters

### Global Parameters

| Parameter | Description | Default |
|-----------|-------------|---------|
| `nameOverride` | Override chart name | `""` |
| `fullnameOverride` | Override full resource names | `""` |

### Service Account

| Parameter | Description | Default |
|-----------|-------------|---------|
| `serviceAccount.create` | Create Service Account | `true` |
| `serviceAccount.name` | Service Account name | `""` |

### Image Parameters

| Parameter | Description | Default |
|-----------|-------------|---------|
| `image.repository` | GEL image repository | `geldata/gel` |
| `image.tag` | GEL version | `6.5` |
| `image.pullPolicy` | Image pull policy | `IfNotPresent` |
| `imagePullSecrets` | Image pull secrets | `[]` |

### Autoscaling

| Parameter | Description | Default |
|-----------|-------------|---------|
| `autoScaling.enabled` | Enable HPA | `false` |
| `autoScaling.minReplicas` | Minimum replicas | `1` |
| `autoScaling.maxReplicas` | Maximum replicas | `10` |
| `autoScaling.targetCPUUtilizationPercentage` | Target CPU utilization | `80` |
| `autoScaling.targetMemoryUtilizationPercentage` | Target memory utilization | `80` |
| `autoScaling.scaleDown.stabilizationWindowSeconds` | Scale down stabilization window | `300` |
| `autoScaling.scaleUp.stabilizationWindowSeconds` | Scale up stabilization window | `0` |

### Security

| Parameter | Description | Default |
|-----------|-------------|---------|
| `podSecurityContext.fsGroup` | Pod FSGroup | `999` |
| `podSecurityContext.runAsNonRoot` | Run as non-root user | `true` |
| `podSecurityContext.runAsUser` | User ID | `999` |
| `podSecurityContext.runAsGroup` | Group ID | `999` |
| `securityContext.allowPrivilegeEscalation` | Allow privilege escalation | `false` |
| `terminationGracePeriodSeconds` | Grace period for shutdown | `10` |

### Resources

| Parameter | Description | Default |
|-----------|-------------|---------|
| `resources.requests.memory` | Memory request | `512Mi` |
| `resources.requests.cpu` | CPU request | `250m` |
| `resources.limits.memory` | Memory limit | `1Gi` |
| `resources.limits.cpu` | CPU limit | `500m` |

### Health Checks

| Parameter | Description | Default |
|-----------|-------------|---------|
| `readinessProbe.initialDelaySeconds` | Readiness probe initial delay | `5` |
| `readinessProbe.periodSeconds` | Readiness probe period | `10` |
| `livenessProbe.initialDelaySeconds` | Liveness probe initial delay | `15` |
| `livenessProbe.periodSeconds` | Liveness probe period | `20` |

### TLS

| Parameter | Description | Default |
|-----------|-------------|---------|
| `tls.enabled` | Enable TLS | `true` |
| `tls.secretName` | TLS secret name | `""` |
| `tls.duration` | Certificate duration | `2160h` |
| `tls.renewBefore` | Renew certificate before | `360h` |
| `tls.privateKey.algorithm` | Private key algorithm | `ECDSA` |
| `tls.privateKey.size` | Key size | `384` |
| `tls.commonName` | Certificate common name | `gel.example.com` |

### Service

| Parameter | Description | Default |
|-----------|-------------|---------|
| `service.type` | Service type | `