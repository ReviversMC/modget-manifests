In order for Modget not having to download all manifests when looking for a specific mod, we provide a lookup table with basic metadata like the id, various variations on the mod name and the different packages that exist.

Table of Contents
----------------------------------
YAML Specification
   1. YAML Syntax
   2. Lookup table mod examples
   3. Best Practices

# YAML Specification

## YAML Syntax
Each field in the file must be `camelCase`d and cannot be duplicated.

## Lookup table mod examples

```YAML
- id: sodium
  names:
    - sodium
  packages:
    - CaffeineMC.Sodium
    - IrisShaders.Sodium
    - Comp500.Sodium
- id: computercraft
  names:
    - cc-r
    - cc-tf
    - cc-t-f
    - cc-restitched
    - cc-tweakedfabric
    - cc-tweaked-fabric
    - computercraft
    - computercraft-restitched
      - computercraft-tweaked-fabric
  packages:
    - Merith-TK.Computercraft
    - Zundrel.Computercraft
```

### Optional Keys

```YAML
  tags:
    - adventure
    - armor
    - tools
    - weapons
    - cosmetic
    - magic
    - ...
```


## Best Practices
The package identifier (`Publisher.ModID`) must be unique (see the [manifest spec](https://github.com/ReviversMC/modget-manifests/tree/master/doc/manifest-spec-v1.md)). Only one pull request per package is allowed.

**Apart from packages, all values have to be lower case!**

Names should contain the actual mod name (listed in the mod's `fabric.mod.json`), different variations of that and, if applicable, a differing CurseForge/Modrinth url name. If one of the aforementioned versions consists of multiple words, it must be `kebab-case`d.

Packages should be sorted according to relevance. If multiple forks exist, the original mod (if still maintained, of course), should be listed at the top. If multiple packages have the same relevance, sort them alphabetically.

Avoid creating redundant tags. Tags should be very short and descriptive! We will add a list with accepted ones later on; for now please resort to the tags used above or CurseForge/Modrinth ones. Note that each tag has to be one-word only and, if necessary, has to be `kebab-case` formatted.

Provide as many fields as possible. The more metadata you provide the better the user experience will be. In some cases, the fields may not yet be supported
by Modget, but support for them will be added in the future.
