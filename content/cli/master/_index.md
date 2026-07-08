---
weight: 50
title: CLI Overview
description: "Command-line tools for Crossplane development"
cascade:
    version: "master"
---

* Crossplane CLI
  * allows
    * simplifying, about Crossplane,
      * development
        * , about Crossplane Packages, to
          * build
          * install
          * update
          * push
        * building -- , by using Crossplane Projects, -- platforms
        * troubleshooting
          * Crossplane Compositions
          * Composite Resources
          * Managed Resources
      * administration
        * testing & rendering Composition Function / ❌WITHOUT need a Kubernetes Cluster❌
  * == 1! binary /
    * ❌NO has EXTERNAL dependencies❌

## Installing the CLI

* ways
  * -- via -- script
    * `curl -sL "https://raw.githubusercontent.com/crossplane/crossplane/main/install.sh" | sh`
      * by default,
        * FROM stable channel & current version
      * if you want to specify it -> `curl -sL "https://raw.githubusercontent.com/crossplane/crossplane/main/install.sh" | XP_VERSION=<v.X.Y.Z> && XP_CHANNEL=<masterORstable> sh`
    * `sudo mv crossplane /usr/local/bin`
  * MANUALLY
    * [download the binary](https://releases.crossplane.io/stable/current/bin)
      * choose your OS & crank
    * add the location of the "bin" | your $PATH
