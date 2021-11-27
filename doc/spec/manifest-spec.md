Modget uses manifests (YAML files) to locate and install packages. This specification provides
a guide to writing these manifests as well as best practices.

Table of Contents
----------------------------------
Manifest Specification
  1. [File names and folder structure](#file-names-and-folder-structure)
  2. [Syntax](#syntax)
  3. [Main file](#main-file)
  4. [Version files](#version-files)
  5. [Best Practices](#best-practices)
  6. [Migration from v3](#migration-from-the-v3-spec)



# Manifest Specification Version 4


## File names and folder structure
Manifests shall be added to the repository with the following folder structure:<br>
`manifests / P / publisher / modid / main.yaml`

Example:<br>
`manifests / R / ReviversMC / modget / main.yaml`

- Manifests are partitioned by the first letter of the publisher name (in upper case). For example: `R`. (The partitioning scheme was added to help with GitHub's UX. Folders with thousands of children do not render well in the browser.)
- A publisher is the name of the individual or group that publishes the tool. For example: `ReviversMC`.
- The child folder `modid` is the id of the corresponding mod. For example: `modget`. It can be found in its `fabric.mod.json`.

The `publisher` and `modid` folders **must** match the values used in the lookup table. See the [lookup table spec](./lookup-table-spec.md). They must also match the values defined in the manifest itself.

**Neither the publisher nor the modid strings are allowed to contain periods, spaces or non-ASCII characters!**



## Syntax
Each field in the file must be camelCased and cannot be duplicated.



## Main file
This example manifest mixes together metadata from various different mods to show off everything you can add, so don't be confused by potentially wrong data!

```YAML
# Current manifest specification
manifestSpecVersion: 4

# GitHub organization name, otherwise name of the main uploader on CurseForge/Modrinth.
# Only here is it allowed for the publisher name to contain periods, spaces or non-ASCII characters!
publisher: CaffeineMC

iconUrls:
  # Sort from best to worst quality, otherwise Modrinth → CurseForge → GitHub/GitLab etc.
  # We only have multiple links here in case one platform's servers go down, so even if
  # there are multiple GitHub branches for example, only put one GitHub link here!
  - https://cdn.modrinth.com/data/AANobbMI/icon.png
  - https://media.forgecdn.net/avatars/284/773/637298471098686391.png
  - https://raw.githubusercontent.com/CaffeineMC/sodium-fabric/1.17.x/dev/src/main/resources/assets/sodium/icon.png

# Is this mod `active`, `eol`, `abandoned` or is its status `unknown`?
## - `abandoned`: The mod developer has explicitly said he won't update this mod anymore,
##    or there hasn't been an update in several months/years
## - `eol`: End of life; the mod is still receiving bug fixes every now and then,
##    but isn't being ported to newer MC versions anymore
## - `unknown`: The mod developer hasn't explicitly noted he won't update the mod anymore,
##    but hasn't done the opposite either and the mod hasn't received updates in a long time
status: active

# If there is no working version of this mod for newer Minecraft versions and other people
# have ported it / mods of very similar scope exist
updatedAlternatives:
  - packageId: ExamplePublisher.sodium-revived

# Name of the mod, see CurseForge/Modrinth or fabric.mod.json
name: Sodium

# Short description from CurseForge/Modrinth/Github or fabric.mod.json
description: Sodium is a free and open-source optimization mod for Minecraft which improves frame rates and reduces lag spikes.

# Take the following values from the mod's fabric.mod.json
authors:
  - name: JellySquid
  - name: 2No2Name
home: https://caffeinemc.net
source: https://github.com/CaffeineMC/sodium-fabric
issues: https://github.com/CaffeineMC/sodium-fabric/issues

# An official place where users can get support from the developers
support: https://jellysquid.me/discord

# Link to an *official* wiki, no third-party one
wiki: https://github.com/CaffeineMC/lithium-fabric/wiki

chats:
  discord: https://jellysquid.me/discord
  irc: irc://irc.esper.net:6667/fabric
  others:
    - name: Gitter
      url: https://gitter.im/Open-Shell

# All versions of the mod we support. Copy them from fabric.mod.json, and only modify them
# if they aren't semver compliant!
versions:
  # Sort from latest to oldest
  - version: 0.3.2+build.7
  - version: 0.2.0+build.4
  - version: 0.1.0+1.16.3
  - version: 0.1.0
```

Some tags/arrays, like `iconUrls`, `chats` etc. have to be set to `~` or `null` if no such thing exists for the given mod. If the mod doesn't have an icon for example, simply set the `iconUrls` array to `~` or `null`!
For collections with sub-tags like `chats`, only set the sub-tags to `~` or `null`, not the whole collection (e.g.: `discord: ~`).



## Version files
Version files are located in the same directory as the main manifest file, but are further partitioned into subfolders to enhance readability, but with two subfolders at most. See this example:
```
publisher/
├──modid/
│   ├── main.yaml
│   ├── 0.x/
│   │   ├── 0.3.x/
│   │   │   ├── 0.3.2.yaml
│   │   │   ├── 0.3.1.yaml
│   │   │   ├── 0.3.0.1.yaml
│   │   │   └── 0.3.0.yaml
│   ├── 1.x/
│   │   ├── 1.0.x/
│   │   │   ├── 1.0.0.yaml
│   │   │   ├── 1.0.0.1.yaml
│   │   │   └── 1.0.1.yaml
│   │   ├── 1.1.x/
│   │   │   ├── 1.1.0.yaml
│   │   │   ├── 1.1.1.yaml
│   │   │   └── 1.1.2.yaml
│   ├── 2.x/
│   │   ├── 2.0.x/
│   │   │   ├── 2.0.0.yaml
```
The files' names have to **exactly** match the version number defined in the main manifest file!

The following example file mixes together metadata from various different mods to show off everything you can add, so don't be confused by potentially wrong data!

```YAML
  # All mod loaders this version supports. Can be `fabric`, `forge` and/or `liteloader`
- loaders:
    - fabric
    - forge

  # All Minecraft versions this mod version supports
  minecraftVersions:
    # Sort from latest to oldest
    - 1.17.1
    - 1.17

  # On which sides can this mod be installed? See the mod's Modrinth page.
  # Possible values: `unsupported`, `optional` or `required`
  environment:
    server: unsupported
    client: required

  # Is this an `alpha`, `beta` or `release` version? See CurseForge/Modrinth.
  channel: release

  # Note that for the following arrays (except `bundles`), the `version` tag's value has to be
  # in quotation marks, otherwise symbols like `>` or `*` will cause issues with the YAML parser!
  ## Which other mods does this version need to run? See fabric.mod.json.
  depends:
    - packageId: FabricMC.fabric
      version: ">=0.41.1"
  ## Which other mods does this version bundle? See build.gradle (`include`).
  bundles:
    - packageId: FabricMC.fabric
      version: 0.41.1
  # Which other mods is this version incompatible with? See fabric.mod.json.
  breaks:
    - packageId: Chocohead.optifabric
      version: "*"
    - packageId: vram-guild.canvas
      version: "*"
  ## Mods which cause weird behavior, bugs etc. when used with this version
  conflicts: ~
  ## Optional dependencies not required to run this version. See fabric.mod.json.
  recommends:
    - packageId: CatCore.server_translations
      version: "*"

  # Which CurseForge/Modrinth project IDs has this version been released under?
  thirdPartyIds:
    modrinth: AANobbMI
    curseforge: 394468

  # The version's license. See its fabric.mod.json or, of not specified there, its GitHub page.
  license: LGPL-3.0-only

  # The file type; usually `jar`, or for some older mods, `zip`
  fileType: jar

  # The MD5 hash of the file. See CurseForge, or generate it yourself
  # (e.g. here: https://emn178.github.io/online-tools/md5_checksum.html)
  md5: b88cf07d124c5b96b82ab27e51fdff04

  # The page from where you can directly download this version with just one button click.
  # Unless explicitly permitted, don't add any mod repost sites!
  # You can see a list of them here: https://stopmodreposts.org/sites.html
  downloadPageUrls:
    modrinth: ~
    curseforge: https://www.curseforge.com/minecraft/mc-mods/sodium/files/3488836
    sourceControl: https://github.com/CaffeineMC/sodium-fabric/releases/tag/mc1.17.1-0.3.2
    others: ~


  # A direct link to the mod file. As above, don't put any mod repost sites here
  # unless explicitly permitted!
  fileUrls:
    modrinth: ~
    # Tip: Simply replace `files` with `download` and append `/file` to all CurseForge
    # downloadPageUrls to get a direct download link!
    curseforge: https://www.curseforge.com/minecraft/mc-mods/sodium/download/3488836/file
    sourceControl: https://github.com/CaffeineMC/sodium-fabric/releases/download/mc1.17.1-0.3.2/sodium-fabric-mc1.17.1-0.3.2+build.7.jar
    others:
      - name: Example site
        url: https://example.com/
```

As with the main file, tags/arrays like `depends` or `breaks` have to be set to `~` or `null` if no such thing exists for the given mod. If the mod doesn't have strict dependencies for example, simply set the `depends` array to `~` or `null`!
For collections with sub-tags like `thirdPartyIds`, only set the sub-tags to `~` or `null`, not the whole collection (e.g.: `modrinth: ~`).



## Best Practices
The package identifier (`publisher.modid`) must be unique. Only one pull request per package version is allowed.

Avoid creating multiple publisher folders. For example, do not create a `JellySquid3` if there is already a `JellySquid` folder.

Provide as many fields as possible. The more metadata you provide, the better the user experience will be. In some cases, the fields may not yet be supported by Modget, but support for them will be added in the future.



## Migration from the v3 spec
You can find all changes from v3 summarized in [this](https://github.com/ReviversMC/modget-manifests/issues/8) issue.
