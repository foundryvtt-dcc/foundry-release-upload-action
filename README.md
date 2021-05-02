# GitHub Action to Upload zip to AWS S3 and manifest to `dcc-content` After a Release is Published

This GitHub Action enables you to release a Foundry VTT System or Module by simply updating the 'version.txt' file in your main branch.

This is to be used with the `foundry-release-action`, and runs after a release is published to copy the manifest into main with the latest version information.

## Install Instructions

Create a folder named `.github` at the root of your workflow, and inside that folder, create a `workflows` folder.

In the `workflows` folder, create a file named `foundry_release_upload.yml` with this content:

```
name: Foundry Release Upload

on:
  release:
    types:
      - published

jobs:
  upload_release:
    runs-on: ubuntu-latest
    name: Foundry Release Upload
    steps:
      - name: Checkout
        uses: actions/checkout@v2
        with:
          token: ${{ secrets.GITHUB_CONTENT_TOKEN }}
          ref: main
          repository: foundryvtt-dcc/dcc-content

      - name: Foundry Manifest Update
        id: foundry-manifest-update
        uses: foundryvtt-dcc/foundry-manifest-update-action@main
        with:
          actionToken: ${{ secrets.GITHUB_TOKEN }}
          manifestFileName: 'system.json'
```

For `manifestFileName` you will either enter `system.json` or `module.json` depending on your project.

You should not need to change `actionToken` from the example above.