# major-version-tag

A composite action that takes a txm standard version number (vM.m.p) and returns the major version/revision (vM)

In the following example, in the "calculate-version" job, we specify the major version of oe-cli to use for versioning (v0).

In the "build_and_tag_new_image" job, we use the "major-version-tag" composite action to get the major version of the new, current version.

Then, when building the container image under the "tag and push image" step, we use the major version tag as a secondary tag on the image.

## Example Usage

```yaml
name: Update main Version Tag

on:
  pull_request:
    branches:
      - main
    types:
      - closed

jobs:
  calculate-version:
    if: github.event.pull_request.merged == true
    uses: sportsball-ai/actions-hub/.github/workflows/calculate-version.yml@v1
    with:
      # in the case of this calculate-version shared workflow, this is the major version of oe-cli to use for versioning
      # the major version tag can be used to get the latest major revision of any artifact that is versioned using this approach
      version: v0
    secrets: inherit

  git_version_tag:
      if: github.event.pull_request.merged == true
      needs: calculate-version
      runs-on: ubuntu-latest
      steps:
        - name: checkout repo
          uses: actions/checkout@v3
        
        - name: echo version
          run: |
            echo ${{ needs.calculate-version.outputs.version }}

        - name: git tag
          uses: sportsball-ai/actions-hub/git-version-tag@v1
          with:
            version: ${{ needs.calculate-version.outputs.version }}

  build_and_tag_new_image:
    if: github.event.pull_request.merged == true
    needs: calculate-version
    runs-on: ubuntu-latest
    steps:
      - name: checkout repo
        uses: actions/checkout@v3
      
      - name: docker build
        run:  docker build -t <your image> .     

        # this composite action will produce the major version tag for a given version
      - name: get major version tag
        id: major_version_tag
        uses: sportsball-ai/actions-hub/major-version-tag@v1
        with: 
          version: ${{ needs.calculate-version.outputs.version }}

      - name: GHCR Login
        uses: docker/login-action@v2
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

        # here we use the major version as a secondary tag on the docker image
        # because a docker tag can only be unique across all images, the next run of this workflow for a minor version bump
        # will produce the same major version tag which will move to the new version of the image that gets pushed to ghcr
        # a deprecation (major version bump) will produce a new major version tag which will then follow each revision of that major version
      - name: tag and push image
        run: |
          docker tag <your image> ghcr.io/sportsball-ai/<your image>:${{ needs.calculate-version.outputs.version }}
          docker tag <your image> ghcr.io/sportsball-ai/<your image>:${{ steps.major_version_tag.outputs.major_version }}
          docker push -a ghcr.io/sportsball-ai/<your image>

```

The above ensures that with each run of the workflow:
- the latest oe-cli docker image for the specified major version is used to handle versioning of the artifact being built in the workflow
- a new, TXM standard version is generated (vM.m.p) for the artifact being built/versioned in the workflow (in this case a docker image)
- any feature (i.e. a minor version bump) will produce the same major version tag via the "major-version-tag" composite action
- because docker tags must be unique, this same major version tag will follow/move to the latest major version of the artifact with each push to GHCR.
- a deprecation (i.e. a major version bump) will produce a new major version tag, which will continue to follow the latest revision for that major version
- and so on...

```
workflow run 1
    image v0.0.0 v0
workflow run 2 (minor version bump)
    image v0.1.0 v0
workflow run 3 (patch version bump)
    image v0.1.1 v0
workflow run 4 (deprecation/major version bump)
    image v1.0.0 v1
workflow run 5 (minor version bump)
    image v1.1.0 v1
```
The major version tag "follows" each latest revision of the same major version, thus allowing users to specify a major version tag and always get the "latest" compatible major version of a given artifact that is versioned in this way.