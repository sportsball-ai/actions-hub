# setup-nexus-npm

Configure a github action to point towards the nexus npm and npm-txm repositories. After usage, any calls to `npm install` will point towards the repository at the endpoint provided.

## Parameters
- Endpoint: the npm registry endpoint.  defaults to nexus.txm.tools
- Login: the username for the self hosted npm repository, typically stored as a secret, this defaults to the NEXUS_USER_CI_NPM secret
- Password: the password for the self hosted npm repository, typically stored as a secret, this defaults to the NEXUS_PASSWORD_CI_NPM secret

## Example Usage

```yaml
- uses: actions/setup-node@v3
  with:
    node-version: 14
- name: setup-nexus-npm
  uses: sportsball-ai/actions-hub/setup-nexus-npm@<VERSION>
  with:
    endpoint: <URL> (optional - defaults to nexus.txm.tools)
    username: ${{ secrets.LOGIN }} (optional - has default for nexus)
    password: ${{ secrets.PASSWORD }} (optional - has default for nexus)    
```