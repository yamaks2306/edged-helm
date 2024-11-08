# EdgeDB Helm Chart

Helm chart for deploying EdgeDB in a Kubernetes cluster.

## Description

This Helm chart allows you to install and configure EdgeDB - the next-generation object-relational database. EdgeDB can be deployed in standalone mode, with existing external PostgreSQL backend, or PostgreSQL can be deployed with the EdgeDB cluster.

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
| `image.repository` | EdgeDB image repository | `edgedb/edgedb` |
| `image.tag` | EdgeDB version | `5.5` |
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
| `tls.commonName` | Certificate common name | `edgedb.example.com` |

### Service

| Parameter | Description | Default |
|-----------|-------------|---------|
| `service.type` | Service type | `ClusterIP` |
| `service.port` | Service port | `5656` |
| `service.name` | Service name | `edgedb` |

### Ingress

| Parameter | Description | Default |
|-----------|-------------|---------|
| `ingress.enabled` | Enable ingress | `false` |
| `ingress.className` | Ingress class name | `nginx` |
| `ingress.annotations` | Ingress annotations | `{}` |
| `ingress.hosts` | List of hosts | `[]` |

### EdgeDB

| Parameter | Description | Default |
|-----------|-------------|---------|
| `edgedb.adminUI` | Enable Admin UI | `enabled` |
| `edgedb.auth.username` | Username | `edgedb` |
| `edgedb.auth.password` | Password | `your-edgedb-password` |
| `edgedb.logLevel` | Log level | `info` |
| `edgedb.storage.size` | Storage size | `5Gi` |
| `edgedb.storage.storageClassName` | Storage Class | `do-block-storage` |
| `edgedb.defaultAuthMethod` | Default auth method | `""` |
| `edgedb.httpEndpointSecurity` | HTTP endpoint security | `""` |
| `edgedb.serverSecurity` | Server security mode | `""` |

### PostgreSQL

| Parameter | Description | Default |
|-----------|-------------|---------|
| `postgresdb.postgresdbExternal.enabled` | Use external PostgreSQL | `false` |
| `postgresdb.postgresdbExternal.dsn` | External DB DSN | `""` |
| `postgresdb.postgresdbInternal.enabled` | Deploy PostgreSQL | `true` |
| `postgresdb.postgresdbInternal.database` | Database name | `edgedb` |
| `postgresdb.postgresdbInternal.instances` | Number of instances | `3` |
| `postgresdb.postgresdbInternal.storage.size` | Storage size | `5Gi` |
| `postgresdb.postgresdbInternal.auth.username` | Username | `postgres` |
| `postgresdb.postgresdbInternal.auth.password` | Password | `super-secret-password` |

### Additional Parameters

| Parameter | Description | Default |
|-----------|-------------|---------|
| `nodeSelector` | Node selectors | `{}` |
| `tolerations` | Tolerations | `[]` |
| `affinity` | Affinity rules | `{}` |

## Usage

```bash
helm install my-edgedb ./edgedb \
  --set edgedb.auth.password=mypassword \
```

## License

MIT License
