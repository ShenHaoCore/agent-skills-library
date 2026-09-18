# GitHub Actions Hardening

## Security

- Prefer OIDC over long-lived cloud keys
- Restrict `pull_request_target` — it runs with base secrets; easy to misuse
- Never checkout untrusted PR code and then run it with privileged secrets in the same trust boundary without isolation
- Pin actions by SHA for high-security repos

## Performance

| Technique | Note |
| :--- | :--- |
| Path filters | `paths:` / `paths-ignore:` |
| Cache | Hash lockfiles |
| Shard tests | Split slow suites |
| Larger runners | Only when CPU-bound |

## Debugging

- Re-run failed jobs with logging
- Upload artifacts on failure (`actions/upload-artifact`)
- Keep scripts in repo (`scripts/ci/*`) rather than huge inline YAML

## CD tips

- Deploy from tags or approved environments
- Separate `workflow_dispatch` for hotfix
- Health-check after deploy; auto rollback if you own the mechanism
