# Update Guide

## How to update documentation

1. Edit Markdown files under `docs/`.
2. If needed, update navigation in `mkdocs.yml`.
3. Commit and push changes to `main`.

Example:

```bash
git add docs/ mkdocs.yml
git commit -m "docs: update service documentation"
git push origin main
```

## Recommended structure

- `docs/index.md`: service overview
- `docs/api.md`: API details and examples
- `docs/runbook.md`: operational procedures
- `docs/adr.md`: architecture and key decisions

## Documentation quality checklist

- Owner and contacts are clearly stated
- API behavior is documented
- Dependencies and external systems are listed
- Deployment and rollback notes are present
- Last update date is visible
