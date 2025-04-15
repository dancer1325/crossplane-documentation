---
title: Crossplane Introduction
weight: 2
---

* Crossplane
  * allows
    * 👀connecting your Kubernetes cluster -- to -- EXTERNAL NON-Kubernetes resources 👀
      * == users ONLY -- communicate with -- Kubernetes
        * _Example:_ 
          * you -- communicate with -- Kubernetes & 
          * Crossplane -- communicate with -- external resources (AWS, Azure, Google Cloud, ...)
      * EXTERNAL NON-Kubernetes resources
        * ⚠️== ANY service / has API ⚠️
    * building CUSTOM Kubernetes APIs / consume THOSE resources
  * 💡creates Kubernetes [CRDs](https://kubernetes.io/docs/tasks/extend-kubernetes/custom-resources/custom-resource-definitions/) 💡 
    * == EXTERNAL resources -- are represented as -- NATIVE [Kubernetes objects](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/) 
      * -> AVAILABLE FULL [Kubernetes API](https://kubernetes.io/docs/reference/using-api/)
  * 💡acts -- as a -- [Kubernetes Controller](https://kubernetes.io/docs/concepts/architecture/controller/) 💡
    * == 
      * watch the EXTERNAL resources' state 
      * enforce the state
        * _Example:_ if something OUTSIDE of Kubernetes modifies or deletes a resource -> Crossplane
          * reverses the change or
          * recreates the deleted resource

      ![](/content/media/crossplane-intro-diagram.png)

## Crossplane components overview

| Component | Abbreviation | Scope | Summary |
| --- | --- | --- | ---- | 
| [Provider](#providers) | | cluster | Creates NEW Kubernetes CRDs / EXTERNAL service |
| [ProviderConfig](#provider-configurations) | `PC` | cluster | Applies settings / Provider |
| [Managed Resource](#managed-resources) | `MR` | cluster | A Provider resource created and managed by Crossplane inside the Kubernetes cluster. | 
| [Composition](compositions) |  | cluster | A template for creating multiple _managed resources_ at once. |
| [Composite Resources](#composite-resources) | `XR` | cluster | Uses a _Composition_ template to create multiple _managed resources_ as a single Kubernetes object. |
| [CompositeResourceDefinitions](#composite-resource-definitions) | `XRD` | cluster | Defines the API schema for _Composite Resources_ and _Claims_ |
| [Claims](#claims) | `XC` | namespace | Like a _Composite Resource_, but namespace scoped. | 

## The Crossplane Pod

* | install Crossplane | Kubernetes cluster,
  * core Crossplane components' set of `CRDs`

    ```shell
    ❯ kubectl get crd
    NAME                                                    
    compositeresourcedefinitions.apiextensions.crossplane.io
    compositionrevisions.apiextensions.crossplane.io        
    compositions.apiextensions.crossplane.io                
    configurationrevisions.pkg.crossplane.io                
    configurations.pkg.crossplane.io                        
    controllerconfigs.pkg.crossplane.io                     
    deploymentruntimeconfigs.pkg.crossplane.io              
    environmentconfigs.apiextensions.crossplane.io          
    functionrevisions.pkg.crossplane.io                     
    functions.pkg.crossplane.io                             
    locks.pkg.crossplane.io                                 
    providerrevisions.pkg.crossplane.io                     
    providers.pkg.crossplane.io                             
    storeconfigs.secrets.crossplane.io                      
    usages.apiextensions.crossplane.io                                        
    ```

## Providers

* allows
  * creating NEW (!= Crossplane ones) CRD / non-Kubernetes service 
    * == how Crossplane -- connects to a -- non-Kubernetes service
    * non-Kubernetes service's requirements
      * API
    * cluster-scoped & 👀AVAILABLE | ALL cluster-namespaces 👀
* [marketPlace](https://marketplace.upbound.io/providers) & [contribRepository](https://github.com/crossplane-contrib/)
* `kubectl get providers`
  * check the installed ones
* _Examples:_ [AWS](https://marketplace.upbound.io/providers/upbound/provider-family-aws/v1.7.0), [Azure](https://marketplace.upbound.io/providers/upbound/provider-family-azure/v1.3.0), [GCP](https://marketplace.upbound.io/providers/upbound/provider-family-gcp/v1.3.0)
* `kubectl get providers`

## Provider configurations

* allows
  * applying settings (authentication, global defaults, ..) / Provider
    * cluster-scoped
    * 👁available to ALL cluster-namespaces!! 👁
    * -> 
      * API endpoints unique / provider
* `kubectl get providerconfig`
  * check the installed ones

* TODO:
_ProviderConfigs_ configure settings
related to the Provider like authentication or global defaults for the
Provider.

The API endpoints for ProviderConfigs are unique to each Provider.

_ProviderConfigs_ are cluster scoped and available to all cluster namespaces.

View all installed ProviderConfigs with the command `kubectl get providerconfig`.

## Managed resources

* := Provider resource / managed & created by Crossplane, living in Kubernetes cluster
  * Crossplane controller (as controller pattern == [kube-controller-manager](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-controller-manager/)) enforces settings & existence of `MR`
  * cluster-scoped
  * 👁available to ALL cluster-namespaces!! 👁
* `kubectl get managed`
  * check ALL managed resources
  * ⚠️depend on API server -> it may timeout -- [issue](https://github.com/kubernetes/kubernetes/issues/111880) -- ⚠️
A Provider's CRDs map to individual _resources_ inside the provider. When
Crossplane creates and monitors a resource it's a _Managed Resource_.

Using a Provider's CRD creates a unique _Managed Resource_. For example,
using the Provider AWS's `bucket` CRD, Crossplane creates a `bucket` _Managed Resource_
inside the Kubernetes cluster that's connected to an AWS S3 storage bucket.

The Crossplane controller provides state enforcement for _Managed Resources_.
Crossplane enforces the settings and existence of _Managed Resources_. This
"Controller Pattern" is like how the Kubernetes 
[kube-controller-manager](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-controller-manager/)
enforces state for pods.

_Managed Resources_ are cluster scoped and available to all cluster namespaces.

Use `kubectl get managed` to view all _managed resources_.
{{<hint "warning" >}}
The `kubectl get managed` creates a lot of Kubernetes API queries.
Both the `kubectl` client and kube-apiserver throttle the API queries. 

Depending on the size of the API server and number of managed resources, this
command may take minutes to return or may timeout. 

For more information, read 
[Kubernetes issue #111880](https://github.com/kubernetes/kubernetes/issues/111880)
and 
[Crossplane issue #3459](https://github.com/crossplane/crossplane/issues/3459).
{{< /hint >}}

## Compositions

* := template / create multiple `MR` at once
  * template == 👁 NO resource is created 👁
  * _Example:_ Create storage resource + compute resource + VN resource -- via -- 1! Composition resource
* `kubectl get compositions`
  * check ALL compositions
A _Composition_ is a template for a collection of _managed resource_. _Compositions_ 
allow platform teams to define a set of _managed resources_ as a 
single object.

For example, a compute _managed resource_ may require the creation of a storage 
resource and a virtual network as well. A single _Composition_ can define all three
resources in a single _Composition_ object. 

Using _Compositions_ simplifies the deployment of infrastructure made up of
multiple _managed resources_. _Compositions_ also enforce standards and settings
across deployments.

Platform teams can define fixed or default settings for each _managed resource_ inside a
_Composition_ or define fields and settings that users may change.

Using the previous example, the platform team may set a compute resource size
and virtual network settings. But the platform team allows users to define the 
storage resource size.

Creating a _Composition_ Crossplane doesn't create any managed
resources. The _Composition_ is only a template for a collection of _managed
resources_ and their settings. A _Composite Resource_ creates the specific resources.

{{< hint "note" >}}
The [_Composite Resources_]({{<ref "#composite-resources">}}) section discusses
_Composite Resources_.
{{< /hint >}}

_Compositions_ are cluster scoped and available to all cluster namespaces.

Use `kubectl get compositions` to view all _compositions_.

## Composite Resources

* allows
  * creating multiple `MR` as 1! Kubernetes object -- via -- Composition
    * cluster-scoped -> requires cluster-wide permissions
    * 👁available to ALL cluster-namespaces!! 👁
* `kubectl get composite`
  * check ALL Composite Resources
A _Composite Resource_ (`XR`) is a set of provisioned _managed resources_. A
_Composite Resource_ uses the template defined by a _Composition_ and applies
any user defined settings. 

Multiple unique _Composite Resource_ objects can use the same _Composition_. For
example, a _Composition_ template can create a compute, storage and networking
set of _managed resources_. Crossplane uses the same _Composition_ template
every time a user requests this set of resources.

If a _Composition_ allows a user to define resource settings, users apply them
in a _Composite Resource_.


<!-- A _Composition_ defines which _Composite Resources_ can use the _Composition_
template with the _Composition_ `spec.compositeTypeRef` value. This defines the
{{<hover label="comp" line="7">}}apiVersion{{< /hover >}} and {{<hover
label="comp" line="8">}}kind{{< /hover >}} of _Composite Resources_ that can use the
_Composition_.

For example, in the _Composition_:
```yaml {label="comp"}
apiVersion: apiextensions.crossplane.io/v1
kind: Composition
metadata:
  name: test.example.org
spec:
  compositeTypeRef:
    apiVersion: test.example.org/v1alpha1
    kind: myComputeResource
    # Removed for brevity
```

A _Composite Resource_ that can use this template must match this 
{{<hover label="comp" line="7">}}apiVersion{{< /hover >}} and {{<hover
label="comp" line="8">}}kind{{< /hover >}}.

```yaml {label="xr"}
apiVersion: test.example.org/v1alpha1
kind: myComputeResource
metadata:
  name: myResource
spec:
  storage: "large"
```

The _Composite Resource_ {{<hover label="xr" line="1">}}apiVersion{{< /hover >}}
matches the and _Composition_ 
{{<hover label="comp" line="7">}}apiVersion{{</hover >}} and the 
_Composite Resource_  {{<hover label="xr" line="2">}}kind{{< /hover >}}
matches the _Composition_ {{<hover label="comp" line="8">}}kind{{< /hover >}}.

In this example, the _Composite Resource_ also sets the 
{{<hover label="xr" line="7">}}storage{{< /hover >}} setting. The
_Composition_ uses this value when creating the associated _managed resources_
owned by this _Composite Resource_. -->

{{< hint "tip" >}}
_Compositions_ are templates for a set of _managed resources_.  
_Composite Resources_ fill out the template and create _managed resources_.

Deleting a _Composite Resource_ deletes all the _managed resources_ it created.
{{< /hint >}}

_Composite Resources_ are cluster scoped and available to all cluster namespaces.

Use `kubectl get composite` to view all _Composite Resources_.

## Composite Resource Definitions

* allows
  * defining the schema for `XR` & `XC`
* _Example:_ Check 'examples.yaml'
_Composite Resource Definitions_ (`XRDs`) create custom Kubernetes APIs used by 
_Claims_ and _Composite Resources_.

{{< hint "note" >}}
The [_Claims_]({{<ref "#claims">}}) section discusses
_Claims_.
{{< /hint >}}

Platform teams define the custom APIs.  
These APIs can define specific values
like storage space in gigabytes, generic settings like `small` or `large`,
deployment options like `cloud` or `onprem`. Crossplane doesn't limit the API definitions.

The _Composite Resource Definition's_ `kind` is from Crossplane.
```yaml
apiVersion: apiextensions.crossplane.io/v1
kind: CompositeResourceDefinition
```

The `spec` of a _Composite Resource Definition_ creates the  `apiVersion`,
`kind` and `spec` of a _Composite Resource_. 

{{< hint "tip" >}}
The _Composite Resource Definition_ defines the parameters for a _Composite
Resource_.
{{< /hint >}}

A _Composite Resource Definition_ has four main `spec` parameters:
* A {{<hover label="specGroup" line="3" >}}group{{< /hover >}} 
to define the 
{{< hover label="xr2" line="2" >}}apiVersion{{</hover >}} 
in a _Composite Resource_ .
* The {{< hover label="specGroup" line="7" >}}versions.name{{</hover >}} 
that defines the version used in a _Composite Resource_.
* A {{< hover label="specGroup" line="5" >}}names.kind{{</hover >}}
to define the _Composite Resource_ 
{{< hover label="xr2" line="3" >}}kind{{</hover>}}.
* A {{< hover label="specGroup" line="8" >}}versions.schema{{</hover>}} section
to define the _Composite Resource_ {{<hover label="xr2" line="6" >}}spec{{</hover >}}.

```yaml {label="specGroup"}
# Composite Resource Definition (XRD)
spec:
  group: test.example.org
  names:
    kind: myComputeResource
  versions:
  - name: v1alpha1
    schema:
      # Removed for brevity
```

A _Composite Resource_ based on this _Composite Resource Definition_ looks like this:

```yaml {label="xr2"}
# Composite Resource (XR)
apiVersion: test.example.org/v1alpha1
kind: myComputeResource
metadata:
  name: myResource
spec:
  storage: "large"
```

A _Composite Resource Definition_ {{< hover label="specGroup" line="8" >}}schema{{</hover >}} defines the _Composite Resource_
{{<hover label="xr2" line="6" >}}spec{{</hover >}} parameters.

These parameters are the new, custom APIs, that developers can use. 

For example, creating a compute _managed resource_ requires knowledge of a
cloud provider's compute class names like AWS's `m6in.large` or GCP's
`e2-standard-2`. 

A _Composite Resource Definition_ can limit the choices to `small` or `large`.
A _Composite Resource_ uses those options and the _Composition_ maps them
to specific cloud provider settings. 

The following _Composite Resource Definition_ defines a {{<hover label="specVersions" line="17" >}}storage{{< /hover >}}
parameter. The storage is a 
{{<hover label="specVersions" line="18">}}string{{< /hover >}} 
and the OpenAPI 
{{<hover label="specVersions" line="19" >}}oneOf{{< /hover >}} requires the
options to be either {{<hover label="specVersions" line="20" >}}small{{< /hover >}} 
or {{<hover label="specVersions" line="21" >}}large{{< /hover >}}.

```yaml {label="specVersions"}
# Composite Resource Definition (XRD)
spec:
  group: test.example.org
  names:
    kind: myComputeResource
  versions:
  - name: v1alpha1
    served: true
    referenceable: true
    schema:
      openAPIV3Schema:
        type: object
        properties:
          spec:
            type: object
            properties:
              storage:
                type: string
                oneOf:
                  - pattern: '^small$'
                  - pattern: '^large$'
            required:
            - storage  
```

A _Composite Resource Definition_ can define a wide variety of settings and options. 

Creating a _Composite Resource Definition_ enables the creation of _Composite
Resources_ but can also create a _Claim_.

_Composite Resource Definitions_ with a `spec.claimNames` allow developers to
create _Claims_.

For example, the 
{{< hover label="xrdClaim" line="6" >}}claimNames.kind{{</hover >}}
allows the creation of _Claims_ of `kind: computeClaim`.
```yaml {label="xrdClaim"}
# Composite Resource Definition (XRD)
spec:
  group: test.example.org
  names:
    kind: myComputeResource
  claimNames:
    kind: computeClaim
  # Removed for brevity 
```

## Claims

* == `XR` at namespace scope
  * -> main way / developers -- interact with -- Crossplane
* `kubectl get claim`
  * check ALL claims
* _Example:_ Check 'examples.yaml'
_Claims_ are the primary way developers interact with Crossplane. 

_Claims_ access the custom APIs defined by the platform team in a _Composite
Resource Definition_.

_Claims_ look like _Composite Resources_, but they're namespace scoped,
while _Composite Resources_ are cluster scoped. 

{{< hint "note" >}}
**Why does namespace scope matter?**  
Having namespace scoped _Claims_ allows multiple teams, using unique namespaces,
to create the same types of resources, independent of each other. The compute
resources of team A are unique to the compute resources of team B.

Directly creating _Composite Resources_ requires cluster-wide permissions,
shared with all teams.   
_Claims_ create the same set of resources, but on a namespace level.
{{< /hint >}}

The previous _Composite Resource Definition_ allows the creation of _Claims_
of the kind  
{{<hover label="xrdClaim2" line="7" >}}computeClaim{{</hover>}}.  

Claims use the same 
{{< hover label="xrdClaim2" line="3" >}}apiVersion{{< /hover >}}
defined in _Composite Resource Definition_ and also used by 
_Composite Resources_.
```yaml {label="xrdClaim2"}
# Composite Resource Definition (XRD)
spec:
  group: test.example.org
  names:
    kind: myComputeResource
  claimNames:
    kind: computeClaim
  # Removed for brevity 
```

In an example _Claim_ the 
{{<hover label="claim" line="2">}}apiVersion{{< /hover >}}
matches the {{<hover label="xrdClaim2" line="3">}}group{{< /hover >}} in the
_Composite Resource Definition_. 

The _Claim_ {{<hover label="claim" line="3">}}kind{{< /hover >}} matches the
_Composite Resource Definition_ 
{{<hover label="xrdClaim2" line="7">}}claimNames.kind{{< /hover >}}.

```yaml {label="claim"}
# Claim
apiVersion: test.example.org/v1alpha1
kind: computeClaim
metadata:
  name: myClaim
  namespace: devGroup
spec:
  size: "large"
```

A _Claim_ can install in a {{<hover label="claim" line="6">}}namespace{{</hover >}}.  
The _Composite Resource Definition_ defines the 
{{<hover label="claim" line="7">}}spec{{< /hover >}} options the same way it
does for a _Composite Resource_ 
{{<hover label="xr-claim" line="6">}}spec{{< /hover >}}.

{{< hint "tip" >}}
_Composite Resources_ and _Claims_ are similar.   
Only _Claims_ can be in
a {{<hover label="claim" line="6">}}namespace{{</hover >}}.  
Also the _Composite Resource's_ {{<hover label="xr-claim"
line="3">}}kind{{</hover >}} may be different than the _Claim's_
{{<hover label="claim" line="3">}}kind{{< /hover >}}.  
The _Composite Resource Definition_ defines the 
{{<hover label="xrdClaim2" line="7">}}kind{{</hover >}} values.
{{< /hint >}}

```yaml {label="xr-claim"}
# Composite Resource (XR)
apiVersion: test.example.org/v1alpha1
kind: myComputeResource
metadata:
  name: myResource
spec:
  storage: "large"
```

_Claims_ are namespace scoped.

View all available Claims with the command `kubectl get claim`.

## Next steps
Build your own Crossplane platform using one of the quickstart guides.
* [Azure Quickstart]({{<ref "provider-azure" >}})
* [AWS Quickstart]({{<ref "provider-aws" >}})
* [GCP Quickstart]({{<ref "provider-gcp" >}})
