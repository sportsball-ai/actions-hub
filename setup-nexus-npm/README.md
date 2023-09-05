# setup-nexus-npm

A composite github action for creating a .npmrc file that will point to the npm repositories in nexus

## Parameters
- username: the username for accessing the npm repositories in nexus, typically provided as a secret from the calling workflow
- password: the password for accessing the npm repositories in nexus, typically provided as a secret from the calling workflow

## Example Usage

```yaml
- uses: actions/setup-node@v3
  with:
    node-version: 16
- name: setup-nexus-npm
  uses: sportsball-ai/actions-hub/setup-nexus-npm@<VERSION>
  with:
    username: ${{ secrets.USERNAME }}
    password: ${{ secrets.PASSWORD }}    
```