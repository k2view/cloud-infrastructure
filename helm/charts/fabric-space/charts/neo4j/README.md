# neo4j
![Version: 1.0.1](https://img.shields.io/badge/Version-1.0.1-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 5.18.0](https://img.shields.io/badge/AppVersion-5.18.0-informational?style=flat-square)

This Helm chart supports highly available Neo4j configurations, tailored for K2view Fabric use case.

## Values
| Key | Type | Default | Description |
|-----|------|---------|-------------|
| container.envList[0].key | string | `"NEO4JLABS_PLUGINS"` | Environment variable key for Neo4j plugins |
| container.envList[0].value | string | `"[\"graph-data-science\"]"` | Environment variable value for Neo4j plugins |
| container.image.repoSecret.dockerRegistry.auths."docker.share.cloud.k2view.com".password | string | `""` | Docker registry password for image pull |
| container.image.repoSecret.dockerRegistry.auths."docker.share.cloud.k2view.com".username | string | `""` | Docker registry username for image pull |
| container.image.repoSecret.enabled | bool | `false` | Enable Docker registry secret for image pull |
| container.image.repoSecret.name | string | `"registry-secret"` | Name of the Docker registry secret |
| container.image.url | string | `"neo4j:5.18-enterprise"` | URL of the Neo4j Docker image |
| container.resource_allocation.limits.cpu | string | `"1"` | CPU limits for Neo4j container |
| container.resource_allocation.limits.memory | string | `"4Gi"` | Memory limits for Neo4j container |
| container.resource_allocation.requests.cpu | string | `"0.4"` | CPU requests for Neo4j container |
| container.resource_allocation.requests.memory | string | `"2Gi"` | Memory requests for Neo4j container |
| container.storage_path | string | `"/var/lib/neo4j/data"` | Path for Neo4j data storage |
| credentials.neo4j_password | string | `"changeit"` | Password for Neo4j |
| credentials.neo4j_username | string | `"neo4j"` | Username for Neo4j |
| global.labels[0].name | string | `"tenant"` | Global label key for tenant |
| global.labels[0].value | string | `"my-tenant"` | Global label value for tenant |
| global.labels[1].name | string | `"space"` | Global label key for space |
| global.labels[1].value | string | `"my-space"` | Global label value for space |
| global.namespace.create | bool | `false` | Enable creation of global namespace |
| global.namespace.name | string | `"space-tenant"` | Global namespace for Neo4j deployment |
| labels[0].name | string | `"tenant"` | Label key for tenant |
| labels[0].value | string | `"my-tenant"` | Label value for tenant |
| labels[1].name | string | `"space"` | Label key for space |
| labels[1].value | string | `"my-space"` | Label value for space |
| namespace.name | string | `"space-tenant"` | Namespace for Neo4j deployment |
| networkPolicy.enabled | bool | `true` | Enable network policy for Neo4j |
| storage.alocated_amount | string | `"10Gi"` | Amount of storage allocated for Neo4j |
| storage.class | string | `"efs-pg"` | Storage class for Neo4j data |
| affinity.type | string | `"none"` | Specifies the type of affinity rule to apply. Options: `affinity`, `anti-affinity`, `none`. |
| affinity.label | object | `{}` | Label configuration for affinity rules. |
| affinity.label.name | string | `""` | The key of the label to be used for affinity rules. For example: `topology.kubernetes.io/zone`. |
| affinity.label.value | string | `""` | The value of the label to be used for affinity rules. For example: `region-a`. |

## Installation
### Install from helm repo
1. Add repo
```bash
helm repo add neo4j https://nexus.share.cloud.k2view.com/repository/neo4j
```

2. Install
```bash
helm install neo4j/neo4j neo4j
```
