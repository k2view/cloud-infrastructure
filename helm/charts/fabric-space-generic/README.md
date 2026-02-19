# Fabric-space

![Version: 1.1.5](https://img.shields.io/badge/Version-1.1.5-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 8.2](https://img.shields.io/badge/AppVersion-8.2-informational?style=flat-square)

The following Helm Chart Deploys Fabric and Postgres. It supports both Deployment (for single-node Fabric) and StatefulSet (for Fabric server cluster) modes.

## Requirements
| Repository | Name | Version |
|------------|------|---------|
| file://./charts/generic-db | generic-db | 1.0.2 |
| file://./charts/fabric     | fabric     | ~1.2.0-0 |

## Deployment Modes

### Deployment Mode
Used for single-node Fabric installations. In this mode:
- Uses a PersistentVolumeClaim for workspace storage
- Supports emptyDir for temporary storage
- Better suited for development and testing environments

### StatefulSet Mode
Used for Fabric server clusters. In this mode:
- Uses volumeClaimTemplates for persistent storage
- Provides stable network identities
- Better suited for production environments

## Values

### Global Configuration
| Key | Type | Default | Description |
|-----|------|---------|-------------|
| app_name | string | `"postgres"` | Name of the application |
| namespace.create | bool | `true` | Flag to create a new Kubernetes namespace |
| namespace.name | string | `"space-tenant"` | Name of the Kubernetes namespace |

### Deployment Configuration
| Key | Type | Default | Description |
|-----|------|---------|-------------|
| deploy.type | string | `"Deployment"` | Deployment type: "Deployment" for single node, "StatefulSet" for cluster |
| labels | list | `[]` | Pod labels |

### Container Configuration
| Key | Type | Default | Description |
|-----|------|---------|-------------|
| container.replicas | int | `1` | Number of container replicas |
| container.image.url | string | `"postgres:15.7"` | Container image URL |
| container.image.repoSecret.enabled | bool | `false` | Enable Docker registry secret |
| container.image.repoSecret.name | string | `"registry-secret"` | Docker registry secret name |
| container.image.repoSecret.dockerRegistry.auths."docker.share.cloud.k2view.com".username | string | `""` | Docker registry username |
| container.image.repoSecret.dockerRegistry.auths."docker.share.cloud.k2view.com".password | string | `""` | Docker registry password |
| container.storage_path | string | `"/opt/apps/pgsql/data/data/"` | Container storage path |
| container.resource_allocation.limits.cpu | string | `"1"` | CPU limit (Deployment: 1, StatefulSet: 2) |
| container.resource_allocation.limits.memory | string | `"4Gi"` | Memory limit (Deployment: 4Gi, StatefulSet: 8Gi) |
| container.resource_allocation.requests.cpu | string | `"0.4"` | CPU request (Deployment: 0.4, StatefulSet: 1) |
| container.resource_allocation.requests.memory | string | `"1Gi"` | Memory request (Deployment: 1Gi, StatefulSet: 4Gi) |

### Health Check Configuration
| Key | Type | Default | Description |
|-----|------|---------|-------------|
| container.livenessProbe.initialDelaySeconds | int | `120` | Initial delay for liveness probe |
| container.livenessProbe.periodSeconds | int | `60` | Period for liveness probe |
| container.readinessProbe.initialDelaySeconds | int | `120` | Initial delay for readiness probe |
| container.readinessProbe.periodSeconds | int | `60` | Period for readiness probe |
| container.readinessProbe.successThreshold | int | `30` | Success threshold for readiness probe |
| container.readinessProbe.failureThreshold | int | `5` | Failure threshold for readiness probe |

### Storage Configuration
| Key | Type | Default | Description |
|-----|------|---------|-------------|
| storage.class | string | `"regional-pd"` | Storage class name |
| storage.allocated_amount | string | `"10Gi"` | Allocated storage size |
| storage.securityContext | bool | `true` | Enable security context |
| storage.pvc.enabled | bool | `true` | Enable PVC (required for Deployment mode) |

### Network Configuration
| Key | Type | Default | Description |
|-----|------|---------|-------------|
| service.port | int | `5432` | Service port |
| networkPolicy.enabled | bool | `true` | Enable network policy |
| listening_port | int | `3213` | Application listening port |

### Ingress Configuration
| Key | Type | Default | Description |
|-----|------|---------|-------------|
| ingress.enabled | bool | `true` | Enable ingress |
| ingress.host | string | `"space-tenant.domain"` | Ingress hostname |
| ingress.port | int | `3213` | Ingress port |
| ingress.annotations | list | `[]` | Ingress annotations |
| ingress.tlsSecret.enabled | bool | `false` | Enable TLS |
| ingress.tlsSecret.crt | string | `""` | TLS certificate |
| ingress.tlsSecret.key | string | `""` | TLS private key |

### Service Account Configuration
| Key | Type | Default | Description |
|-----|------|---------|-------------|
| serviceAccount.create | bool | `true` | Create service account |
| serviceAccount.name | string | `""` | Service account name (if empty, uses Release.Name) |
| serviceAccount.provider | string | `""` | Cloud provider (aws/gcp/azure) |
| serviceAccount.arn | string | `""` | AWS IAM role ARN |
| serviceAccount.gcp_service_account_name | string | `""` | GCP service account name |
| serviceAccount.project_id | string | `""` | GCP project ID |

### Affinity Configuration
| Key | Type | Default | Description |
|-----|------|---------|-------------|
| affinity.type | string | `"none"` | Affinity type (none/affinity/anti-affinity) |
| affinity.label.name | string | `"topology.kubernetes.io/zone"` | Affinity label name |
| affinity.label.value | string | `"region-a"` | Affinity label value |

### Secret Configuration
| Key | Type | Default | Description |
|-----|------|---------|-------------|
| mountSecret.enabled | bool | `false` | Enable config secret mounting |
| mountSecret.name | string | `"config-secrets"` | Config secret name |
| mountSecret.mountPath | string | `"/opt/apps/fabric/config-secrets"` | Config secret mount path |
| mountSecret.data.config | string | `""` | Configuration data |
| mountSecret.data.cp_files | string | `""` | Copy files data |
| mountSecret.data.idp_cert | string | `""` | IDP certificate data |
| mountSecretB64enc.enabled | bool | `false` | Enable base64-encoded secret mounting |
| mountSecretB64enc.name | string | `""` | Base64-encoded secret name |
| mountSecretB64enc.mountPath | string | `""` | Base64-encoded secret mount path |
| secretsList | list | `[]` | List of secrets to mount as environment variables |
| initSecretsList | list | `[]` | List of init container secrets |

### Scaling Configuration
| Key | Type | Default | Description |
|-----|------|---------|-------------|
| scaling.enabled | bool | `false` | Enable auto-scaling |
| scaling.minReplicas | int | `1` | Minimum replicas |
| scaling.maxReplicas | int | `1` | Maximum replicas |
| scaling.targetCPU | int | `90` | Target CPU utilization % |

## Important Notes
### Secret Mounting
1. Config Secrets (`mountSecret`):
   - Must be enabled via `mountSecret.enabled=true`
   - Requires valid secret name and mount path
   - Used for configuration files and certificates

2. Base64 Secrets (`mountSecretB64enc`):
   - Must be enabled via `mountSecretB64enc.enabled=true`
   - Requires valid secret name and mount path
   - Used for encoded configuration data

3. Environment Variable Secrets (`secretsList`):
   - Mounted as environment variables
   - Each secret requires name and data fields

### Resource Allocation
Different resource defaults for Deployment and StatefulSet modes:
- Deployment Mode: Lower resource requirements for single-node setups
- StatefulSet Mode: Higher resource requirements for cluster deployments.


