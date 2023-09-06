# setup-nexus-npm

A composite github action for creating a .npmrc file that will point to the npm repositories in nexus.  This allows developers to use Nexus to both access TXM internal Node modules as well as as a proxy for all Node dependencies.

## Parameters
- username: Required. The username for accessing the npm repositories in nexus, typically provided as a secret from the calling workflow
- password: Required. The password for accessing the npm repositories in nexus, typically provided as a secret from the calling workflow

## Example Usage

```yaml
- uses: actions/setup-node@v3
  with:
    node-version: 18
- name: setup-nexus-npm
  uses: sportsball-ai/actions-hub/setup-nexus-npm@<VERSION>
  with:
    username: ${{ secrets.USERNAME }}
    password: ${{ secrets.PASSWORD }}    
```
