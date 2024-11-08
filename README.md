# action-auto-release

A composite GitHub action to create a GitHub Release with auto-generated release notes

## Guide

* [Used Actions](#used-actions)
* [Usage](#usage)
* [Inputs](#inputs)
* [Outputs](#outputs)
* [Permissions](#permissions)
* [Source](#source)

## Used Actions

The following actions are being used in this composite action...

* [`softprops/action-gh-release@v2`](https://github.com/softprops/action-gh-release/tree/v2)

## Usage

```yaml
jobs:
  create_release:
    permissions:
      contents: write
    steps:
      - name: Create GitHub Release
        uses: manferlo81/action-auto-release@v1
        with:
          tag_name: ${{ github.ref_name }}
          files: file.txt
          summary: false
```

## Inputs

| Input | Description | Required | Default |
| ----- | ----------- | -------- | ------- |
| [`summary`](#summary) | Whether or not to show release url on summary | No | "true" |
| [`tag_name`](#tag_name) | Tag name to be used for release | No | handled by [`softprops/action-gh-release@v2`](https://github.com/softprops/action-gh-release/tree/v2?tab=readme-ov-file#inputs) |
| [`token`](#token) | GitHub Personal Access Token | No | handled by [`softprops/action-gh-release@v2`](https://github.com/softprops/action-gh-release/tree/v2?tab=readme-ov-file#inputs) |
| [`make_latest`](#make_latest) | Specifies whether this release should be set as the latest release | No | handled by [`softprops/action-gh-release@v2`](https://github.com/softprops/action-gh-release/tree/v2?tab=readme-ov-file#inputs)  |
| [`files`](#files) | Newline-delimited globs of paths to assets to upload for release | No | handled by [`softprops/action-gh-release@v2`](https://github.com/softprops/action-gh-release/tree/v2?tab=readme-ov-file#inputs) |

### `summary`

Whether or not to show release url on summary. Anything other than `false` will default to `true`.

### `tag_name`

Tag name to be used for release. This input [will be passed unchanged](#source) to [`softprops/action-gh-release@v2`](https://github.com/softprops/action-gh-release/tree/v2). Check their documentation [here](https://github.com/softprops/action-gh-release/tree/v2?tab=readme-ov-file#inputs).

### `token`

GitHub Personal Access Token. This input [will be passed unchanged](#source) to [`softprops/action-gh-release@v2`](https://github.com/softprops/action-gh-release/tree/v2). Check their documentation [here](https://github.com/softprops/action-gh-release/tree/v2?tab=readme-ov-file#inputs).

### `make_latest`

Specifies whether this release should be set as the latest release. This input [will be passed unchanged](#source) to [`softprops/action-gh-release@v2`](https://github.com/softprops/action-gh-release/tree/v2). Check their documentation [here](https://github.com/softprops/action-gh-release/tree/v2?tab=readme-ov-file#inputs).

### `files`

Newline-delimited globs of paths to assets to upload for release. This input [will be passed unchanged](#source) to [`softprops/action-gh-release@v2`](https://github.com/softprops/action-gh-release/tree/v2). Check their documentation [here](https://github.com/softprops/action-gh-release/tree/v2?tab=readme-ov-file#inputs).

## Outputs

### `url`

URL to the Release HTML Page. This output is coming from [`softprops/action-gh-release@v2`](https://github.com/softprops/action-gh-release/tree/v2). Check their documentation [here](https://github.com/softprops/action-gh-release/tree/v2?tab=readme-ov-file#outputs).

### `id`

Release ID. This output is coming from [`softprops/action-gh-release@v2`](https://github.com/softprops/action-gh-release/tree/v2). Check their documentation [here](https://github.com/softprops/action-gh-release/tree/v2?tab=readme-ov-file#outputs).

### `upload_url`

URL for uploading assets to the release. This output is coming from [`softprops/action-gh-release@v2`](https://github.com/softprops/action-gh-release/tree/v2). Check their documentation [here](https://github.com/softprops/action-gh-release/tree/v2?tab=readme-ov-file#outputs).

## Permissions

[`softprops/action-gh-release@v2`](https://github.com/softprops/action-gh-release/tree/v2) requires the `contents` permission to be set to `write`. You can read more about that in [their documentation permissions section](https://github.com/softprops/action-gh-release/tree/v2?tab=readme-ov-file#permissions).

## Source

```yaml
name: GitHub Auto Release
description: A composite GitHub action to create a GitHub Release with auto-generated release notes

author: Manuel Fernández

branding:
  icon: upload
  color: purple

inputs:
  summary:
    description: Whether or not to show release url on summary
    required: false
    default: "true"

  tag_name:
    description: Tag name to be used for release
    required: false
    # default: handled by softprops/action-gh-release

  token:
    description: GitHub Personal Access Token
    required: false
    # default: handled by softprops/action-gh-release

  make_latest:
    description: Specifies whether this release should be set as the latest release.
    required: false
    # default: handled by softprops/action-gh-release

  files:
    description: Newline-delimited globs of paths to assets to upload for release
    required: false
    # default: handled by softprops/action-gh-release

outputs:
  url:
    description: "URL to the Release HTML Page"
    value: ${{ steps.create-release.outputs.url }}

  id:
    description: "Release ID"
    value: ${{ steps.create-release.outputs.id }}

  upload_url:
    description: "URL for uploading assets to the release"
    value: ${{ steps.create-release.outputs.upload_url }}

runs:
  using: composite

  steps:
    - name: Create GitHub Release
      id: create-release
      uses: softprops/action-gh-release@v2
      with:
        tag_name: ${{ inputs.tag_name }}
        token: ${{ inputs.token }}
        make_latest: ${{ inputs.make_latest }}
        generate_release_notes: true
        files: ${{ inputs.files }}

    - name: Show Release URL on summary
      if: inputs.summary != 'false'
      shell: bash
      run: |
        echo "### GitHub Release Created!" >> $GITHUB_STEP_SUMMARY
        echo "" >> $GITHUB_STEP_SUMMARY
        echo "${{ steps.create-release.outputs.url }}" >> $GITHUB_STEP_SUMMARY
```

## License

[MIT](./LICENSE) &copy; 2024 [Manuel Fernández](https://github.com/manferlo81)
