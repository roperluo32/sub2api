# Custom deployment maintenance

This fork uses `custom/sig-ubuntu-02` as the production customization branch.

## Remotes

- `origin`: `https://github.com/roperluo32/sub2api.git`
- `upstream`: `https://github.com/Wei-Shaw/sub2api`

Keep local custom changes on `custom/sig-ubuntu-02`. Sync official changes with:

```bash
git fetch upstream
git checkout custom/sig-ubuntu-02
git merge upstream/main
```

Use `rebase upstream/main` only when the branch has not been shared with other
maintainers.

## CI/CD

`.github/workflows/custom-image-deploy.yml` builds and pushes:

- `ghcr.io/roperluo32/sub2api:custom-<short-sha>`
- `ghcr.io/roperluo32/sub2api:custom-sig-ubuntu-02`

The deployment job reads host and compose metadata from
`roperluo32/api-deploy-info`, then updates `/opt/sub2api/docker-compose.yml` and
`/opt/sub2api/.env.runtime` on the recorded host. The production `.env` remains
on the server and is not committed to Git.

## Required repository secrets

Configure these in `roperluo32/sub2api`:

- `DEPLOY_INFO_REPO_TOKEN`: read access to `roperluo32/api-deploy-info`.
- `DEPLOY_SSH_PRIVATE_KEY`: private key for the deploy user on the target host.
- `GHCR_READ_TOKEN`: optional; use when the target host cannot pull the image
  with the workflow `GITHUB_TOKEN`.

