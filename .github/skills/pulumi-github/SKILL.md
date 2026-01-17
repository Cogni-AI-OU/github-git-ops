<!-- markdownlint-disable MD003 MD022 MD026 MD041 -->

---
name: pulumi-github
description: Activate when managing GitHub resources with Pulumi CLI to import, preview, update, and repair state.
---

# Pulumi GitHub

Use Pulumi CLI to manage GitHub resources (repos, branches, teams) with fast imports, previews, and state fixes.

## Core Process

- Ensure token: classic PAT with `repo` + `security_events`,
  or fine-grained with repo Administration (read) and Contents (read); authorize for orgs/SSO.
- Set credentials: `export GITHUB_TOKEN=<pat>` and `pulumi config set --secret github:token <pat>`;
  set owner if needed: `pulumi config set github:owner <owner>`.
- Verify plugins: `pulumi plugin ls`; install if missing: `pulumi plugin install resource github <ver>`.
- Preview/apply non-interactively: `pulumi preview --non-interactive` then `pulumi up --yes --non-interactive`.
- Import existing repo: `pulumi import github:index/repository:Repository <name> <repo-name>`.
- Keep YAML in sync with import output to avoid drift (merge allowed fields only).

## Example Commands

```bash
# Show stack resources/URNs
pulumi stack --show-urns

# Import existing repository (non-interactive)
pulumi import github:index/repository:Repository repo_example example-repo --yes --non-interactive

# Refresh state without changes (non-interactive)
pulumi refresh --yes --non-interactive

# Detect drift safely
pulumi preview --non-interactive

# Remove an unwanted state entry (no delete in provider)
pulumi state unprotect <urn> --yes --non-interactive
pulumi state delete <urn> --force --yes --non-interactive

# Move resource between stacks
pulumi state move --source <src-stack> --dest <dst-stack> <urn>
```

## Tips

- For default branch settings, use `github_branch_default` instead of repository defaults warning.
- Always run in the stack directory (`-C <dir>` if scripted).
- After token changes, re-run `pulumi config set --secret github:token <pat>` to ensure the new token is used.

## Safety

- `pulumi state delete` is destructive to state; confirm URN and consider backup (`pulumi stack export`).
- Protect critical resources with `protect: true`; unprotect before intentional delete.
- Never commit secrets; rely on Pulumi config/stack secrets and env vars.
