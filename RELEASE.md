# Release Process for HCL VoltMX Go Provisioner

This document outlines the release process for the `STARTcloud/hcl_voltmx_go_provisioner` project.

## Automated Release with Release Please and GitHub Actions

We aim to automate the release cycle using [Release Please Action](https://github.com/google-github-actions/release-please-action) integrated with GitHub Actions. This process relies on [Semantic Versioning (SemVer)](https://semver.org/) and [Conventional Commits](https://www.conventionalcommits.org/).

### How it Works

1.  **Conventional Commits**: Commit messages should follow the Conventional Commits specification. This allows `release-please` to determine the type of change (fix, feature, breaking change) and calculate the next semantic version.
    *   `fix: Some bug fix` -> increments PATCH version (e.g., 1.0.0 -> 1.0.1)
    *   `feat: Add new feature` -> increments MINOR version (e.g., 1.0.1 -> 1.1.0)
    *   `feat!: Add breaking change` or `fix!: Introduce breaking fix` (or a commit with `BREAKING CHANGE:` in the footer) -> increments MAJOR version (e.g., 1.1.0 -> 2.0.0)

2.  **Release PR Creation**: When commits that trigger a new version are merged into the `main` branch, `release-please` will automatically create or update a "Release PR". This PR will contain:
    *   The proposed new version number.
    *   An updated `CHANGELOG.md` with changes since the last release.
    *   An updated `CHANGELOG.md` with changes since the last release.
    *   An updated `provisioner.yml` with the new version number (if applicable, or this step might be manual).

3.  **Merging the Release PR**: Once the Release PR is reviewed and merged by a maintainer, the following actions are triggered by a separate GitHub Actions workflow (or performed manually):
    *   A GitHub Release is created with the new version tag (e.g., `v0.2.0`).
    *   The `CHANGELOG.md` changes (and `provisioner.yml` version update, if automated) are part of the tagged commit.
    *   The HCL VoltMX Go Provisioner project is packaged (e.g., as a zip file containing the necessary structure for SHI).
    *   The packaged provisioner artifact is uploaded to the GitHub Release.

## Manual Release Steps (Fallback or for Initial Setup)

If manual intervention is needed or before full automation:

1.  **Prepare the Release Branch**: Ensure your local `main` branch is up-to-date with the remote. Create a release branch if preferred (e.g., `release/v1.2.3`).
2.  **Update `CHANGELOG.md`**: Manually update `CHANGELOG.md` for the `hcl_voltmx_go_provisioner` project to include all changes since the last release. Group changes by type (Added, Changed, Fixed, Removed, Deprecated, Security).
3.  **Update `provisioner.yml`**: Increment the `version` in the main `provisioner.yml` file according to SemVer (e.g., `version: 0.2.0`).
4.  **Commit Changes**: Commit the updated `CHANGELOG.md` and `provisioner.yml`.
    ```bash
    git add provisioner.yml CHANGELOG.md
    git commit -m "chore(release): prepare for v0.2.0"
    ```
5.  **Tag the Release**: Create an annotated Git tag.
    ```bash
    git tag -a v0.2.0 -m "Version 0.2.0"
    ```
6.  **Push Changes and Tag**:
    ```bash
    git push origin main # or your release branch
    git push origin v0.2.0
    ```
7.  **Package the Provisioner**: Create an archive (e.g., a `.zip` file) of the `hcl_voltmx_go_provisioner` directory, ensuring it includes all necessary files for SHI import (`provisioner.yml`, `templates/`, `provisioners/ansible_collections/`, etc.), and excluding unnecessary files like `.git/`.
8.  **Create GitHub Release**: Go to the `STARTcloud/hcl_voltmx_go_provisioner` repository's "Releases" page on GitHub and create a new release.
    *   Use the tag you just pushed (e.g., `v0.2.0`).
    *   Set the release title (e.g., `Version 0.2.0` or `v0.2.0`).
    *   Copy the relevant section from `CHANGELOG.md` into the release description.
    *   Upload the packaged provisioner artifact (e.g., `hcl_voltmx_go_provisioner-v0.2.0.zip`) as a binary asset.

## Changelog Format

The `CHANGELOG.md` should be maintained in a format similar to [Keep a Changelog](https://keepachangelog.com/en/1.0.0/). When automated with `release-please`, it will follow the tool's conventions based on Conventional Commits.
