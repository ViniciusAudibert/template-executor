# Template Executor

Warn your team whats is going on with your main product.

![Project MMD](https://github.com/ViniciusAudibert/template-executor/blob/main/.github/project.svg?raw=true)

> To update the image above, change project.mmd and run "yarn update-project-mmd"

## template.yaml

```
id: team-cart-web
executions:
  - description: Count of abandoned carts exceeded 100 records
    provider:
      id: sql
      connection: corporative-connection-string # Preset in Core Repository secrets
    stage:
      queries:
        - "Select count(*) as count
          from Person p
          inner join Cart c ON c.idPerson = p.id
          where p.blacklist = false
          and c.checkout = false
          and c.lastUpdate > DATE_SUB(NOW(), INTERVAL 1 DAY);"

    actions:
      - provider:
          id: teams
          token: my-favourite-team-token # Preset in Core Repository secrets
        schedule: 0 * * * * # https://crontab.guru
        conditions: # Javascript conditions
          - "query1[0].count > 100"

      - provider:
          id: slack
          token: my-favourite-slack-token # Preset in Core Repository secrets
        executeNow: true
        webhookId: some-id # when called it will execute again this action
        conditions: # Javascript conditions
          - "query1[0].count > 100"

```
