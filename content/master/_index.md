---
title: "Welcome"
weight: -1
description: "Control plane framework for building cloud native platforms"
cascade:
    version: "master"
---

* Crossplane
  * history
    * ORIGINALLY,  Upbound
    * RIGHT NOW, CNCF project
  * == Kubernetes extension / 
    * open source 
    * 💡your Kubernetes cluster -- is transformed into a -- **universal control plane** 💡
      * == Kubernetes Control plane -- is -- extended
      * == -- works as -- Kubernetes Controller
  * TODO: == control plane framework -- for -- platform engineering 
  * 👀== extensible BE + configurable FE 👀
    * BE
      * allows
        * orchestrating
          * applications &
            * INDEPENDENTLY | they run
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
    * TODO: policies & permissions & other guardrails / NOT need to be an infra expert
  * use cases
    * create MULTIPLE resources | MULTIPLE clouds /
      * -- via -- 1! API call
      * 💡-> Kubernetes resources & cloud resources | 1! control plane💡 
      * -- provided by -- vendors
    * create NEW abstractions & custom APIs / -- powered by -- Kubernetes resources
      * == create & manage resources / ⚠️EXTERNAL to the cluster ⚠️

* Control plane
  * create & manage the resources' lifecycle
  * CONSTANTLY check that the intended resources exist
  * if the intended state != reality state -> report
  * act / make things right

* see 
  * [websiteIndex](https://github.com/dancer1325/crossplane-website/blob/main/pages/index.md)
  * [websiteWhyControlPlane](https://github.com/dancer1325/crossplane-website/blob/main/pages/why-control-planes.md)
  * [Order pizza via Crossplane](https://github.com/dancer1325/crossplane-website/blob/main/blog/docs/providers-101-ordering-pizza-with-kubernetes-and-crossplane.md)
