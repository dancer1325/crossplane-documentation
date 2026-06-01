---
title: "Welcome"
weight: -1
description: "Control plane framework for building cloud native platforms"
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
  * TODO: == control plane framework -- for -- platform engineering 
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
      * 💡-> Kubernetes resources & cloud resources | 1! control plane💡 
      * -- provided by -- vendors
    * create NEW abstractions & custom APIs / -- powered by -- Kubernetes resources
      * == create & manage resources / ⚠️EXTERNAL to the cluster ⚠️

* Control plane
  * create & manage the resources' lifecycle
  * CONSTANTLY check that the intended resources exist
  * if the intended state != reality state -> report
  * act / make things right

* TODO: Review
  * [websiteIndex](https://github.com/crossplane/website/blob/main/pages/index.tsx)
  * [websiteWhyControlPlane](https://www.crossplane.io/why-control-planes)
  * [Order pizza via Crossplane](https://blog.crossplane.io/providers-101-ordering-pizza-with-kubernetes-and-crossplane/)

* [What's Crossplane?]({{<ref "./whats-crossplane">}}) introduces Crossplane
  and explains why you should use it.

* [What's New in v2?]({{<ref "whats-new">}}) highlights what's changed in
  Crossplane v2.

* [Get Started]({{<ref "get-started">}}) explains how to install Crossplane and
  create a control plane.

* [Composition]({{<ref "composition">}}) covers the key concepts of composition.

* [Operations]({{<ref "operations">}}) covers the key concepts of operations.

* [Managed Resources]({{<ref "managed-resources">}}) covers the key concepts of
  managed resources.

* [Packages]({{<ref "packages">}}) covers the key concepts of the Crossplane
  package manager.

* [Guides]({{<ref "guides">}}) guide you through common use cases, like
  monitoring Crossplane or extending it by writing a composition function.

* [CLI Reference]({{<ref "cli">}}) documents the `crossplane` command-line
  interface that you can use to configure a Crossplane control plane.

* [API Reference]({{<ref "api">}}) documents the APIs that you can use to
  configure a Crossplane control plane.
