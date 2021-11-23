Modget uses manifests (YAML files) to locate and install packages. This specification provides
a guide to writing these manifests as well as best practices.

Table of Contents
----------------------------------
YAML Specification
   1. YAML file name and folder structure
   2. YAML Syntax
   3. YAML file example
   4. Best Practices

# YAML Specification

## YAML file name and folder structure
YAML files shall be added to the repository with the following folder structure:<br>
`manifests / P / publisher / modid / publisher.modid.yaml`

Example:<br>
`manifests / R / ReviversMC / modget / ReviversMC.modget.yaml`

- Manifests are partitioned by the first letter of the publisher name (in upper case). For example: `R`. (The partitioning scheme was added to help with GitHub's UX. Folders with thousands of children do not render well in the browser.)
- A publisher is the name of the individual or group that publishes the tool. For example: `ReviversMC`.
- The child folder `modid` is the id of the corresponding mod. For example: `modget`. It can be found in its `fabric.mod.json`.
- The filename must be the combination of the publisher and the modid, separated by a period. For example: `ReviversMC.modget.yaml`.

The `publisher` and `modid` folders **must** match the values used in the lookup table. See the [lookup table spec](./lookup-table-spec-v2.md). They must also match the values defined in the manifest itself.

**Neither the publisher nor the modid strings are allowed to contain periods, spaces or non-ASCII characters!**

## YAML Syntax
Each field in the file must be camelCased and cannot be duplicated.

## YAML file example
Partly taken from: [`manifests / C / CaffeineMC / sodium / CaffeineMC.sodium.yaml`](../manifests/C/CaffeineMC/sodium/CaffeineMC.sodium.yaml)

```YAML
manifestSpecVersion: 3.1
publisher: CaffeineMC
id: sodium
modType: jar
side: client
name: Sodium

# All values in the following block are optional
description: Sodium is a free and open-source optimization mod for Minecraft which improves frame rates and reduces lag spikes.
license: LGPL-3.0-only
home: https://caffeinemc.net
source: https://github.com/CaffeineMC/sodium-fabric
issues: https://github.com/CaffeineMC/sodium-fabric/issues
support: https://jellysquid.me/discord
wiki: https://github.com/CaffeineMC/sodium-fabric/wiki
chat: https://jellysquid.me/discord

thirdPartyIds:
  curseforge: 394468
  modrinth: AANobbMI
downloads:
  - version: 0.1.0
    loaders:
      - fabric
    md5: 12fe6974813ae5d514af57c3ac679966
    minecraftVersions:
      - 1.16.2
      - 1.16.3
      - 1.16.4
      - 1.16.5

    # All values in the following block are optional
    depends:
      - packageId: FabricMC.fabric
        version: ">=0.42.0"
    breaks:
      - packageId: Chocohead.optifabric
        version: "*"
    recommends:
      - packageId: CaffeineMC.lithium
        version: ">=0.6.6"
      - packageId: CaffeineMC.phosphor
        version: ">=0.7.2"

    downloadPageUrls:
      - name: CurseForge
        url: https://www.curseforge.com/minecraft/mc-mods/sodium/files/3067101
      - name: Modrinth
        url: https://modrinth.com/mod/sodium/version/yaoBL9D9
      - name: GitHub
        url: https://github.com/CaffeineMC/sodium-fabric/releases/tag/mc1.16.1-0.1.0
    fileUrls:
      - name: CurseForge
        url: https://www.curseforge.com/minecraft/mc-mods/sodium/download/3067101/file
      - name: Modrinth
        url: https://cdn.modrinth.com/data/AANobbMI/versions/mc1.16.3-0.1.0/sodium-fabric-mc1.16.3-0.1.0.jar
      - name: GitHub
        url: https://github.com/CaffeineMC/sodium-fabric/releases/download/mc1.16.3-0.1.0/sodium-fabric-mc1.16.3-0.1.0.jar
```


### Information on some keys
- `modType`: Can currently either be `jar` or `zip`. Please note that only `jar` is supported as of now!
- `side`: Can be `client`, `server` or `both`. This prevents the user from installing mods on the wrong side.
- `thirdPartyIds`: Is required for [Automatic PRs through GitHub workflows #1](https://github.com/ReviversMC/modget-manifests/issues/1). Can only be omitted if the mod isn't on CurseForge or Modrinth.
- `loaders`: Can be `fabric`, `forge` and/or `liteloader`.


## Best Practices
The package identifier (`publisher.modid`) must be unique. Only one pull request per package version is allowed.

Avoid creating multiple publisher folders. For example, do not create a `JellySquid3` if there is already a `JellySquid` folder.

Provide as many fields as possible. The more metadata you provide, the better the user experience will be. In some cases, the fields may not yet be supported by Modget, but support for them will be added in the future.
