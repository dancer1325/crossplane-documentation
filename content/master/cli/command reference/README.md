* global flags
  * == available for ALL commands
  * `-h` / `--help`
  * `--verbose`
    * print verbose output
* `crossplane version`
  * returns Crossplane
    * CLI version
    * control plane
* `crossplane xpkg COMMAND`
  * `crossplane xpkg build`
    * allows
      * building 
        * Crossplane packages -- as -- [OCI container image](https://opencontainers.org)
          * follows [Crossplane XPKG specification](https://github.com/crossplane/crossplane/blob/master/contributing/specifications/xpkg.md)
        * composition functions
        * provider package types
    * TODO:
  * `crossplane xpkg install`
  * `crossplane xpkg login`
  * `crossplane xpkg logout`
  * `crossplane xpkg push Package`
  * `crossplane xpkg update`
* TODO: