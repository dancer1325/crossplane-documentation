* allows
  * applying changes to resources / defined by Composition
  * if you create Claims, settings in the Claims -- are passed by Crossplane to -> associated `XR`
    * Patches use these settings to change -> associated `XR` OR `MR`
* common uses
  * change external resource name
  * generic terms (Example: "east" OR "west") -- are mapped to -> specific provider locations
  * resource fields -- are appended with -- custom labels OR strings
* best practices
  * patch and transform -- for -- simple changes
  * Composition Functions -- for -- complex OR programmatic modifications

## Patch
* := action of changing a field
* `Composition.resources[*].patches`
  * == patch = part of 1 Composition's resource
* `patch.type`
  * mandatory
  * if = "PatchSet" -> apply a PatchSet
  * available ones
    * "FromCompositeFieldPath"
    * "ToCompositeFieldPath"
    * "CombineFromComposite"
    * "CombineToComposite"
    * "FromEnvironmentFieldPath"
    * "ToEnvironmentFieldPath"
    * "CombineFromEnvironment"
    * "CombineToEnvironment"
* `patch.fromFieldPath` & `patch.toFieldPath`
  * mandatory
  * refers to
    * Composition's field OR
    * `XR`'s field
* "field selectors"
  * subset of [JsonPath selectors](https://kubernetes.io/docs/reference/kubectl/jsonpath/) /
    * allows
      * selecting fields in a 
        * `XR` OR
        * `MR`
* PatchSet
  * allows
    * Composition -- can reuse -- a Patch on multiple resources
  * `spec.patchSets`
    * way to create them (several can be defined)
    * `....name` & `...patches`
      * mandatories
      * NO other PatchSet can be contained
  * Crossplane ignores in a PatchSet any
    * transform or
    * policy
* restrictions
  * ⚠️NOT possible Composition1's resource1 -- is patched to -- Composition1's resource2 ⚠️
    * Solution: TODO:
  * ⚠️"ToEnvironmentFieldPath" can NOT read from `Status` ⚠️

## Transform
* == values are modified / 👀BEFORE 👀the patch
