# Custom Action to Clean Up Redundant Pull Requests

The GlueOps platform can optionally use automation to create pull requests to add an approval step before the platform Continuous Deployment automation is triggered.  A potential side effect of this automation is the creation of redundant pull requests to deploy new versions of a given application.

For example, if three releases are created to deploy to a Staging environment, but only the latest release is approved for a Production deployment, the approver will experience a pull request for each release and would only approve the latest release for production.  Then, manual cleanup would be required to close out dated pull requests and delete unneeded branches.

This action addresses the clutter of redundant pull requests by closing old PRs and deleting their associated branches.

The action is designed to be triggered by opening a pull request.

## Input Values

* `pr_number`: The number of the pull request triggering the action.  Retrieved from the triggering event
* `gh_token`: The default github token, as this action leverages the [github_cli](https://github.com/cli/cli).

## Usage

Designed to be implemented alongside the [GlueOps CD Actions](https://github.com/GlueOps/github-workflows/blob/main/.github/workflows/argocd-tags-ci.yml).

### Output Values

None.

### Example Configuration

```yaml
name: Trigger GlueOps/github-actions-cleanup-redundant-cd-prs

on:
  pull_request:
    types: [opened, reopened, synchronize]

jobs:
  call-close-old-prs-workflow:
    runs-on: ubuntu-latest
    steps:
      - name: Run Cleanup
        uses: GlueOps/github-actions-cleanup-redundant-cd-prs@v0.1.4
        with:
          pr_number: ${{ github.event.pull_request.number }}
          gh_token: ${{ secrets.GITHUB_TOKEN }}
```
