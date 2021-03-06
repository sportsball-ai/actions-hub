# actions-hub

Actions hub will serve as a store for sportsball-ai's shared composite actions. This allows developers to create commonly used generic github actions to be used throughout the organization. This also makes it easy for developers to adopt custom tooling built within the organization within their CI pipelines. 

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

Each composite action will contain a README `<name>/README.md` with documentation and examples about how to use that particular action.

### Example

```yml
name: oe-cli

on:
  push:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Set up oe-cli
        uses: sportsball-ai/actions-hub/oe-cli@main
        with:
          foo: oe-cli-says-hello-world!
      - name: Version using oe-cli
        uses: sportsball-ai/actions-hub/version@main
        with:
          foo: oe-cli-version!
      - name: Do some linting
        run: |
          echo lint
      - name: Do some testing
        run: |
          echo test
```

