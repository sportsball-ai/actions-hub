# git-version-tag

A composite action that takes in a version number and tags the next version of code

## Example Usage

```yaml
name: git-version-tag-example

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
      version: <TXM-CLI_VERSION>
    secrets: inherit

  git_version_tag:
    if: github.event.pull_request.merged == true
    needs: reusable_workflow_job
    runs-on: ubuntu-latest
    steps:
      - name: checkout repo
        uses: actions/checkout@v3
      
      - name: echo version
        run: |
          echo ${{ needs.reusable_workflow_job.outputs.version }}

      - name: git tag
        uses: sportsball-ai/actions-hub/git-version-tag@<VERSION>
        with:
          version: ${{ needs.reusable_workflow_job.outputs.version }}
```