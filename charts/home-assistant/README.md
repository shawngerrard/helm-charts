# Helm chart for Home Assistant

![Latest Released Version](https://img.shields.io/github/v/tag/pajikos/home-assistant-helm-chart?sort=semver)
![Helm Chart Release](https://github.com/pajikos/home-assistant-helm-chart/actions/workflows/build-helm-chart-release.yaml/badge.svg)
![Auto-update latest HA version](https://github.com/pajikos/home-assistant-helm-chart/actions/workflows/check_ha_release.yml/badge.svg)
[![Artifact Hub](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/helm-hass)](https://artifacthub.io/packages/search?repo=helm-hass)

## Introduction

This chart bootstraps a [Home Assistant](https://home-assistant.io) deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager. 

It is updated **automatically** with each new release of Home Assistant, ensuring you always have access to the latest features and improvements.
  
## Installing the Chart

To install the chart with the release name `home-assistant`:

```console
$ helm repo add pajikos http://pajikos.github.io/home-assistant-helm-chart/
$ helm repo update
$ helm install home-assistant pajikos/home-assistant
```

The command deploys Home Assistant on the Kubernetes cluster in the default configuration. The [configuration](#configuration) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `home-assistant` deployment:

```console
$ helm delete home-assistant
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

The following table lists the configurable parameters of the Home Assistant chart and their default values.

# Home Assistant Helm Chart

This document provides detailed configuration options for the Home Assistant Helm chart.

| Parameter | Description | Default |
| --------- | ----------- | ------- |
| `replicaCount` | Number of replicas for the deployment | `1` |
| `image.repository` | Repository for the Home Assistant image | `ghcr.io/home-assistant/home-assistant` |
| `image.pullPolicy` | Image pull policy | `IfNotPresent` |
| `image.tag` | Overrides the image tag (default is the chart appVersion) | `""` |
| `image.imagePullSecrets` | List of imagePullSecrets for private image repositories | `[]` |
| `nameOverride` | Override the default name of the Helm chart | `""` |
| `fullnameOverride` | Override the default full name of the Helm chart | `""` |
| `serviceAccount.create` | Specifies whether a service account should be created | `true` |
| `serviceAccount.annotations` | Annotations to add to the service account | `{}` |
| `serviceAccount.name` | The name of the service account to use | `""` |
| `podAnnotations` | Annotations to add to the pod | `{}` |
| `podSecurityContext` | Pod security context settings | `{}` |
| `env` | Environment variables | `[]` |
| `envFrom` | Use environment variables from ConfigMaps or Secrets | `[]` |
| `hostNetwork` | Specifies if the containers should be started in `hostNetwork` mode. | `false` |
| `hostPort.enabled` | Enable 'hostPort' or not | `false` |
| `hostPort.port` | Port number | `8123` |
| `dnsConfig` | Override the default dnsConfig and set your own nameservers or ndots, among other options | `{}` |
| `service.type` | Service type (ClusterIP, NodePort, LoadBalancer, or ExternalName) | `ClusterIP` |
| `service.port` | Service port | `8080` |
| `service.annotations` | Annotations to add to the service | `{}` |
| `ingress.enabled` | Enable ingress for Home Assistant | `false` |
| `resources` | Resource settings for the container | `{}` |
| `nodeSelector` | Node selector settings for scheduling the pod on specific nodes | `{}` |
| `tolerations` | Tolerations settings for scheduling the pod based on node taints | `[]` |
| `affinity` | Affinity settings for controlling pod scheduling | `{}` |
| `persistence.enabled` | Enable or disable persistence | `true` |
| `persistence.accessMode` | Access mode for the persistent volume claim | `ReadWriteOnce` |
| `persistence.size` | Size of the persistent volume claim | `5Gi` |
| `persistence.storageClass` | Storage class for the persistent volume claim | `""` |
| `additionalVolumes` | Additional volumes to be mounted in the home assistant container | `[]` |
| `additionalVolumeMounts` | Additional volume mounts to be mounted in the home assistant container | `[]` |
| `addons.codeserver.enabled` | Enable or disable the code-server addon | `false` |
| `addons.codeserver.resources` | Resource settings for the code-server container | `{}` |
| `addons.codeserver.image.repository` | Repository for the code-server image | `ghcr.io/coder/code-server` |
| `addons.codeserver.image.pullPolicy` | Image pull policy for the code-server image | `IfNotPresent` |
| `addons.codeserver.image.tag` | Tag for the code-server image | `latest released version, automatically updated` |
| `addons.codeserver.service.type` | Service type for the code-server addon | `ClusterIP` |
| `addons.codeserver.service.port` | Service port for the code-server addon | `12321` |
| `addons.codeserver.ingress.enabled` | Enable or disable the ingress for the code-server addon | `false` |
| `addons.codeserver.ingress.hosts` | Hosts for the code-server addon | `[]` |
| `addons.codeserver.ingress.tls` | TLS settings for the code-server addon | `[]` |
| `addons.codeserver.ingress.annotations` | Annotations for the code-server addon | `{}` |

## Persistence

The default configuration of this chart uses an emptyDir volume for persistence, which is lost when the pod is removed.
To enable persistence, set `persistence.enabled` to `true`. In addition, you can specify the `persistence.storageClass` or `persistence.accessMode` values. The default values are `ReadWriteOnce` and `""` respectively.

## Ingress

To enable ingress for Home Assistant, set `ingress.enabled` to `true`. In addition, you can specify the `ingress.hosts` and `ingress.tls` values. The default values are `[]` and `[]` respectively.
The second option is to set `service.type` to `NodePort` or `LoadBalancer` (when ingress is not available in your cluster)

## HostPort and HostNetwork

To enable hostPort, set `hostPort.enabled` to `true`. In addition, you can specify the `hostPort.port` value. The default value is `8123`.
To enable hostNetwork, set `hostNetwork` to `true`.
HostNetwork is required for auto-discovery of Home Assistant, when not using auto-discovery, hostNetwork is not required and not recommended.

## Addons

The Home Assistant chart supports the following addons:

* [code-server](https://github.com/coder/code-server)

## Additional volumes and volume mounts

To add additional volumes and volume mounts, you can use the `additionalVolumes` and `additionalVolumeMounts` values. The default values are `[]`.
Example mounting usb devices:

```yaml

additionalVolumes:
  - hostPath:
      path: >-
        /dev/serial/by-id/usb-ITEAD_SONOFF_Zigbee_3.0_USB_Dongle_Plus_V2_20230509111242-if00
      type: CharDevice
    name: usb

additionalMounts:
  - mountPath: /dev/ttyACM0
    name: usb

```

Note: When mounting usb devices, you need to set the `securityContext.privileged` value to `true`. 

### code-server

To enable the code-server addon, set `addons.codeserver.enabled` to `true`. In addition, you can specify the `addons.codeserver.resources` values. The default value is `{}`.
To be able to access the code-server addon, you need to enable the ingress for the code-server addon by setting `addons.codeserver.ingress.enabled` to `true` or setting `service.type` to `NodePort` or `LoadBalancer`.
