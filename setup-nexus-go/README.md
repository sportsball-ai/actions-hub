# setup-nexus-go

Configure a github action to point towards a self hosted Go repository. After usage, any calls to `go get` will point towards the repository at the endpoint provided.

## Parameters
- Endpoint: the endpoint to use for GOPROXY
- Login: the login for the self hosted Go repository, typically stored as a secret
- Password: the password for the self hosted Go repository, typically stored as a secret

## Example Usage

```yaml
- uses: actions/setup-go@v3
  with:
    go-version: '^1.13.1'
- name: Set up setup-nexus-go
  uses: sportsball-ai/actions-hub/setup-nexus-go@<VERSION>
  with:
    endpoint: <URL>
    login: ${{ secrets.LOGIN }}
    password: ${{ secrets.PASSWORD }}
```