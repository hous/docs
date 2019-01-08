---
title: Import/Export Tenant Configuration to Directory Structure
description: Understand how the Auth0 Deploy Command Line Interface (CLI) tool works.
topics:
  - extensions
  - deploy-cli
contentType:
  - how-to
useCase: extensibility-extensions
---
# Import/Export Tenant Configuration to Directory Structure

The `auth0-deploy-cli` tool directory option supports exporting and importing the Auth0 tenant configuration into a predefined directory structure.

For information on how the files are expected to be laid out to work with the source control configuration utilities, see [Github Deployments](/extensions/github-deploy).

## Import tenant configuration

1. Copy `config.json.example` and fill out details.

   ```json
   {
     "AUTH0_DOMAIN": "<YOUR_TENANT>.auth0.com",
     "AUTH0_CLIENT_ID": "<client_id>",
     "AUTH0_CLIENT_SECRET": "<client_secret>",
     "AUTH0_KEYWORD_REPLACE_MAPPINGS": {
       "AUTH0_TENANT_NAME": "<NAME>",
       "ENV": "DEV"
     },
     "AUTH0_ALLOW_DELETE": false,
     "AUTH0_EXCLUDED_RULES": [
       "rule-1-name",
       "rule-2-name"
     ]
   }
   ```

   Use the `client ID` and secret from your newly created client (the client is named `auth0-deploy-cli-extension` if you used the extension).

   By default the tool merges your current environment variables and overrides `config.json` which has the same top key. Use the `--no-env` option to disable the override via the command line.

   You can either set env variables or place the values in a config file anywhere on the file system.

2. Run deploy.

   ```bash
   a0deploy import -c config.json -i .
   ```

### Config file example

Here is an example of a `config.json` file:

```json
{
  "AUTH0_DOMAIN": "<your auth0 domain (e.g. fabrikam-dev.auth0.com) >",
  "AUTH0_CLIENT_SECRET": "<your deploy client secret>",
  "AUTH0_CLIENT_ID": "<your deploy client ID>",
  "AUTH0_KEYWORD_REPLACE_MAPPINGS": {
    "YOUR_ARRAY_KEY": [
      "http://localhost:8080",
      "https://somedomain.com"
    ],
    "YOUR_STRING_KEY": "some environment specific string"
  },
  "AUTH0_ALLOW_DELETE": false,
  "AUTH0_EXCLUDED_RULES": [
    "rule-1-name",
    "rule-2-name"
  ]
}
```

## Export tenant configuration

To export your current tenant configuration, use a command like the following example:

`a0deploy export -c config.json --strip -f directory -o path/to/export`

<%= include('../_includes/_strip-option') %>

<%= include('../_includes/_limitations') %>

For more information, see [Environment Variables and Keyword Mappings](/extensions/deploy-cli/references/environment-variables-keyword-mappings).

### Directory structure example

Here is an example of a export directory structure:

```
repository =>
  clients
    client1.json
    client2.json
  connections
    connection1.json
  database-connections
    connection1
      database.json
      create.js
      delete.js
      get_user.js
      login.js
      verify.js
  emails
    provider.json
    verify_email.json
    verify_email.html
    welcome_email.json
    welcome_email.html
  grants
    grant1.json
  pages
    login.html
    login.json
    password_reset.html
    password_reset.json
  resource-servers
    resource_server1.json
    resource_server2.json
  rules
    rule1.js
    rule1.json
    rule2.js
  rules-configs
    env_param1.json
    some_secret1.json
  guardian
    factors
      sms.json
      email.json
      otp.json
      push-notification.json
    provider
      sms-twilio.json
    templates
      sms.json
```

## Keep reading

* [Incorporate Deploy CLI into Build Environment](/extensions/deploy-cli/guides/incorporate-deploy-cli-into-build-environment)
* [Call Deploy CLI Tool Programmatically](/extensions/deploy-cli/guides/call-deploy-cli-programmatically)
* [Deploy CLI Tool Options](/extensions/deploy-cli/references/deploy-cli-options)
* [Import/Export Tenant Configuration to a YAML File](/extensions/deploy-cli/guides/import-export-yaml-file)