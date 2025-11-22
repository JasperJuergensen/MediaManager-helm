# MediaManager Helm Chart

This repository contains a Helm chart to deploy MediaManager (https://github.com/maxdorninger/MediaManager) on Kubernetes.

The chart packages the MediaManager application and provides configurable options for image, service, ingress, persistence and optional PostgreSQL dependency (Bitnami chart).

## Usage

[Helm](https://helm.sh) must be installed to use the charts.  Please refer to
Helm's [documentation](https://helm.sh/docs) to get started.

Once Helm has been set up correctly, add the repo as follows:

```
helm repo add <alias> https://jasperjuergensen.github.io/MediaManager-helm/
```

If you had already added this repo earlier, run `helm repo update` to retrieve
the latest versions of the packages.  You can then run `helm search repo <alias>` to see the charts.

To install the MediaManager chart:

```
helm install my-<chart-name> <alias>/MediaManager
```

To uninstall the chart:

```
helm uninstall my-<chart-name>
```

## Configuration

The configurable values are in `MediaManager/values.yaml`. Below are important options and typical uses.

Image
- `image.repository` (string): Container image repository. Default: `ghcr.io/maxdorninger/mediamanager/mediamanager`.
- `image.tag` (string): Image tag. If empty, the chart's `appVersion` is used.
- `image.pullPolicy` (string): Image pull policy. Default: `IfNotPresent`.

Service
- `service.type` (string): Kubernetes Service type (`ClusterIP`, `NodePort`, `LoadBalancer`). Default: `ClusterIP`.
- `service.port` (int): Service port exposed. Default: `8000`.

Ingress
- `ingress.enabled` (bool): Create an Ingress resource. Default: `false`.
- `ingress.hosts` (list): Hostnames and paths to route to the Service. Default includes an example `chart-example.local` host.
- `ingress.tls` (list): TLS configuration entries — set `secretName` and `hosts` to enable TLS.

Persistence
- `persistence.images.enabled` (bool): Enable a PersistentVolumeClaim for storing image files. Default: `false`.
- `persistence.images.size` (string): PVC size (e.g., `10Gi`). Default: `10Gi`.
- `persistence.images.storageClass` (string): StorageClass for the PVC. Default: empty (cluster default).
- `persistence.images.accessMode` (string): PVC access mode. Default: `ReadWriteOnce`.

Application configuration and secrets
- `mediamanager.port` (int): Application port within the container. Default: `8000`.
- `mediamanager.config` (map): Configuration for MediaManager. The chart renders this into a `config.toml` and mounts it at `/app/config`. Refer to the upstream MediaManager configuration docs for available options: https://maxdorninger.github.io/MediaManager/configuration.html
- `mediamanager.env` (list): Additional environment variables for the container.
- `mediamanager.tokenSecret` (string): Token secret for MediaManager authentication.

PostgreSQL dependency
- The chart includes an optional dependency on the Bitnami PostgreSQL chart. Enable it by setting `postgresql.enabled: true` and configure the nested values as needed. See the Bitnami PostgreSQL chart documentation for details.
