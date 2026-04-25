# Cloudflare publishing handoff — 2026-04-25

## Summary

A minimal no-build Cloudflare publishing surface has been added to the repository so the Git-synced Cloudflare service can deploy successfully without a local install or build step.

## What was added

- `wrangler.jsonc` at the repo root, configured for Workers static assets.
- `public/index.html` as the root landing page.
- `public/research-os/index.html` as a rendered Research OS publishing guide.
- `public/404.html` and `public/styles.css` for basic static-site support.
- Research OS docs updated to describe the temporary no-build publishing path.

## Live deployment

- Live URL: https://codex-autoresearch.a-b21.workers.dev
- Research OS URL: https://codex-autoresearch.a-b21.workers.dev/research-os/
- Cloudflare account id: `b21a0d5780d3f958123b2d964e58b015`
- Latest successful deploy observed during setup: version `f00cdadb-c73d-433c-9691-9dcc9c40f8a0`

## Relevant repository history

- Main branch deploy/config commit already pushed: `fcad7a0` — `Add no-build Cloudflare publishing surface`

## Why this was needed

The existing Cloudflare Git deployment was running `npx wrangler deploy`, but the repository had no configured static asset directory. Wrangler therefore failed with:

> Could not detect a directory containing static files (e.g. html, css and js) for the project

The new root `wrangler.jsonc` fixes that by explicitly pointing Cloudflare to `./public`.

## Current architecture position

This should be treated as a temporary publishing layer only:

- GitHub remains the source of truth.
- Cloudflare is publishing read-only output.
- Research execution remains local / agent-driven.
- A fuller renderer can later replace this placeholder path and publish from `dist` or another generated output directory.

## Recommended next steps

1. Confirm the Git-connected Cloudflare service auto-redeploys cleanly from `main` after the pushed config change.
2. Decide whether `workers.dev` and preview URLs should remain enabled by default or be made explicit in `wrangler.jsonc`.
3. Replace the placeholder `public/` surface with the proper research-site renderer when that exists.
4. Optionally place Cloudflare Access in front of the published site if the research output should be private.

## Operator notes

- Standard Cloudflare credential bootstrap used: `Cloudflare Management/ops/cloudflare-access-all.ps1 -Persist`
- No local build or package install was performed for this setup.
- This repo was kept isolated under its own folder inside the monorepo.
