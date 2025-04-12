---
title: Install Crossplane
weight: 100
---

* goal
  * install Crossplane
    * -> 
      * create `Crossplane` pod
      * enable the installation of Crossplane _Provider_ resources

## Prerequisites
* [supported Kubernetes version](https://kubernetes.io/releases/patch-releases/#support-period)
* [Helm](https://helm.sh/docs/intro/install/) `v3.2.0`+
### recommendations
* install [Kind](https://kind.sigs.k8s.io/)

## How to install?

* -- via -- Crossplane Helm chart

* `helm repo add crossplane-stable https://charts.crossplane.io/stable`
* `helm repo update`
* `helm install crossplane --namespace crossplane-system --create-namespace crossplane-stable/crossplane`
  * if you want to see the changes done in life -> run 
    ```
    helm install --dry-run --debug crossplane --namespace crossplane-system --create-namespace crossplane-stable/crossplane
    ```
  * `kubectl get pods -n crossplane-system`
    ```shell {copy-lines="1"}
    kubectl get pods -n crossplane-system
    NAME                                       READY   STATUS    RESTARTS   AGE
    crossplane-6d67f8cd9d-g2gjw                1/1     Running   0          26m
    crossplane-rbac-manager-86d9b5cf9f-2vc4s   1/1     Running   0          26m
    ```
  * `--version DesiredCrossPlaneVersionToInstall`
    * OPTIONAL
  * `kubectl get deployments -n crossplane-system` 
    ```shell {copy-lines="1"}
    kubectl get deployments -n crossplane-system
    NAME                      READY   UP-TO-DATE   AVAILABLE   AGE
    crossplane                1/1     1            1           8m13s
    crossplane-rbac-manager   1/1     1            1           8m13s
    ```

## Installed deployments

### Crossplane deployment -- "crossplane" --
* `crossplane-init container`
  * starts the deployments 
  * installs the Crossplane CRDs | Kubernetes cluster
  * | finish,
    * `crossplane` pod manages
      * _Package Manager controller_
        * installs
          * provider,
          * function
          * configuration packages
      * _Composition controller_
        * installs & manages
          * Crossplane _Composite Resource Definitions_,
          * Crossplane _Compositions_
          * Crossplane _Claims_

### Crossplane RBAC manager deployment -- "crossplane-rbac-manager"
* creates & manages Kubernetes _ClusterRoles_ / installed
  * Crossplane _Provider_ &
  * 's _Custom Resource Definitions_
* [Crossplane RBAC Manager's design document](https://github.com/crossplane/crossplane/blob/master/design/design-doc-rbac-manager.md) 

## How to customize the instalation?

### customize Crossplane Helm chart

* [here](https://github.com/crossplane/crossplane/blob/master/cluster/charts/crossplane/README.md)

#### -- via -- CL 

* `--set <setting>=<value>`
  * == `helm install crossplane --set <setting1>=<value1>,<setting2>=<value2>`
  * _Examples:_
    ```shell
    helm install crossplane \
    --namespace crossplane-system \
    --create-namespace \
    crossplane-stable/crossplane \
    --set image.pullPolicy=Always
    ```
    ```shell
    helm install crossplane \
    --namespace crossplane-system \
    --create-namespace \
    crossplane-stable/crossplane \
    --set image.pullPolicy=Always,replicas=2  # setting1=value1,setting2=value2
    ```

#### -- via -- Helm values file

* `-f <filename>`
  * == `helm install crossplane -f <filename>`
  * _Examples:_
    ```yaml
    replicas: 2
    
    image:
      pullPolicy: Always
    ```
    ```shell
    helm install crossplane \
    --namespace crossplane-system \
    --create-namespace \
    crossplane-stable/crossplane \
    -f settings.yaml
    ```

#### -- via -- Feature flags

* TODO:
Crossplane introduces new features behind feature flags. By default
alpha features are off. Crossplane enables beta features by default. To enable a 
feature flag, set the `args` value in the Helm chart. Available feature flags
can be directly found by running `crossplane core start --help`, or by looking 
at the table below.

{{< expand "Feature flags" >}}
{{< table caption="Feature flags" >}}
| Status | Flag | Description |
| --- | --- | --- |
| Beta | `--enable-composition-functions` | Enable support for Composition Functions. |
| Beta | `--enable-composition-functions-extra-resources` | Enable support for Composition Functions Extra Resources. Only respected with `--enable-composition-functions` enabled. |
| Beta | `--enable-composition-webhook-schema-validation` | Enable Composition validation using schemas. |
| Beta | `--enable-deployment-runtime-configs` | Enable support for DeploymentRuntimeConfigs. |
| Alpha | `--enable-environment-configs` | Enable support for EnvironmentConfigs. |
| Alpha | `--enable-external-secret-stores` | Enable support for External Secret Stores. |
| Alpha | `--enable-realtime-compositions` | Enable support for real time compositions. |
| Alpha | `--enable-ssa-claims` | Enable support for using server-side apply to sync claims with XRs. |
| Alpha | `--enable-usages` | Enable support for Usages. |
{{< /table >}}
{{< /expand >}}

Set these flags either in the `values.yaml` file or at install time using the
`--set` flag, for example: `--set
args='{"--enable-composition-functions","--enable-composition-webhook-schema-validation"}'`.

#### Change the default package registry

Beginning with Crossplane version 1.15.0 Crossplane downloads packages from the
[Upbound Marketplace](https://marketplace.upbound.io) at `xpkg.upbound.io` 
instead of DockerHub. 

Change the default registry location during the Crossplane install with 
`--set args='{"--registry=index.docker.io"}'`.

### Install pre-release Crossplane versions
Install a pre-release versions of Crossplane from the `master` Crossplane Helm channel.

Versions in the `master` channel are under active development and may be unstable.

{{< hint "warning" >}}
Don't use Crossplane `master` releases in production. Only use `stable` channel.  
Only use `master` for testing and development.
{{< /hint >}}


#### Add the Crossplane master Helm repository

Add the Crossplane repository with the `helm repo add` command.

```shell
helm repo add crossplane-master https://charts.crossplane.io/master/
```

Update the
local Helm chart cache with `helm repo update`.
```shell
helm repo update
```

#### Install the Crossplane master Helm chart

Install the Crossplane `master` Helm chart with `helm install`.

{{< hint "tip" >}}
View the changes Crossplane makes to your cluster with the 
`helm install --dry-run --debug` options. Helm shows what configurations it
applies without making changes to the Kubernetes cluster.
{{< /hint >}}

Crossplane creates and installs into the `crossplane-system` namespace.

```shell
helm install crossplane \
--namespace crossplane-system \
--create-namespace crossplane-master/crossplane \
--devel 
```

## Crossplane distributions 
 
* [Crossplane conformant](https://github.com/cncf/crossplane-conformance)

### Vendors

#### Upbound
 
* [Universal Crossplane -- `UXP` --](https://www.upbound.io/product/universal-crossplane)
  * distribution of Crossplane
    * free &
    * open source  
