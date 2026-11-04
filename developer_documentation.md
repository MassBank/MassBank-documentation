# Developer documentation

## Installation and API

An installation guide and examples for the REST API are available in the README of [MassBank3](https://github.com/MassBank/MassBank3).

## Release process of [MassBank-data](https://github.com/MassBank/MassBank-data)

MassBank-data uses two long-lived branches:

- `dev` is the development branch and is the source for the next release.
- `main` contains released data. A commit merged into `main` represents a release candidate and is tagged with the release version.

GitHub Actions validates pull requests and both long-lived branches with the Validator from [MassBank-web](https://github.com/MassBank/MassBank-web). Do not use the former Travis CI configuration or the deprecated `hub` tool.

### Prerequisites

Install and authenticate the [GitHub CLI](https://cli.github.com/):

```sh
gh auth login
gh auth status
```

The commands below assume that the local checkout is the `MassBank-data` repository and that you have permission to push branches and merge pull requests.

### Prepare a release branch

Start from an up-to-date `dev` branch and create a release branch. Replace `2026.04` with the release version.

```sh
git switch dev
git pull --ff-only origin dev
git switch -c release-2026.04
./bump-version.sh 2026.04
git add VERSION
git commit -m "Bump version to 2026.04"
git push --set-upstream origin release-2026.04
```

`bump-version.sh` writes the version and the timestamp of the current commit to `VERSION`.

### Create and merge the release pull request

Create a pull request from the release branch into `main` with `gh`:

```sh
gh pr create \
  --base main \
  --head release-2026.04 \
  --title "Release version 2026.04" \
  --body "Prepare MassBank-data release 2026.04."
```

Use the URL printed by `gh pr create` to request or complete the required reviews. Check the GitHub Actions jobs from the command line:

```sh
gh pr checks <PR-number> --watch
```

Only merge after the checks have passed and the required reviews are present. The following command merges the pull request on GitHub and removes the remote release branch:

```sh
gh pr merge <PR-number> --merge --delete-branch
```

If the repository uses a merge queue, use `--auto` instead and let GitHub merge the pull request after the requirements are met:

```sh
gh pr merge <PR-number> --merge --auto --delete-branch
```

### Create the GitHub release

After the pull request has been merged, create the release from the resulting `main` branch. `gh release create` creates the tag and the GitHub release. GitHub Actions then creates the release artifacts.

```sh
git switch main
git pull --ff-only origin main
gh release create 2026.04 \
  --target main \
  --title "Release version 2026.04" \
  --generate-notes
```

Verify the release and its assets with:

```sh
gh release view 2026.04
```

Released versions are archived through the MassBank Zenodo integration and receive a DOI. Check the [MassBank-data releases](https://github.com/MassBank/MassBank-data/releases) and the [Zenodo record](https://doi.org/10.5281/zenodo.3378723) after publication.

### Prepare the next development version

After the release, merge `main` back into `dev`. Update the version to the
next snapshot version in the same branch and commit.

```sh
git switch dev
git pull --ff-only origin dev
git fetch origin main
git merge --no-ff origin/main -m "Merge release 2026.04 into dev"
# Resolve conflicts if necessary, especially in VERSION.
./bump-version.sh 2026.04.1-SNAPSHOT
git add VERSION
git commit -m "Bump version to 2026.04.1-SNAPSHOT"
git push origin dev
```

The next development version is validated by the GitHub Actions workflow for the `dev` branch.

## Jupyter Notebook

The [MassBank Analytics](https://github.com/MassBank/MassBank-analytics) repository provides an example of fetching data from the MassBank API and using it for plotting. It can also be run directly as a [Jupyter notebook on Binder](https://mybinder.org/v2/gh/MassBank/MassBank-analytics/HEAD?urlpath=%2Fdoc%2Ftree%2Frecord_grow.ipynb).
