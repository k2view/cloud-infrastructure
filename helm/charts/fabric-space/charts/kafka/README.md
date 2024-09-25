# kafka

![Version: 1.0.2](https://img.shields.io/badge/Version-1.0.2-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 7.2.0](https://img.shields.io/badge/AppVersion-7.2.0-informational?style=flat-square)

This Helm chart deploys Kafka on Kubernetes.

## Description
The kafka Helm chart is designed to deploy Apache Kafka, a distributed event streaming platform, on Kubernetes. Kafka is widely used for building real-time data pipelines and streaming applications. This chart includes options for configuring Kafka's storage, resource allocation, and network policies to ensure optimal performance and security.

## Values
| Key | Type | Default | Description |
|-----|------|---------|-------------|
| container.envList[0].key | string | `"DATA"` | Environment variable key for data path |
| container.envList[0].value | string | `"/home/kafka/zk_data"` | Environment variable value for data path |
| container.image.repoSecret.dockerRegistry.auths."docker.share.cloud.k2view.com".password | string | `""` | Docker registry password for image pull |
| container.image.repoSecret.dockerRegistry.auths."docker.share.cloud.k2view.com".username | string | `""` | Docker registry username for image pull |
| container.image.repoSecret.enabled | bool | `false` | Enable Docker registry secret for image pull |
| container.image.repoSecret.name | string | `"registry-secret"` | Name of the Docker registry secret |
| container.image.url | string | `"329508970117.dkr.ecr.eu-central-1.amazonaws.com/k2view_shared:kafka_7.2"` | URL of the Kafka Docker image |
| container.replicas | int | `1` | Number of Kafka broker replicas |
| container.resource_allocation.limits.cpu | string | `"1"` | CPU limits for Kafka container |
| container.resource_allocation.limits.memory | string | `"4Gi"` | Memory limits for Kafka container |
| container.resource_allocation.requests.cpu | string | `"0.4"` | CPU requests for Kafka container |
| container.resource_allocation.requests.memory | string | `"2Gi"` | Memory requests for Kafka container |
| container.storage_path | string | `"/home/kafka/zk_data"` | Path for Kafka data storage |
| global.labels[0].name | string | `"tenant"` | Global label key for tenant |
| global.labels[0].value | string | `"my-tenant"` | Global label value for tenant |
| global.labels[1].name | string | `"space"` | Global label key for space |
| global.labels[1].value | string | `"my-space"` | Global label value for space |
| global.namespace.name | string | `"space-tenant"` | Global namespace for Kafka deployment |
| labels[0].name | string | `"tenant"` | Label key for tenant |
| labels[0].value | string | `"my-tenant"` | Label value for tenant |
| labels[1].name | string | `"space"` | Label key for space |
| labels[1].value | string | `"my-space"` | Label value for space |
| listening_port | int | `9093` | Port for Kafka to listen on |
| namespace.name | string | `"space-tenant"` | Namespace for Kafka deployment |
| networkPolicy.enabled | bool | `false` | Enable network policy for Kafka |
| storage.alocated_amount | string | `"10Gi"` | Amount of storage allocated for Kafka |
| storage.class | string | `"efs-kafka"` | Storage class for Kafka data |
| affinity.type | string | `"none"` | Specifies the type of affinity rule to apply. Options: `affinity`, `anti-affinity`, `none`. |
| affinity.label | object | `{}` | Label configuration for affinity rules. |
| affinity.label.name | string | `""` | The key of the label to be used for affinity rules. For example: `failure-domain.beta.kubernetes.io/zone`. |
| affinity.label.value | string | `""` | The value of the label to be used for affinity rules. For example: `region-a`. |

## Installation
### Install from helm repo
1. Add repo
```bash
helm repo add kafka https://nexus.share.cloud.k2view.com/repository/kafka
```

2. Install
```bash
helm install kafka/kafka kafka
```
