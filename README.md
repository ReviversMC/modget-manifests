# Modget Manifest Repository
This repository contains the manifest files for the **Modget** default source. You are highly encouraged to submit manifests for your favorite mod.
> Note: At this time mods must be .JARs. Standalone executables or compressed .zip files are not currently supported.

[**Modget**](https://github.com/ReviversMC/modget) is an open source Minecraft mod package manager.


# Submitting a Package
To submit a package to this repository, you should follow these steps:
1. Follow the [Contributing](#contributing) guidelines below.
2. [Author](#authoring-a-manifest) a Manifest.
3. [Test](#testing-your-manifest) your manifest.
4. [Submit](#submitting-your-pr) your pull request (PR).
5. Respond to any feedback in your PR.

> Note: Please check the package's manifest you intend to submit does not already exist in the repository, and there are no open PRs for it in order to avoid duplicates.

## Authoring a Manifest
In the future, you will be able to use the [Modget Manifest Creator](https://github.com/ReviversMC/modget-create). Currently, you have to craft a manifest manually following our [authoring guidelines](AUTHORING_MANIFESTS.md).

> Note: Only one manifest may be submitted per PR.

## Testing your manifest
Now that you have authored your manifest, you should make sure it works as expected.

> ❗ Note: Modget doesn't support the following commands yet!

1. Verify the syntax by executing the following command:
```
modget validate <manifest-name>
```

2. Test the install by executing the following command:
```
modget install -m <manifest-name>
```
The manifests have to be located inside the `.minecraft/config/modget/custom-manifests/` folder.

## Submitting your PR
With the manifest verified, you will need to submit a PR. Your manifest should be located in the folder path matching `manifests / <first upper case letter of publisher> / <Publisher> / <ModID> / Publisher.ModID.yaml`

### Validation Process
The PR request will go through a validation process. During the process, the core team or (in the future) the Modget bot will use [labels](https://docs.microsoft.com/windows/package-manager/package/winget-validation#pull-request-labels) to help. In the event of a failure, the bot will suggest where the problem is with the submission and assign the PR back to you.

### Respond to PR feedback
If the PR has been assigned to you, a timer is triggered. You will have 7 days to resolve the issue, or the PR will be closed automatically by the bot.

Submissions to the repository are reviewed by Modget administrators and/or community moderators.

#### Modget administrators:
- [@NebelNidas](https://github.com/NebelNidas)

<!-- #### Community administrators:
- ... -->


# Contributing
This project welcomes contributions and suggestions.

For the avoidance of doubt, you may not make any submissions linking to third party materials if such submission is prohibited by the applicable third party and/or otherwise violates such third party's rights.
