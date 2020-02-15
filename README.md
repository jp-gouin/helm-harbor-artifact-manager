# Helm Chart for Artifact Manager

## Introduction

Artifact manager is an app that help you manage your private catalog of services on Harbor.

See which chart are using your private docker images.

1. Scan your repository
2. Scan your docker registry
3. perform analisys and correlation
4. Store all data in a ETCD database for high performance

Validate a chart and a docker image by adding a specific label.

Create project and add your artifact in it !

Once a project is created and artifact are added in it, you can generate a script to build deliveries for air gap installation.

Ldap authentication for the Webui

## Prerequisites

- Kubernetes cluster 1.10+
- Helm 2.8.0+
- PV provisioner support in the underlying infrastructure.

## Installation

### Add Helm repository

```bash
helm repo add i3s <your-repo>
helm repo update
```

### Configure the chart

The following items can be set via `--set` flag during installation or configured by editing the `values.yaml` directly (you need to download the chart first).

#### Configure the way how to expose phpLDAPadmin service:

- **Ingress**: The ingress controller must be installed in the Kubernetes cluster.
- **ClusterIP**: Exposes the service on a cluster-internal IP. Choosing this value makes the service only reachable from within the cluster.
- **NodePort**: Exposes the service on each Node’s IP at a static port (the NodePort). You’ll be able to contact the NodePort service, from outside the cluster, by requesting `NodeIP:NodePort`.
- **LoadBalancer**: Exposes the service externally using a cloud provider’s load balancer.

#### Configure how to persist data (TODO):

- **Disable**: The data does not survive the termination of a pod.
- **Persistent Volume Claim(default)**: A default `StorageClass` is needed in the Kubernetes cluster to dynamic provision the volumes. Specify another StorageClass in the `storageClass` or set `existingClaim` if you have already existing persistent volumes to use.

### Install the chart

Install the phpLDAPadmin helm chart with a release name `my-release`:

```bash
helm install --name my-release cetic/phpldapadmin
```

## Uninstallation

To uninstall/delete the `my-release` deployment:

```bash
helm delete --purge my-release
```

## Configuration

The following table lists the configurable parameters of the phpLDAPadmin chart and the default values.

| Parameter                                                                   | Description                                                                                                        | Default                         |
| --------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------| ------------------------------- |
| **ReplicaCount**                                                            |
| `replicaCount`                                                              | number of phpLDAPadmin images                                                                                               | `1`      |
| **Env**                                                                     |
| `env`                                                                       | See values.yaml                                                                                                           | `nil`      |
| **Image**                                                                   |
| `image.repository`                                                          | phpldapadmin Image name                                                                                                 | `osixia/phpldapadmin`      |
| `image.tag`                                                                 | phpldapadmin Image tag                                                                                                  | `0.7.1`                    |
| `image.pullPolicy`                                                          | phpldapadmin Image pull policy                                                                                          | `IfNotPresent`             |
| **Service**                                                                 |
| `service.type`                                                              | Type of service for phpldapadmin frontend                                                                               | `LoadBalancer`             |
| `service.port`                                                              | Port to expose service                                                                                             | `80`                            |
| `service.loadBalancerIP`                                                    | LoadBalancerIP if service type is `LoadBalancer`                                                                   | `nil`                           |
| `service.loadBalancerSourceRanges`                                          | LoadBalancerSourceRanges                                                                                           | `nil`                           |
| `service.annotations`                                                       | Service annotations                                                                                                | `{}`                            |
| **Ingress**                                                                 |
| `ingress.enabled`                                                           | Enables Ingress                                                                                                    | `false`                         |
| `ingress.annotations`                                                       | Ingress annotations                                                                                                | `{}`                            |
| `ingress.path`                                                              | Path to access frontend                                                                                            | `/`                             |
| `ingress.hosts`                                                             | Ingress hosts                                                                                                      | `nil`                           |
| `ingress.tls`                                                               | Ingress TLS configuration                                                                                          | `[]`                            |
| **ReadinessProbe**                                                          |
| `readinessProbe`                                                            | Rediness Probe settings                                                                                            | `{ "httpGet": { "path": "/", "port": http }}`|
| **LivenessProbe**                                                           |
| `livenessProbe`                                                             | Liveness Probe settings                                                                                            | `{ "httpGet": { "path": "/", "port": http }}`|
| **Resources**                                                               |
| `resources`                                                                 | CPU/Memory resource requests/limits                                                                                | `{}`                            |
| **nodeSelector**                                                            |
| `nodeSelector`                                                              | nodeSelector                                                                                                       | `{}`                            |
| **tolerations**                                                             |
| `tolerations`                                                               | tolerations                                                                                                        | `{}`                            |
| **affinity**                                                                |
| `affinity`                                                                  | affinity                                                                                                           | `{}`                            |

## Credits

Initially inspired from https://github.com/gengen1988/helm-phpldapadmin.

## Contributing

Feel free to contribute by making a [pull request](https://github.com/cetic/helm-phpLDAPadmin/pull/new/master).

Please read the official [Contribution Guide](https://github.com/helm/charts/blob/master/CONTRIBUTING.md) from Helm for more information on how you can contribute to this Chart.

## License

[Apache License 2.0](/LICENSE)
