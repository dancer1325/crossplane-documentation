---
title: Install Crossplane
weight: 100
description: "Install Crossplane in a Kubernetes cluster"
---

* goal
  * install Crossplane | Kubernetes cluster

## Install Crossplane -- via -- Helm chart

* [source code](https://github.com/dancer1325/crossplane/tree/main/cluster/charts/crossplane)

TODO: decide where to place? here or other repo

* steps
  * `helm repo add crossplane-stable https://charts.crossplane.io/stable`
    * if you want to update the local Helm chart cache -> `helm repo update`
  * `helm install crossplane --namespace crossplane-system --create-namespace crossplane-stable/crossplane`
    * if you want to check the changes PREVIOUS to install -> `helm install crossplane --namespace crossplane-system --create-namespace crossplane-stable/crossplane --dry-run --debug`
    * `kubectl get pods -n crossplane-system`
      * check the installed Crossplane pods
    * `kubectl get crd`
      * check installed CRD

* enable
  * create MANY DIFFERENT CR

## Installation options

<!-- vale Google.Headings = NO -->
<!-- vale Microsoft.Headings = NO -->
### Customize the Crossplane Helm chart
<!-- vale Google.Headings = YES -->
<!-- vale Microsoft.Headings = YES -->

Crossplane supports customizations at install time by configuring the Helm
chart.

Read [the Helm chart README](https://github.com/crossplane/crossplane/blob/main/cluster/charts/crossplane/README.md#configuration)
to learn what customizations are available.

Read [the Helm documentation](https://helm.sh/docs/) to learn how to run Helm
with custom options using `--set` or `values.yaml`.

#### Feature flags

Crossplane introduces new features behind feature flags. By default alpha
features are off. Crossplane enables beta features by default. To enable a
feature flag, set the `args` value in the Helm chart. Available feature flags
can be directly found by running `crossplane core start --help`, or by looking
at the table below.

{{< expand "Feature flags" >}}
{{< table caption="Feature flags" >}}
| Status | Flag                                    | Description                                                               |
|--------|-----------------------------------------|---------------------------------------------------------------------------|
| Beta   | `--enable-deployment-runtime-configs`   | Enable support for DeploymentRuntimeConfigs.                              |
| Beta   | `--enable-usages`                       | Enable support for Usages.                                                |
| Beta   | `--enable-realtime-compositions`        | Enable support for real time compositions.                                |
| Alpha  | `--enable-dependency-version-upgrades ` | Enable automatic version upgrades of dependencies when updating packages. |
| Alpha  | `--enable-function-response-cache`      | Enable caching of composition function responses to improve performance.  |
| Alpha  | `--enable-provider-deletion-protection` | Enable automatic protection of Providers from deletion when they have active managed resources. Requires `--enable-usages`. |
| Alpha  | `--enable-signature-verification`       | Enable support for package signature verification via ImageConfig API.    |
{{< /table >}}
{{< /expand >}}

Set these flags either in the `values.yaml` file or at install time using the
`--set` flag, for example: `--set
args='{"--enable-composition-functions","--enable-composition-webhook-schema-validation"}'`.

## Install pre-release Crossplane versions

Install pre-release versions of Crossplane from the `master` Crossplane Helm channel.

Versions in the `master` channel are under active development and may be unstable.

{{< hint "warning" >}}
Don't use Crossplane `master` releases in production. Only use `stable` channel.
Only use `master` for testing and development.
{{< /hint >}}

### Add the Crossplane master Helm repository

Add the Crossplane repository with the `helm repo add` command.

```shell
helm repo add crossplane-master https://charts.crossplane.io/master/
```

Update the
local Helm chart cache with `helm repo update`.
```shell
helm repo update
```

### Install the Crossplane master Helm chart

Install the Crossplane Helm chart from the `master` channel with `helm install`. Use the
`--devel` flag to install the latest pre-release version.

```shell
helm install crossplane \
--namespace crossplane-system \
--create-namespace crossplane-master/crossplane \
--devel
```

## Build and install from source

Building Crossplane from the source code gives you complete control over the build and
installation process. Full instructions for this advanced installation path is in the
[install from source code guide]({{<ref "../guides/install-from-source.md">}}).