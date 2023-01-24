# actions-hub

Actions hub will serve as a store for sportsball-ai's shared workflows and composite actions. This allows developers to create commonly used generic github actions to be used throughout the organization. This also makes it easy for developers to adopt custom tooling built within the organization within their CI pipelines. 

It is important to note that this repository is `public`, due to limitations on Github's sharing of actions resources between private repositories. In the future, if sportsball-ai is upgraded to an enterprise account, it may be possible to mark this repository as `internal` and follow an innersource approach. 

However, since this repository is public, it is important to not expose information that could reveal the form of the organization's infrastructure or any sensitive values. This can be accomplished by parameterizing all composite actions and limiting discussion about infrastructure in PR's.

## Developing

Create new composite actions in the following location. `<name>/action.yml`

### Example

```yml
name: version
inputs:
  foo:
    required: true
runs:
  using: composite
  steps:
    - run: echo ${{ inputs.foo }}
      shell: bash
```

## Usage

Each composite action added must contain a README `<name>/README.md` with documentation and examples about how to use that particular action.  See existing actions for examples of their associated README.

## Versioning

Whenever a shared workflow or composite action from this repo is called from within your workflow, it must have a tag reference so it can be properly identified, e.g. `uses: sportsball-ai/actions-hub/git-version-tag@main`

While it is possible to just use `@main` to get the latest head of the repo on the main branch, main/HEAD is currently tagged as "v2".  This is so that we can identify when a breaking change is introduced and thus introduce new functionality without breaking existing external references to this repo (i.e. v1).

**Note**
If you are contributing to this repo, please consider whether you are introducing a breaking change or not, i.e is this a feature, patch or deprecation. 

Once your PR is approved and merged:
- for a non-breaking change, please move the v2 tag (or whatever tag represents the most current version) to the new commit on main/HEAD by removing the previous vN tag and recreating it on the current commit at main/HEAD.  Then push the tag to github.

- if you are introducing a deprecation, i.e. a removal of previous functionality, then please create the next logical vN tag on the current main/HEAD and push that new tag to github.