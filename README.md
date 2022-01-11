# Changelog Release Action
A GitHub action to create GitHub releases that pulls the release notes from a
changelog file.

If you like this action but find an issue or are missing something while using
it, please create an issue and/or a pull request.

## Overview
This action can be use to create a release, where the release notes are pulled
from the section in the repository's changelog that corresponds to the version
being released. The changelog must adhere to the format used in
[Keep a Changelog][1]. See the image for an example release created with this
action.

![release example](/assets/example-release.png)

## How to use
### Inputs
<table>
    <tr>
        <th>Name</th>
        <th>Required</th>
        <th>Default value</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>github_token</td>
        <td>Yes</td>
        <td>N/A</td>
        <td>
            A GitHub token. This is used to create the release on the
            repository.
        </td>
    </tr>
    <tr>
        <td>tag</td>
        <td>Yes</td>
        <td>N/A</td>
        <td>
            The tag on which the release should be created (string starting with
            "refs/tags/").
        </td>
    </tr>
    <tr>
        <td>version</td>
        <td>No</td>
        <td>N/A</td>
        <td>
            The semantic version number of the relevant section in the
            changelog. If unspecified, the tag name is assumed to be the version
            number (possibly preceded by a 'v').
        </td>
    </tr>
    <tr>
        <td>changelog_path</td>
        <td>No</td>
        <td>"./CHANGELOG.md"</td>
        <td>
            The path to the changelog file, relative to the repository root.
        </td>
    </tr>
    <tr>
        <td>release_name</td>
        <td>No</td>
        <td>"$VERSION"</td>
        <td>
            The name for the release. Use "$VERSION" to insert the version
            number.
        </td>
    </tr>
    <tr>
        <td>draft</td>
        <td>No</td>
        <td>false</td>
        <td>
            true to create a draft release; false to create a public release.
        </td>
    </tr>
    <tr>
        <td>prerelease</td>
        <td>No</td>
        <td>false</td>
        <td>
            true to mark the release as a prerelease; false otherwise.
        </td>
    </tr>
</table>

### Outputs
<table>
    <tr>
        <th>Name</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>upload_url</td>
        <td>
            The asset upload url of the created release.
        </td>
    </tr>
    <tr>
        <td>changelog_section</td>
        <td>
            The contents of the relevant section from the changelog.
        </td>
    </tr>
</table>

### Example usage
Imagine you have a repository with a `CHANGELOG.md` file in the root which has
the following content:

```md
# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to
[Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.2.0] - 2022-01-11
### Added
- Added some stuff.

### Changed
- Changed a thing.
- Changed another thing too, which requires a description so long that it is
split over multiple lines.

## [0.1.0] - 2022-01-01
### Added
- Files and such.
```

Furthermore, imagine there is also a `README.md` file in the root which you want
to include in the release as an asset. One could use the following workflow:

```yaml
name: Create release

# Triggered when a tag is pushed whose name starts with "v".
on:
  push:
    tags:
      - v*

jobs:
  create-release:
    name:    Create release
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v2
      - name: Create release
        id:   create-release
        uses: Kumodatsu/changelog-release-action@v0.3.0
        with:
          # Pass your secrets.GITHUB_TOKEN, which is needed to create a release
          # for your repository.
          github_token: ${{ secrets.GITHUB_TOKEN }}
          # As this job is triggered when a tag is pushed, ${{ github.ref }}
          # evaluates to the tag name (for example, "refs/tags/v0.2.0"). Passing
          # this will create the release at this tag.
          tag:          ${{ github.ref }}
          # The name of the release. $VERSION will be replaced by the version in
          # the tag name. For example, a release for the tag v0.2.0 will have
          # the name "Version 0.2.0" here.
          release_name: Version $VERSION
      - name: Upload release assets
        id:   upload-assets
        uses: actions/upload-release-asset@v1
        with:
          # You can use the upload_url output from this action to upload assets.
          upload_url:         ${{ steps.create-release.outputs.upload_url }}
          asset_path:         "README.md"
          asset_name:         "README.md"
          asset_content_type: text/markdown
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }
```

Now, creating and pushing a tag named "0.2.0" or "v0.2.0" will result in a
release that looks like this:

![release example](/assets/example-release.png)

Note that the second point under "Changed" had a newline in the `CHANGELOG.md`
file. Normally, newlines like these will be displayed in GitHub release notes.
This action takes them out, [as would be expected in Markdown][3]. This is
fortunate if you want to keep the lines in your source short without having to
worry about the formatting of the release notes.

You can also take a look at this repository's own
[`.github/workflows/release.yml`][2] file for another simple example of how this
action can be used.

[1]: <https://keepachangelog.com/>
[2]: <https://github.com/Kumodatsu/changelog-release-action/blob/master/.github/workflows/release.yml>
[3]: <https://daringfireball.net/projects/markdown/syntax#p>
