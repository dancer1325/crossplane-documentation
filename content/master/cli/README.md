# CLI
* allows
  * simplifying about Crossplane
    * development & 
    * administration
* == Tools + Composition Function + Troubleshooting
  * tools to
    * build
    * install
    * update
    * push
  * Composition Function
    * for
      * testing
      * rendering
    * 👁️ WITHOUT need to have Kubernetes Cluster with Crossplane 👁️
  * troubleshooting for
    * Crossplane Compositions
    * `XR`
    * `MR`
* == 1! binary / NO external dependencies
## How to install?
* ways
  * `curl -sL "https://raw.githubusercontent.com/crossplane/crossplane/master/install.sh" | sh`
  * manually
    * [download the binary](https://releases.crossplane.io/stable/current/bin)
      * choose your OS & crank
    * add the location of the "bin" to your $PATH
  * other versions
    * TODO:
* `crossplane --version`
  * check it has been installed and globally added the environment variable