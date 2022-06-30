# setup-nexus-python

Configure a github action to point towards a self hosted Python repository. After usage, any calls to `pip3 install` will point towards the repostiroy at the endpoint provided.

## Parameters
- Endpoint: the python registry endpoint
- Login: the login for the self hosted python repository, typically stored as a secret
- Password: the password for the self hosted python repository, typically stored as a secret

## Example Usage
```yaml
- uses: actions/setup-python@v4
  with:
    python-version: '3.x'
- name: Set up setup-nexus-python
  uses: sportsball-ai/actions-hub/setup-nexus-python@<VERSION>
  with:
    endpoint: <URL>
    login: ${{ secrets.LOGIN }}
    password: ${{ secrets.PASSWORD }}
```