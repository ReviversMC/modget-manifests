# Authoring Manifests

First, we want to say thank you. Your contribution is highly valued. And we appreciate the time you have taken to get here and read this document. Let's start out with a few definitions to help you understand our vocabulary.

## Definitions

### What is a manifest?
Manifests are the YAML files in this repository containing the metadata used by Modget to install and upgrade mods. There are hundreds of these files partitioned under the [manifests](./manifests) directory. We've had to partition the directory structure so you don't have to scroll as much on GitHub when you are looking for a manifest.

### What is a package?
A package in Modget's case is basically just a mod. It is identified by the `publisher.modid` notation.

### What is a version?
Package versions are associated with a specific release. In some cases you will see a perfectly formed [semantic](https://semver.org) version number, and in other cases you might see something different. These may be date driven, or they might have other characters with some package specific meaning. The YAML key for a package version is simply `version`.


## Understanding the directory structure
Once you have determined the mod publisher and id, it is possible to know the proper location for the manifest. See the [file name and folder structure](./doc/spec/manifest-spec.md#file-names-and-folder-structure).


## First steps
Before you invest the time to generate and submit a manifest, you should check to see if the package already exists. Start out with `modget search <modid>`. If that doesn't yield anything, try a quick search using the search box in the top left corner of GitHub for the package "In this repository". If you still don't find anything, finally check to see if there is already a [PR](https://github.com/ReviversMC/modget-manifests/pulls) for the package by putting the package in the filters box, and be sure to remove the "is:pr is:open" filters.


## What next?
You should take a look at our [manifest specification](./doc/spec/manifest-spec.md). Don't worry. If this is starting to look too complicated you can create a new issue and select [Mod Request/Submission](https://github.com/ReviversMC/modget-manifests/issues/new/choose).