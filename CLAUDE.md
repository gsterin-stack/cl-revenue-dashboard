# CL Revenue Dashboard — rules for code changes

## Which file to edit
- `index.html` is the live dashboard and the **source of truth for the code**. Edit it directly.
- `template.html` is an old version (no Weekly tab). Do not port changes there and do not regenerate `index.html` from it.

## Automated commits on `main`
Automations push to `main` several times a day and only touch data:
- `Update dashboard data`: replaces the single `const INLINE_DATA = {...};` line in `index.html` (plus the `build-time` meta).
- `Publish data.json snapshot`, `hubspot pipeline: …`, `weekly snapshot …`, `quarter history: …`: rewrite `data.json`, `hubspot_pipeline.json`, `weekly_snapshot.json`, `quarter_history.json`, `snapshots/`.

## Never overwrite an update (data or code)
1. **Never edit the `INLINE_DATA` line or the data JSON files** in a code change. A code commit must not show them in `git diff`.
2. **Start from fresh `main`**: `git fetch origin main` and branch/rebase from `origin/main` right before working, and again right before pushing.
3. **Deliver through git only** (merge or rebase, then push). Never upload a whole `index.html` (GitHub file API, copy/paste of an old version, `git checkout <old> -- index.html`): it silently reverts the latest data and any code merged in between.
4. **Before pushing, check** that `git diff origin/main -- index.html` only contains your code change, and that the `INLINE_DATA` line is identical to `origin/main`'s. If the push is rejected, fetch + rebase/merge again; never force-push `main`.
5. After a merge into `main`, confirm earlier code changes from other branches are still present (`git log origin/main` should still contain them).
