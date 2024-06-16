* Check 'crossPlaneAsKubernetesController.png'

## Crossplane components
* Provider
  * allows
    * creating new (!= Crossplane ones) CRD / external service 👁️with an API (-> NOT necessarily Cloud-just)👁️
      * cluster-scoped
      * 👁available to ALL cluster-namespaces!! 👁
  * [marketPlace](https://marketplace.upbound.io/providers) & [contribRepository](https://github.com/crossplane-contrib/)
  * `kubectl get providers`
    * check the installed ones
  * _Examples:_ [AWS](https://marketplace.upbound.io/providers/upbound/provider-family-aws/v1.7.0), [Azure](https://marketplace.upbound.io/providers/upbound/provider-family-azure/v1.3.0), [GCP](https://marketplace.upbound.io/providers/upbound/provider-family-gcp/v1.3.0)
* ProviderConfig - `PC` -
  * allows
    * applying settings (authentication, global defaults, ..) / Provider
      * cluster-scoped
      * 👁available to ALL cluster-namespaces!! 👁
      * -> 
        * API endpoints unique / provider
  * `kubectl get providerconfig`
    * check the installed ones
* Managed Resource - `MR` -
  * := Provider resource / managed & created by Crossplane, living in Kubernetes cluster
    * Crossplane controller (as controller pattern == [kube-controller-manager](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-controller-manager/)) enforces settings & existence of `MR`
    * cluster-scoped
    * 👁available to ALL cluster-namespaces!! 👁
  * `kubectl get managed`
    * check ALL managed resources
    * ⚠️depend on API server -> it may timeout -- [issue](https://github.com/kubernetes/kubernetes/issues/111880) -- ⚠️
* Composition
  * := template / create multiple `MR` at once
    * template == 👁 NO resource is created 👁
    * _Example:_ Create storage resource + compute resource + VN resource -- via -- 1! Composition resource
  * `kubectl get compositions`
    * check ALL compositions
* Composite Resources - `XR` -
  * allows
    * creating multiple `MR` as 1! Kubernetes object -- via -- Composition
      * cluster-scoped -> requires cluster-wide permissions
      * 👁available to ALL cluster-namespaces!! 👁
  * `kubectl get composite`
    * check ALL Composite Resources
* CompositeResourceDefinitions - `XRD` -
  * allows
    * defining the schema for `XR` & `XC`
  * _Example:_ Check 'examples.yaml'
* Claims - `XC` -
  * == `XR` at namespace scope
    * -> main way / developers -- interact with -- Crossplane
  * `kubectl get claim`
    * check ALL claims
  * _Example:_ Check 'examples.yaml'
