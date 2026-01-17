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

## References

- Pulumi CLI download/install: [Pulumi download & install][pulumi-install]
- Pulumi GitHub provider install/config: [Pulumi GitHub provider install/config][pulumi-github-provider]
- Pulumi GitHub YAML template: [Pulumi GitHub YAML template][pulumi-template]
- GitHub REST: check if vulnerability alerts are enabled: [GitHub REST vulnerability alerts][gh-vuln-alerts]
- GitHub REST: fine-grained PAT required permissions: [GitHub PAT permissions][gh-pat-perms]

<!-- Named links -->

[pulumi-install]: https://www.pulumi.com/docs/get-started/download-install/
[pulumi-github-provider]: https://www.pulumi.com/registry/packages/github/installation-configuration/
[pulumi-template]: https://github.com/pulumi/templates/blob/master/github-yaml/Pulumi.yaml
[gh-vuln-alerts]: https://docs.github.com/en/rest/repos/repos?apiVersion=2022-11-28#check-if-vulnerability-alerts-are-enabled-for-a-repository
[gh-pat-perms]: https://docs.github.com/en/rest/authentication/permissions-required-for-fine-grained-personal-access-tokens
