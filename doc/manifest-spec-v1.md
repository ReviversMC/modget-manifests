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
`manifests / P / Publisher / ModID / Publisher.ModID.yaml`

Example:<br>
`manifests / R / ReviversMC / Modget / ReviversMC.Modget.yaml`

- Manifests are partitioned by the first letter of the publisher name (in upper case). For example: `R`. (The partitioning scheme was added to help with GitHub's UX. Folders with thousands of children do not render well in the browser.)
- A `Publisher` is the name of the individual or group that publishes the tool. For example: `ReviversMC`.
- The child folder `ModID` is the capitalized ID of the corresponding mod. For example: `Modget`.
- The filename must be a combination of the publisher and the mod ID. For example: `ReviversMC.Modget.yaml`.

The `publisher` and `modID` folders **must** match the values used in the lookup table. See the [lookup table spec](https://github.com/ReviversMC/modget-manifests/tree/master/doc/lookup-table-spec-v1.md). They must also match the values defined in the manifest itself.

**Neither the publisher nor the mod id strings are allowed to contain periods, spaces or non-ASCII characters!**

## YAML Syntax
Each field in the file must be camelCased and cannot be duplicated.

## YAML file example
Path: [`manifests / C / CaffeineMC / Sodium / CaffeineMC.Sodium.yaml`](https://github.com/ReviversMC/modget-manifests/tree/master/manifests/C/CaffeineMC/Sodium/CaffeineMC.Sodium.yaml)

```YAML
manifestSpecVersion: 1.0
publisher: CaffeineMC
name: Sodium
id: sodium
modType: JAR
downloads:
  - version: 0.1.0
    minecraftVersions:
      - 1.16.3
      - 1.16.4
      - 1.16.5
    md5: 12fe6974813ae5d514af57c3ac679966
    urls:
      - https://media.forgecdn.net/files/3067/101/sodium-fabric-mc1.16.3-0.1.0.jar
      - https://cdn.modrinth.com/data/AANobbMI/versions/mc1.16.3-0.1.0/sodium-fabric-mc1.16.3-0.1.0.jar
      - https://github.com/CaffeineMC/sodium-fabric/releases/download/mc1.16.3-0.1.0/sodium-fabric-mc1.16.3-0.1.0.jar
```

### Optional Keys

```YAML
license: LGPL-3.0
description: A Minecraft mod designed to improve frame rates and reduce micro-stutter
home: https://jellysquid.me/projects
source: https://github.com/CaffeineMC/sodium-fabric
issues: https://github.com/CaffeineMC/sodium-fabric/issues
support: https://jellysquid.me/discord
```


## Best Practices
The package identifier (`Publisher.ModID`) must be unique. Only one pull request per package version is allowed.

Avoid creating multiple publisher folders. For example, do not create a `JellySquid3` if there is already a `JellySquid` folder.

Provide as many fields as possible. The more metadata you provide the better the user experience will be. In some cases, the fields may not yet be supported
by Modget, but support for them will be added in the future.
