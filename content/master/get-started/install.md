---
title: Install Crossplane
weight: 100
description: "Install Crossplane in a Kubernetes cluster"
---

* goal
  * install Crossplane , -- via Helm chart, | Kubernetes cluster,

* [source code](https://github.com/dancer1325/crossplane/tree/main/cluster/charts/crossplane)

* [steps](https://github.com/dancer1325/crossplane/tree/main/cluster/charts/crossplane/README.md)

* feature flags
  * 👀enable features👀 
  * status
    * alpha
      * by default,
        * off
    * beta,
      * by default,
        * on
    * To enable a
    feature flag, set the `args` value in the Helm chart
  * if you want to check the ALLOWED ones -> `crossplane core start --help`
  * [passed as `args` | Helm chart](https://github.com/dancer1325/crossplane/blob/main/cluster/charts/crossplane/templates/deployment.yaml#L145)

| Status | Flag                                    | Description                                                                                                                |
|--------|-----------------------------------------|----------------------------------------------------------------------------------------------------------------------------|
| Beta   | `--enable-deployment-runtime-configs`   | Enable support for DeploymentRuntimeConfigs                                                                                |
| Beta   | `--enable-usages`                       | Enable support for Usages                                                                                                  |
| Beta   | `--enable-realtime-compositions`        | Enable support for real time compositions                                                                                  |
| Alpha  | `--enable-dependency-version-upgrades ` | Enable automatic version upgrades of dependencies when updating packages.                                                  |
| Alpha  | `--enable-function-response-cache`      | Enable caching of composition function responses to improve performance.                                                   |
| Alpha  | `--enable-provider-deletion-protection` | Enable automatic protection of Providers from deletion when they have active managed resources. Requires `--enable-usages`. |
| Alpha  | `--enable-signature-verification`       | Enable support for package signature verification via ImageConfig API.                                                     |
