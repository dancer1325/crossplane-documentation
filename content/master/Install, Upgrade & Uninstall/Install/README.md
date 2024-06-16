* Outcome
  * create `Crossplane` pod
  * enable installation of Crossplane Provider resources

## Prerequisites
* [supported Kubernetes version](https://kubernetes.io/releases/patch-releases/#support-period)
* [Helm](https://helm.sh/docs/intro/install/) `v3.2.0` +

## How to install?
* `helm repo add crossplane-stable https://charts.crossplane.io/stable`
* `helm repo update`
* `helm install crossplane --namespace crossplane-system --create-namespace crossplane-stable/crossplane`
  * `--version DesiredCrossPlaneVersionToInstall`
    * optional, if you want to install a concrete version
  * [customizations](https://docs.crossplane.io/v1.16/software/install/#installation-options)
    * TODO:
  * `kubectl get pods -n crossplane-system`
    * check the pods installed
    * 'crossplane' pod manages 2 Kubernetes controllers
      * Package Manager
        * installs provider, function & configuration packages
      * Composition
        * installs and manages Composite CRD, Compositions and Claims
  * `kubectl get deployments -n crossplane-system`
    * check the deployments installed
      * 'crossplane'
        * 'crossplane-init container' installs [CRDs](https://github.com/crossplane/crossplane/tree/master/cluster/crds)
      * 'crossplane-rbac-manager'
        * manages Kubernetes ClusterRoles

* TODO:

## Distributions
* [Upbound](https://www.upbound.io/product/universal-crossplane)