# Installation of MassBank

An installation guide and examples of how to use the REST API can be found in the README of the spectral reference library's next generation software product, [MassBank3](https://github.com/MassBank/MassBank3).

# Release process of [MassBank-data](https://github.com/MassBank/MassBank-data)

The release strategy of MassBank-data is similar to the one of MassBank-web and is described below. Additionally it needs to be taken care of the validation and the codebase for the validation.

## Main branches

We use two main branches, `main` and `dev`. All development should happen in `dev` and we define every commit to `main` to be a release. When new data in the `dev` branch has reached a stable point and is ready to be released, all of the changes should be merged back into `main` somehow and then tagged with a release number. The data of `main` should be validated with the `main` of MassBank-web and the data of `dev` should be validated with `dev` of MassBank-web. If the data contains incompatible changes MassBank-web needs to be released first, because before a merge to `main` is possible a validation needs to be successfully. The code base for the validation is defined in travis.yml. Depending on the given version the `bump-version.sh` script adjusts the content of travis.yml to choose the corresponding branch from MassBank-web. If the version tag contains `SNAPSHOT` like in `2019.09-SNAPSHOT` its identified as `dev` data branch and travis will pull the `dev` branch of MassBank-web for validation. If `SNAPSHOT` is missing its identified to be a release version and travis will pull the `main` branch of MassBank-web for validation. All versions which get a release tag in github are used by a webhook from zenodo and get a DOI attached. The authors list of the record at zenodo needs to be manually edited to contain `MassBank-consortium and its contributors`.

The release process is discussed in detail below. To use all of the command lines below the [github/hub](https://github.com/github/hub) tool is required.

### Release branches

Release branches are created in preparation of a release.

Branch off from: `dev`

Must merge back into: `main`

Branch naming: `release-*`

Release branches support preparation of a new production release. They allow for preparing the version number for a release.

#### Creating a release branch

```
$ git checkout -b release-2019.09 dev
Switched to a new branch "release-2019.09"
$ ./bump-version.sh 2019.09
Set version of release to 2019.09. This version will validate against the main branch of MassBank-web.
git add .travis.yml VERSION
git commit -m "Bumped version number to 2019.09"
[release-2019.09 74d9424] Bumped version number to 2019.09
$ git push --set-upstream origin release-2019.09
```

#### Finishing a release branch

When the state of the release branch is ready to become a real release, the release branch is merged into `main` with a pull request and tagged for easy future reference.

```
$ hub pull-request -m 'Release version 2019.09' -b main
```

Wait for all checks to finish. If all tests are successfull the release can be merged to `main` and tagged as release.

```
git merge main
```

Resolve conflicts and commit.

```
$ git checkout main
$ git merge --no-ff release-2019.09
$ git push origin main
$ hub release create -m 'Release version 2019.09' 2019.09
```

Now we are done and the release branch may be removed

```
$ git branch -d release-2019.09
Deleted branch release-2019.09 (was ff452fe).
```

and the version of the `dev` branch should be incremented.

```
$ git checkout dev
$ ./bump-version.sh 2019.09.1-SNAPSHOT
Set version of release to 2019.09.1-SNAPSHOT. This version will validate against the dev branch of MassBank-web.
git commit -a -m "Bumped version number to 2019.09.1-SNAPSHOT"
[dev 74d9424] Bumped version number to 2019.09.1-SNAPSHOT
$ git push
```

# Jupyter Notebook

The [MassBank Analytics](https://github.com/MassBank/MassBank-analytics) repository provides an example of how to fetch data from the MassBank API and how to use it for simple plotting. This can also be directly tested in a [Jupyter notebook](https://mybinder.org/v2/gh/MassBank/MassBank-analytics/HEAD?urlpath=%2Fdoc%2Ftree%2Frecord_grow.ipynb).
