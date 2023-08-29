# setup-nexus-npm

Configure a github action to point towards a self hosted Npm repository. After usage, any calls to `npm install` will point towards the repository at the endpoint provided.

## Parameters
- Endpoint: the npm registry endpoint
- Login: the login for the self hosted npm repository, typically stored as a secret
- Password: the password for the self hosted npm repository, typically stored as a secret

## Example Usage

```yaml
- uses: actions/setup-node@v3
  with:
    node-version: 14
- name: Set up setup-nexus-npm
  uses: sportsball-ai/actions-hub/setup-nexus-npm@<VERSION>
  with:
    endpoint: <URL>
    login: ${{ secrets.LOGIN }}
    password: ${{ secrets.PASSWORD }}    
```