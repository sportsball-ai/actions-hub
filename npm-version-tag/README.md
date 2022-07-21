# npm-version-tag

A composite action that takes in a version number and tags the next version of code using `npm` utilities

## Example Usage

```yaml
name: oe-version

on:
  pull_request:
    branches:
      - main
    types:
      - closed

jobs:
  reusable_workflow_job:
    if: github.event.pull_request.merged == true
    uses: sportsball-ai/actions-hub/.github/workflows/calculate-version.yml@<VERSION>
    with:
      version: <OE-CLI_VERSION>
    secrets: inherit

  npm_version_tag:
    if: github.event.pull_request.merged == true
    needs: reusable_workflow_job
    runs-on: ubuntu-latest
    steps:
      - name: checkout repo
        uses: actions/checkout@v3
      
      - name: echo version
        run: |
          echo ${{ needs.reusable_workflow_job.outputs.version }}

      - uses: actions/setup-node@v3
        with:
          node-version: 16

      - name: configure user
        run: |
          git config --global user.email <EMAIL>
          git config --global user.name <USERNAME>

      - name: npm tag
        uses: sportsball-ai/actions-hub/npm-version-tag@<VERSION>
        with:
          version: ${{ needs.reusable_workflow_job.outputs.version }}
```
