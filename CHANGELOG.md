# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog][1], and this project adheres
to [Semantic Versioning][2].

[1]: <https://keepachangelog.com/en/1.0.0/>
[2]: <https://semver.org/spec/v2.0.0.html>

## [0.3.0][] - 2022-01-11
### Changed
- The `tag-name` input has been renamed to `tag` and now expects the full tag
string, including the `refs/tags/` prefix. This allows one to simply pass in
`${{ github.ref }}` in tag-triggered workflows.
- The `version` input is no longer required. When unspecified, the version
number is extracted from the tag name (which may start with a "v").
- The names of input and output values now use snake case instead of kebab case,
for consistency with the standard create-release action. **This is a breaking
change.**
- The `release_name` input now defaults to just "$VERSION" instead of
"v$VERSION".
- Updated readme file to reflect above changes.

## [0.2.0][] - 2022-01-11
### Added
- Release workflow to automatically create releases from tags.
- `release-name` input to control the name of the created release.

### Changed
- Updated the readme file with an overview and documentation of the input and
outputs.

## [0.1.0][] - 2022-01-11
### Added
- The changelog release action.
- Readme file.
- Changelog file.
- GNU General Public License version 3.

[0.1.0]: <https://github.com/Kumodatsu/changelog-release-action/releases/tag/v0.1.0>
[0.2.0]: <https://github.com/Kumodatsu/changelog-release-action/releases/tag/v0.2.0>
[0.3.0]: <https://github.com/Kumodatsu/changelog-release-action/releases/tag/v0.3.0>
