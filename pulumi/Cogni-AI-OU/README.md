# Pulumi GitHub Stack

Steps to manage the existing public repositories via Pulumi YAML.

## Prerequisites

- Pulumi CLI installed (v3.216.0+).
- GitHub token with access to repos
    with **Administration: Read** and **Contents: Read**.
  - Classic: Scopes `repo` and `security_events`.
- If org SSO is enforced, authorize the token for the org.

## Configure

```bash
export GITHUB_TOKEN=<token>
pulumi config set --secret github:token <token>
pulumi config set github:owner Cogni-AI-OU
```

## Import existing repositories

```bash
pulumi import github:index/repository:Repository repo_github_git_ops github-git-ops
pulumi import github:index/repository:Repository repo_example_ops_template example-ops-template
pulumi import github:index/repository:Repository repo_ansible_role_template ansible-role-template
```

## Apply (no changes expected after import)

```bash
pulumi up
pulumi stack --show-urns
```

## Troubleshooting

- 403 on `vulnerability-alerts`: token lacks **Administration (read)** or `security_events`.
- Verify token in use: `pulumi config get github:token --show-secrets` and `echo $GITHUB_TOKEN`.
