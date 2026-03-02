# postgres

![Version: 1.0.9](https://img.shields.io/badge/Version-1.0.9-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 17.5](https://img.shields.io/badge/AppVersion-17.5-informational?style=flat-square)

Example Helm chart for deploying PostgreSQL on Kubernetes.

## Description
The `postgres` Helm chart is designed to deploy a PostgreSQL database on Kubernetes. PostgreSQL is a powerful, open-source object-relational database system with a strong reputation for reliability, feature robustness, and performance. This chart provides options for configuring the PostgreSQL environment, resource allocation, and network policies to ensure optimal performance and security.


## Values
| Key | Type | Default | Description |
|-----|------|---------|-------------|
| container.envList[0].key | string | `"PGDATA"` | Environment variable key for PostgreSQL data directory |
| container.envList[0].value | string | `"/opt/apps/pgsql/data/data/pgdata"` | Environment variable value for PostgreSQL data directory |
| container.image.repoSecret.dockerRegistry.auths."docker.share.cloud.k2view.com".password | string | `""` | Docker registry password for image pull |
| container.image.repoSecret.dockerRegistry.auths."docker.share.cloud.k2view.com".username | string | `""` | Docker registry username for image pull |
| container.image.repoSecret.enabled | bool | `false` | Enable Docker registry secret for image pull |
| container.image.repoSecret.name | string | `"registry-secret"` | Name of the Docker registry secret |
| container.image.url | string | `"postgres:17.5"` | URL of the PostgreSQL Docker image |
| container.replicas | int | `1` | Number of PostgreSQL replicas |
| container.resource_allocation.limits.cpu | string | `"1"` | CPU limits for PostgreSQL container |
| container.resource_allocation.limits.memory | string | `"4Gi"` | Memory limits for PostgreSQL container |
| container.resource_allocation.requests.cpu | string | `"0.4"` | CPU requests for PostgreSQL container |
| container.resource_allocation.requests.memory | string | `"2Gi"` | Memory requests for PostgreSQL container |
| credentials.postgres_password | string | `"postgres"` | Password for PostgreSQL |
| credentials.postgres_username | string | `"postgres"` | Username for PostgreSQL |
| labels | list | `[]` | A list of labels to be applied to Kubernetes resources. |
| annotations | list | `[]` | A list of annotations (name/value pairs) for Kubernetes resources. |
| listening_port | int | `5432` | Port for PostgreSQL to listen on |
| namespace.create | bool | `true` | Enable creation of namespace |
| namespace.name | string | `"space-tenant"` | Namespace for PostgreSQL deployment |
| networkPolicy.enabled | bool | `true` | Enable network policy for PostgreSQL |
| storage.allocated_amount | string | `"10Gi"` | Amount of storage allocated for PostgreSQL |
| storage.class | string | `"gp2"` | Storage class for PostgreSQL data |
| affinity.type | string | `"none"` | Specifies the type of affinity rule to apply. Options: `affinity`, `anti-affinity`, `none`. |
| affinity.label | object | `{}` | Label configuration for affinity rules. |
| affinity.label.name | string | `""` | The key of the label to be used for affinity rules. For example: `topology.kubernetes.io/zone`. |
| affinity.label.value | string | `""` | The value of the label to be used for affinity rules. For example: `region-a`. |

## Installation
### Install from helm repo
1. Add repo
```bash
helm repo add postgres https://nexus.share.cloud.k2view.com/repository/postgres
```

2. Install
```bash
helm install postgres/postgres postgres
```
