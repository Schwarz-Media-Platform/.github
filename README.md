# .github

Repository defaults and common workflows/gh actions.

- ~settings.yml: configures the default settings for repositories https://github.com/probot/settings~ WIP to migrate to a [safer alternative](https://github.com/github/safe-settings)
- pull_request_template: is the default Pull Request template.
- .github/workflows: Reusable workflows:
  - Gitops Promotion


## Github Promotion

Use the following in your repository to enable gitops auto promotion. File must be `.github/workflows/promote.yaml`.

```yaml
name: GitOps PR generator

on:
  issue_comment:
    types: [created]

jobs:
  call-promotion-engine:
    if: |
      github.event.issue.pull_request && 
      (startsWith(github.event.comment.body, '/promote stage') || 
       startsWith(github.event.comment.body, '/promote prod'))

    uses: Schwarz-Media-Platform/.github/.github/workflows/shared-promotion.yml@main
    secrets:
      API_KEY: ${{ secrets.API_KEY }}
      GITOPS_PROMOTER_WEBHOOK_SECRET: ${{ secrets.GITOPS_PROMOTER_WEBHOOK_SECRET }}
```
