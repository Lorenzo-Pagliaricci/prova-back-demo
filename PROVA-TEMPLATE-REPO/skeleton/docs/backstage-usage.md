# Backstage Usage

This service is automatically registered in Backstage by the template.

## What you can see in Backstage

- `Overview`: component metadata and key cards
- `GitLab`: repository data
- `CI/CD`: Jenkins job builds and status
- `Artifact`: Nexus repository information
- `Docs`: documentation linked through `backstage.io/techdocs-ref`

## How Docs tab is enabled

The template writes this annotation in `catalog-info.yaml`:

```yaml
metadata:
  annotations:
    backstage.io/techdocs-ref: dir:.
```

This tells Backstage to read docs from this repository (`mkdocs.yml` + `docs/`).

## Troubleshooting

- If docs are not visible:
  - check `catalog-info.yaml` annotation
  - check that `mkdocs.yml` exists
  - check that at least one page exists in `docs/`
  - verify Backstage can reach your GitLab repository
