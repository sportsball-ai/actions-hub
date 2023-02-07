# calculate-version

A github action reusable workflow that uses `txm-cli` to calculate the next version of code

## Example Usage with Npm

```yaml
name: txm-version

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

## Example Usage without Npm

```yaml
name: txm-version

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

# publish-to-confluence

A configurable github action reusable workflow that will publish specified documents to Confluence.
Outputs the confluence url of the published document(s) as an environment variable called $confluence_url.
See https://github.com/justmiles/go-markdown2confluence for more details.

## Example Usage

```yaml
name: publish README to confluence

on:
  pull_request:
    branches:
      - main
    types:
      - closed

jobs:
  publish_to_confluence:
    if: github.event.pull_request.merged == true
    uses: sportsball-ai/actions-hub/.github/workflows/publish-to-confluence.yml@v2
    secrets: inherit
    with:
      comment: "this is a comment..." # add an optional comment to the published page
      exclude: ".*temp.md" # List of exclude file patterns (regex) that will be applied on markdown file paths.
      modified_since: 10 # Only upload files that have modifed in the past n minutes.
      parent: "Teams/Operational Excellence" # Optional parent page to nest content under.  Separate multiple parent pages with /.
      space: "TXM" # Confluence space under which content should be published.  Default TXM.
      title: "" # Set the page title on upload (defaults to filename without extension).
      use_document_title: true # Will use the Markdown document title (# Title) if available.  Default false.
      documents: "some-folder/some-doc(s)" # The documents to upload.  Can be a directory of documents or a single file.  Default is README.md at root of repo.
```
