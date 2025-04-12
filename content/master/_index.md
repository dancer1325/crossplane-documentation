---
title: "Overview"
weight: -1
cascade:
    version: "master"
---

* Crossplane
  * ORIGINALLY,  Upbound
  * == Kubernetes extension / 
    * open source 
    * 💡your Kubernetes cluster -- is transformed into a -- **universal control plane** 💡
      * == Kubernetes Control plane -- is -- extended
      * == -- works as -- Kubernetes Controller
  * 👀== extensible BE + configurable FE 👀
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
  * allows
    * manage ANYTHING (/ has an API) -- through -- standard Kubernetes APIs
    * building [Control Planes](https://kubernetes.io/docs/reference/glossary/?all=true#term-control-plane) without coding
    * application & infrastructure configuration EXIST | SAME control plane
  * use cases
    * create MULTIPLE resources | MULTIPLE clouds /
      * -- via -- 1! API call
      * Kubernetes == control plane for everything
      * -- provided by -- vendors
    * create NEW abstractions & custom APIs / -- powered by -- Kubernetes resources

* Control plane
  * create & manage the resources' lifecycle
  * CONSTANTLY check that the intended resources exist
  * if the intended state != reality state -> report
  * act / make things right

* TODO: Review
  * [websiteIndex](https://github.com/crossplane/website/blob/main/pages/index.tsx)
  * [websiteWhyControlPlane](https://www.crossplane.io/why-control-planes)
  * [Order pizza via Crossplane](https://blog.crossplane.io/providers-101-ordering-pizza-with-kubernetes-and-crossplane/)