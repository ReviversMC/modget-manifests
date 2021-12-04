In order for Modget not having to download all manifests when looking for a specific mod, we provide a lookup table with basic metadata like the id, various variations on the mod name and the different packages that exist.

Table of Contents
----------------------------------
Lookup Table Specification
  1. [Syntax](#syntax)
  2. [Lookup table example](#lookup-table-example)
  3. [Best Practices](#best-practices)
  4. [Migration from v3](#migration-from-the-v3-spec)



# Lookup Table Specification Version 4


## Syntax
Each field in the file must be `camelCase`d and cannot be duplicated.

**None of the tags are allowed to contain periods, spaces or non-ASCII characters!**

The only exceptions are the modid, if it contains these special characters (which it shouldn't), and the package ID, where a period separates publisher and modid.



## Lookup table example

```YAML
  # The ID of the mod, see fabric.mod.json
- id: fdlink

  # The official name defined in fabric.mod.json, potentially differing
  # Modrinth/CurseForge names and their slugs etc. (kebab-cased).
  # If a lower- and kebab-cased name equals the id above, omit it!
  # If this leads to an empty list, set `alternativeNames` to `~` or `null`.
  alternativeNames:
    - fabric-discord-link

  # Take the tags from Modrinth/CurseForge
  tags:
    - utility
    - miscellaneous
    - information

  # All packages which have the aforementioned modid
  packages:
    # The package id, so `publisher.modid`
    # (both formatted to contain no periods, spaces or non-ASCII characters)
    - packageId: CatCore.fdlink
      # All mod loaders the package supports
      loaders:
        - fabric

- id: ferritecore
  alternativeNames:
    - ferritecore
  tags:
    - utility
    - miscellaneous
    - performance
  packages:
    - packageId: malte0811.ferritecore
      loaders:
        - fabric
        - forge
# etc.
```


## Best Practices
The package identifier (`publisher.modid`) must be unique (see the [manifest spec](./manifest-spec.md)). Only one pull request per package is allowed.

**Apart from packageIds, all values have to be lower case!**

Names should contain the actual, lower-cased mod name (listed in the mod's `fabric.mod.json`), different variations of that and, if applicable, a differing CurseForge/Modrinth url name. If one of the aforementioned versions consists of multiple words, it must be `kebab-case`d.

The packages of each ID should be sorted by relevance. If multiple forks exist, the original mod (if still maintained, of course), should be listed at the top. If multiple packages have the same relevance, sort them alphabetically.

Avoid creating redundant tags. Tags should be very short and descriptive! We will add a list with accepted ones later on; for now please resort to the tags used by CurseForge/Modrinth. Note that tags also have to be `kebab-case` formatted.

Provide as many fields as possible. The more metadata you provide the better the user experience will be. In some cases, the fields may not yet be supported by Modget, but support for them will be added in the future.



## Migration from the v3 spec
You can find all changes from v3 summarized in [this](https://github.com/ReviversMC/modget-manifests/issues/8) issue.
