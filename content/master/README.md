* := CN Control Plane Framework
  * allows
    * Kubernetes cluster == universal control plane for any resource
    * building [Control Planes](https://kubernetes.io/docs/reference/glossary/?all=true#term-control-plane) without coding
    * application & infrastructure configuration coexist in the same control plane
  * == extensible BE + configurable FE
    * BE
      * allows
        * orchestrating
          * applications &
          * infrastructure
      * extensible — via — [providers](https://marketplace.upbound.io/)
    * FE
      * allows
        * defining declarative API
      * configurable to expose new
  * characteristics
    * open source
    * built on top of Kubernetes Control Plane
      * -> works as Kubernetes Controller
    * new CRDs
      * == new abstractions & custom APIs / powered by Kubernetes' policies, namespaces, RBAC ...
    * approach
      * policies & permissions & other guardrails / NOT need to be an infra expert 
      * == done by vendors
* Originally by Upbound

## Notes
* Some content comes from
  * [websiteIndex](https://github.com/crossplane/website/blob/main/pages/index.tsx)
  * [websiteWhyControlPlane](https://www.crossplane.io/why-control-planes)
* [Order pizza via Crossplane](https://blog.crossplane.io/providers-101-ordering-pizza-with-kubernetes-and-crossplane/)