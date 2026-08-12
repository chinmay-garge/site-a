# site-a

Sandbox site repo for store `site-a-store`. Part of the multi-site theme CI/CD
sandbox — the shared code lives in
[`shopify-common`](https://github.com/chinmay-garge/shopify-common).

## What this repo is for

It is git-connected to this store's **Staging** theme, so it mirrors **content**:
anything edited in the Shopify theme editor arrives here as a commit.

It is **not** where code changes are made. `.liquid` files here are overwritten
by the next deploy from `shopify-common`. To change code, change it there.

| Lane | Owner | Path |
|---|---|---|
| CODE | `shopify-common` | `sections/`, `snippets/`, `layout/`, `assets/*.vbt.*` |
| CONTENT | editors, in the Shopify admin | `templates/*.json`, `config/settings_data.json`, `locales/*.json` |

## Drift detection

`.github/workflows/drift-detection.yml` runs on every push to `main`, diffs the
code files here against `shopify-common/theme`, and maintains a single open
`drift` issue — updating it while drift persists and closing it once resolved.

Requires:

- variable `TARGET_REMOTE_REPO` = `chinmay-garge/shopify-common`
- secret `ACCESS_PAT` — PAT with `repo` scope, to read the shared repo

## Fixtures owned by this site

`sections/sandbox-table.liquid` and `snippets/sandbox-table-cell.liquid` exist
**only here**. They are the rehearsal for porting a site-unique section into the
shared theme, and they are deliberately awkward:

- the section will not render without its snippet, and
- its schema uses `t:` keys absent from the shared repo's locale file,

so porting it carelessly reproduces the exact missing-translation bug that
reached a real store. See `docs/TEST-PLAN.md` in `shopify-common` (F1).
