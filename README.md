# Changelog Release Action
A GitHub action to create GitHub releases that pulls the release notes from a
changelog file.

## Overview
This action can be use to create a release, where the release notes are pulled
from the section in the repository's changelog that corresponds to the version
being released. The changelog must adhere to the format used in
[Keep a Changelog][1].

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
See this repository's [`.github/workflows/release.yml`][2] file for a simple
example on how to use this action in a workflow.

[1]: <https://keepachangelog.com/>
[2]: <https://github.com/Kumodatsu/changelog-release-action/blob/master/.github/workflows/release.yml>
