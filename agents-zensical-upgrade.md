This is a concise checklist for upgrading a Material for MkDocs site to Zensical.

It is intended as a repeatable guide for LLM-driven upgrades.

Feedback and improvements to this checklist are welcome. Please submit a PR if you have suggestions.

## Core Changes

1. Update dependencies by replacing `mkdocs-material` with `zensical` in `requirements.txt`.
2. Update build and serve commands by replacing `mkdocs serve` with `zensical serve` in local tooling (e.g. `docker-compose.yml`, docs) and replacing `mkdocs build` with `zensical build --clean` in CI (e.g. `.github/workflows/ALL-BRANCHES-ALL-PRs-build-and-deploy-to-azure.yml`).
3. Update `mkdocs.yml` to Zensical-compatible settings. Keep the file structure because Zensical is drop-in. If you use Material emoji extensions, switch the module paths to:
   `emoji_index: !!python/name:zensical.extensions.emoji.twemoji`
   `emoji_generator: !!python/name:zensical.extensions.emoji.to_svg`
4. Remove `theme: material` from `mkdocs.yml`.
5. The default theme variant is `modern`. If you want Material-like styling, set
   ```yml
   theme:
     variant: classic
   ```
6. Update documentation references to say Zensical instead of MkDocs/Material, and update any `mkdocs` CLI instructions. Update `spec.md` or architecture docs to reflect Zensical use.

## Validation

1. Install deps:

```bash
pip install -r requirements.txt
```

2. Build locally:

```bash
zensical build --clean
```

3. (Optional) Serve locally:

```bash
zensical serve
```

## Known Gotchas

1. Snippets auto-append: If you use `pymdownx.snippets` with `auto_append`, ensure paths resolve relative to the project root. If you see `SnippetMissingError`, add `base_path: .` or use an absolute path in `auto_append`.
2. CI/CD: Ensure your CI uses Zensical in the build step.
3. Docs drift: Update all docs that mention MkDocs CLI commands or Material-specific wording.

## Files that should be reviewed/updated

1. `requirements.txt` (added `zensical`)
2. `mkdocs.yml` (emoji path updates, theme variant, palette)
3. `docker-compose.yml` (zensical serve command)
4. `.github/workflows` may need review
5. `docs/*` - meta-references to the docs setup within the docs itself may need review/update
6. `spec.md` (architecture/tooling references)
7. `Dockerfile` (comments or tooling references to MkDocs/Material)
8. `s/*` (dev helper scripts that mention MkDocs)
9. `README.md` (user-facing setup and CLI instructions)

## General tidying

- Check that the Copyright notice in `mkdocs.yml` is up to date and includes the current year and correct organization name.
- Check that the version of Python in the `Dockerfile` is still appropriate and update if necessary.
