# docker-base-images

Base Docker images built by GitHub Actions and pushed to GitHub Container Registry
(`ghcr.io/<owner>/<repo>/...`).

## Images

| Image | Source | Notes |
| --- | --- | --- |
| `ruby2.6-prod` | `ruby/2.6/Dockerfile.prod` | Production Ruby 2.6 base |
| `ruby2.6` | `ruby/2.6/Dockerfile.test` | Test variant, `FROM` the prod image |
| `ruby2.7-prod` | `ruby/2.7/Dockerfile.prod` | Production Ruby 2.7 base |
| `ruby2.7` | `ruby/2.7/Dockerfile.test` | Test variant, `FROM` the prod image |
| `tools/postgres-client` | `tools/postgres-client/Dockerfile` | Alpine + `postgresql-client` |

Pull example:

```sh
docker pull ghcr.io/<owner>/<repo>/ruby2.7-prod:latest
```

## Tagging

Tags are emitted by [`docker/metadata-action`](https://github.com/docker/metadata-action):

- **Branch push** → `<branch-slug>`, plus `latest` on the default branch
- **Tag push** matching `v*.*.*` → `1.2.3`, `1.2`, `1`, `latest`
- **Pull request** → `pr-<N>`
- **Every build** → `sha-<full-sha>` (used internally to chain the test image's `FROM`
  to the prod image built in the same run)

## Releases

Push an annotated semver tag:

```sh
git tag -a v1.2.3 -m "release 1.2.3"
git push origin v1.2.3
```

## Security scanning

Each image is scanned by [Trivy](https://github.com/aquasecurity/trivy-action) after
build. CRITICAL/HIGH findings (excluding unfixed) are uploaded as SARIF to the
repository's Security tab.
