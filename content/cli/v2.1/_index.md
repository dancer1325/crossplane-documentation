---
weight: 50
title: CLI Overview
description: "Command-line tools for Crossplane development"
cascade:
    version: "2.1"
---

* Crossplane CLI
  * allows
    * simplifying about Crossplane
      * development &
      * administration
  * == Tools + Composition Function testing & rendering + Troubleshooting
    * tools, about Crossplane Packages, to
      * build
      * install
      * update
      * push
    * Composition Function testing & rendering
      * standalone
        * == ❌NO need Kubernetes Cluster❌
    * troubleshooting
      * Crossplane Compositions
      * Composite Resources
      * Managed Resources
  * == 1! binary / 
    * ❌NO has EXTERNAL dependencies❌

## how to install the CLI

* ways
  * -- via -- script
    * `curl -sL "https://raw.githubusercontent.com/crossplane/crossplane/main/install.sh" | sh`
      * by default,
        * FROM stable channel & current version
      * if you want to specify it -> `curl -sL "https://raw.githubusercontent.com/crossplane/crossplane/main/install.sh" | XP_VERSION=<v.X.Y.Z> && XP_CHANNEL=<masterORstable> sh`
  * MANUALLY
    * [download the binary](https://releases.crossplane.io/stable/current/bin)
      * choose your OS & crank
    * add the location of the "bin" | your $PATH

* `crossplane --version`
  * check it has been installed and globally added the environment variable
