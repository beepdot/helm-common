### Common library chart
A library helm chart for common templates

```
dependencies:
  - name: common
    version: 1.x.x
    repository:
```
Run the below command to update dependencies
```
$ helm dependency update
```

## Introduction

This chart provides a common template helpers which can be used to develop new charts using [Helm](https://helm.sh) package manager.

## Prerequisites

- Kubernetes 1.19+
- Helm 3.2.0+

## Parameters

The following table lists the helpers available in the library which are scoped in different sections.

### Images

| Helper identifier           | Description                                          | Expected Input                                                                                          |
|-----------------------------|------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
| `common.images.image`       | Return the proper and full image name                | `dict "imageRoot" .Values.path.to.the.image "global" $`, see [ImageRoot](#imageroot) for the structure. |
| `common.images.renderPullSecrets` | Return the proper Docker Image Registry Secret Names (evaluates values as templates) | `dict "images" (list .Values.path.to.the.image1, .Values.path.to.the.image2) "context" $` |

### Labels

| Helper identifier           | Description                                                                 | Expected Input    |
|-----------------------------|-----------------------------------------------------------------------------|-------------------|
| `common.labels.standard`    | Return Kubernetes standard labels                                           | `.` Chart context |
| `common.labels.matchLabels` | Labels to use on `deploy.spec.selector.matchLabels` and `svc.spec.selector` | `.` Chart context |

### Names

| Helper identifier                 | Description                                                           | Expected Input    |
|-----------------------------------|-----------------------------------------------------------------------|-------------------|
| `common.names.name`               | Expand the name of the chart or use `.Values.nameOverride`            | `.` Chart context |
| `common.names.fullname`           | Create a default fully qualified app name.                            | `.` Chart context |
| `common.names.namespace`          | Allow the release namespace to be overridden                          | `.` Chart context |
| `common.names.fullname.namespace` | Create a fully qualified app name adding the installation's namespace | `.` Chart context |
| `common.names.chart`              | Chart name plus version                                               | `.` Chart context |

### Storage

| Helper identifier             | Description                           | Expected Input                                                                                                      |
|-------------------------------|---------------------------------------|---------------------------------------------------------------------------------------------------------------------|
| `common.storage.class` | Return  the proper Storage Class | `dict "persistence" .Values.path.to.the.persistence "global" $`, see [Persistence](#persistence) for the structure. |

### Capabilities

| Helper identifier                              | Description                                                                                    | Expected Input    |
|------------------------------------------------|------------------------------------------------------------------------------------------------|-------------------|
| `common.capabilities.kubeVersion`              | Return the target Kubernetes version (using client default if .Values.kubeVersion is not set). | `.` Chart context |
| `common.capabilities.cronjob.apiVersion`       | Return the appropriate apiVersion for cronjob.                                                 | `.` Chart context |
| `common.capabilities.deployment.apiVersion`    | Return the appropriate apiVersion for deployment.                                              | `.` Chart context |
| `common.capabilities.statefulset.apiVersion`   | Return the appropriate apiVersion for statefulset.                                             | `.` Chart context |
| `common.capabilities.ingress.apiVersion`       | Return the appropriate apiVersion for ingress.                                                 | `.` Chart context |
| `common.capabilities.rbac.apiVersion`          | Return the appropriate apiVersion for RBAC resources.                                          | `.` Chart context |
| `common.capabilities.crd.apiVersion`           | Return the appropriate apiVersion for CRDs.                                                    | `.` Chart context |
| `common.capabilities.policy.apiVersion`        | Return the appropriate apiVersion for podsecuritypolicy.                                       | `.` Chart context |
| `common.capabilities.networkPolicy.apiVersion` | Return the appropriate apiVersion for networkpolicy.                                           | `.` Chart context |
| `common.capabilities.apiService.apiVersion`    | Return the appropriate apiVersion for APIService.                                              | `.` Chart context |
| `common.capabilities.hpa.apiVersion`           | Return the appropriate apiVersion for Horizontal Pod Autoscaler                                | `.` Chart context |
| `common.capabilities.supportsHelmVersion`      | Returns true if the used Helm version is 3.3+                                                  | `.` Chart context |


## Special input schemas

### ImageRoot

```yaml
registry:
  type: string
  description: Docker registry where the image is located
  example: docker.io

repository:
  type: string
  description: Repository and image name
  example: bitnami/nginx

tag:
  type: string
  description: image tag
  example: 1.16.1-debian-10-r63

pullPolicy:
  type: string
  description: Specify a imagePullPolicy. Defaults to 'Always' if image tag is 'latest', else set to 'IfNotPresent'

pullSecrets:
  type: array
  items:
    type: string
  description: Optionally specify an array of imagePullSecrets (evaluated as templates).

debug:
  type: boolean
  description: Set to true if you would like to see extra information on logs
  example: false

## An instance would be:
# registry: docker.io
# repository: bitnami/nginx
# tag: 1.16.1-debian-10-r63
# pullPolicy: IfNotPresent
# debug: false
```

### Persistence

```yaml
enabled:
  type: boolean
  description: Whether enable persistence.
  example: true

storageClass:
  type: string
  description: Ghost data Persistent Volume Storage Class, If set to "-", storageClassName: "" which disables dynamic provisioning.
  example: "-"

accessMode:
  type: string
  description: Access mode for the Persistent Volume Storage.
  example: ReadWriteOnce

size:
  type: string
  description: Size the Persistent Volume Storage.
  example: 8Gi

path:
  type: string
  description: Path to be persisted.
  example: /bitnami

## An instance would be:
# enabled: true
# storageClass: "-"
# accessMode: ReadWriteOnce
# size: 8Gi
# path: /bitnami
```