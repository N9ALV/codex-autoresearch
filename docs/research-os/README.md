# Thematic Research OS Scaffold

This repository can be used as a Git-native research workspace and a read-only publishing source. The intended short-term shape is:

```text
local / external agents
  -> write Markdown, YAML, JSON, JSONL, and CSV research outputs
  -> commit those outputs to GitHub branches
  -> review diffs / pull requests
  -> publish the read-only version through Cloudflare Pages
```

Cloudflare should initially be a publishing layer, not the research execution layer. The local agent remains responsible for source gathering, scenario work, evidence synthesis, and packet generation.

## Research doctrine

- Themes are the organizing layer.
- Companies are expressions, beneficiaries, victims, hedges, or narrative-only exposures.
- Portfolios are bundles of theme exposures.
- Scenario analysis tests causal chains.
- Weight-and-rate scoring diagnoses evidence and expression quality.
- `quality_gap` / `research_gap` closes accepted evidence and packet gaps; it does not make uncertainty disappear.
- Outputs are research actions and review queues, not automated trade instructions.

## Recommended repo layout

```text
themes/
  registry.yaml
  collision-map.yaml
  active/<theme-id>/
    thesis.md
    causal-chain.md
    scenarios.yaml
    evidence-log.jsonl
    exposure-map.csv
    expression-universe.yaml
    linked-companies.yaml
    portfolio-exposure.yaml
    disconfirming-evidence.md
    review-history.jsonl

companies/
  registry.yaml
  reviews/<company-id>/
  theme-expression-scores/<company-id>.yaml

portfolios/
  holdings.csv
  theme-exposure-ledger.csv
  reunderwriting-queue.md

rubrics/
  theme-validity.yaml
  theme-investability.yaml
  company-expression-quality.yaml
  basket-expression-quality.yaml
  portfolio-theme-exposure.yaml

schemas/
  theme.schema.json
  scenario.schema.json
  metrics.schema.json
  evidence.schema.json

outputs/
  theme-memos/
  company-memos/
  portfolio-reviews/
```

The `plugins/codex-autoresearch/assets/research-os` folder contains starter templates that agents can copy into a working repository.

## First useful loop

1. Copy the scaffold templates into a fresh research workspace.
2. Create `autoresearch.research/theme-research/quality-gaps.md` from `assets/research-os/templates/quality-gaps-theme-research.md`.
3. Run setup with the external recipe catalog:

```bash
node plugins/codex-autoresearch/scripts/autoresearch.mjs setup --cwd <workspace> --catalog plugins/codex-autoresearch/assets/research-os/recipes.json --recipe theme-research-gap
```

4. Let agents update the theme files and log ASI-backed decisions.
5. Publish the read-only site from the GitHub repo through Cloudflare Pages once the file shape is stable.
