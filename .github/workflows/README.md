# calculate-version

A github action reusable workflow that uses `oe-cli` to calculate the next version of code

## Example Usage with Npm

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
    uses: sportsball-ai/actions-hub/.github/workflows/calculate-version.yml@<VERSION>
    with:
      version: <OE-CLI_VERSION>
    secrets: inherit

  npm_version_tag:
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

## Example Usage without Npm

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
    uses: sportsball-ai/actions-hub/.github/workflows/calculate-version.yml@<VERSION>
    with:
      version: <OE-CLI_VERSION>
    secrets: inherit

  git_version_tag:
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