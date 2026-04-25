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

## Cloudflare publishing path

The short-term Cloudflare role is read-only publishing from GitHub. Do not move the research runner to Cloudflare until there is a clear need for hosted execution.

Recommended setup:

```text
GitHub repo
  -> Cloudflare Pages Git integration
  -> production branch: main
  -> preview deployments: pull requests / non-production branches
  -> optional Cloudflare Access in front of the site
```

### 1. Keep GitHub as the source of truth

Agents should write research outputs to ordinary repository files:

```text
content or research workspace files:
  themes/**/*.md
  themes/**/*.yaml
  themes/**/*.jsonl
  companies/**/*.md
  portfolios/**/*.csv
  rubrics/**/*.yaml
  schemas/**/*.json
```

Do not give the published Cloudflare site client-facing write/edit access. Writes should happen through Git branches, pull requests, or reviewed agent commits.

### 2. Connect Cloudflare Pages to GitHub

In Cloudflare Pages:

1. Create a Pages project.
2. Connect the GitHub repository.
3. Set the production branch to `main`.
4. Use the project build command once a static renderer exists.
5. Set the output directory to the renderer's static output directory.

Early scaffold mode can use a placeholder static site while the renderer is being built. Once the static renderer exists, the usual shape should be:

```bash
npm ci
npm run build:research-site
```

Output directory example:

```text
dist
```

### 3. Use previews for research review

Use branch or pull-request previews for draft research:

```text
agent/theme-ai-power-demand
  -> Cloudflare preview deployment
  -> review research packet as a read-only site
  -> merge to main
  -> production site updates
```

This preserves the Git review trail and keeps the published surface read-only.

### 4. Add Cloudflare Access if research is private

If the research should not be public, put Cloudflare Access in front of the Pages site.

Recommended policy:

```text
GitHub: private or controlled write access for agents/operators
Cloudflare Pages: read-only deployment
Cloudflare Access: identity gate for readers
```

### 5. Keep heavy artifacts out of Git

Normal research outputs are small text files and are safe for Git. Large raw artifacts should be stored outside Git and referenced by manifest:

```text
Good in Git:
  memos, scenarios, metrics, evidence logs, rubrics, schemas, exposure maps

Use object storage later:
  full PDF filings, transcript archives, large datasets, generated PDF books, screenshots, media
```

If Cloudflare R2 is added later, research files should reference R2 objects by key, label, source, and checksum rather than embedding large blobs in the repo.

### 6. Defer advanced Cloudflare services

Do not start with Workers, D1, Vectorize, Queues, Workflows, or Containers unless the requirement changes.

Add them only when needed:

- Workers: light API, redirects, search, signed artifact access.
- R2: large source artifacts or generated packet exports.
- D1: dynamic index/state that cannot be represented cleanly as files.
- Vectorize: hosted semantic search across published research.
- Workflows/Queues/Containers: hosted research execution.

For the current goal, the clean architecture is:

```text
local agents do research
GitHub stores research memory
Cloudflare Pages publishes read-only output
Cloudflare Access optionally gates readers
```
