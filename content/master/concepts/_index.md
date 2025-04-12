---
title: Concepts
weight: 50
description: Understand Crossplane's core components
---

* custom APIs
  * enable abstractions
    * Reason: 🧠
      * NOT need to know -- about -- underlying resources or requirements
        * _Example:_ instance type or region names
      * ONLY relevant details / -- exposed by the -- API
        * _Example:_ `big`, `small`, `US`, `EU` 🧠

* Crossplane's core components
  * [Crossplane pods](./pods)
    * 👀== core Crossplane pod + Crossplane RBAC manager pod👀
    * manage ALL Crossplane components & resources
  * [Providers](./providers)
    * allows
      * 👀connecting Kubernetes -- to -- ANY external provider (AWS, Azure or GCP) 👀
        * == Kubernetes native manifests & API calls -- are translated into -- external API calls 
    * responsible for, about lifecycle of their resources,
      * creating,
      * deleting
      * managing 
  * [Managed resources](./managed-resources)
    * == Kubernetes objects / 
      * 💡-- represent -- things / Provider created OUTSIDE Kubernetes💡
    * requirements
      * Provider -- to 
        * create a -- resource
        * delete the associated -- external resource
  * [Compositions](./compositions)
    * == template of managed resources
    * uses
      * describe MORE complex deployments
        * _Example:_ MULTIPLE managed resources + resource customizations (database, cloud provider region)
  * [Composite Resource Definitions](./composite-resource-definitions)
    * == custom API /
      * -- created by -- platform engineers 
      * -- consumed by -- developers OR end users
      * extend Kubernetes
        * == custom
          * API endpoints,
          * API revisions
          * ..
        * -- via -- OpenAPIv3 schema 
  * [Composite Resources](./composite-resources)
    * == ALL objects -- created by a -- user / call the custom API
      * ALL related managed resources -- are linked to -- it
  * [Claims](./claims)
    * == Composite Resources / 
      * BUT EXIST | Kubernetes namespace /
        * their resources -- are isolated from -- other teams' namespaces 
      * -- linked to -- 1! cluster scoped Composite Resource
      * -- created by -- platform users
  * [Composition Functions](./composition-functions)
    * == custom programs /
      * written | your desired programming language 
      * apply logic & loops | 
        * BEFORE Crossplane creates resources
        * AFTER Crossplane creates resources
  * [Patches and Transforms](./patch-and-transform)
    * enable  
      * platform engineers use user inputs | their custom API
      * how Crossplane creates resources
      * flexible & abstract inputs
        * _Example:_ | create the actual managed resources, `big` or `encrypted` to have specific meanings
  * [EnvironmentConfigs](./environment-configs)
    * | in-memory data store (Kubernetes ConfigMap)
    * uses
      * custom resource mapping
      * store & retrieve data -- across -- Claims & Composite Resources
  * [Usages](./usages) 
    * == define 
      * critical resources
      * custom dependency mappings 
    * uses
      * prevent Crossplane -- from -- deleting
      * ensure that a parent resource -- waits for -- Crossplane / delete ALL child resources first
  * [Packages](./packages)
    * allows
      * packaging up an ENTIRE custom platform
      * defining any other Crossplane
        * == define how to install 
          * Providers,
          * custom APIs or
          * composition functions
