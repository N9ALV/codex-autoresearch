# Experimental Research Workspace

This folder is the file-native research workspace for the thematic Research OS experiment.

Current purpose:

- prove that agents can write durable research state as ordinary Git files
- let Cloudflare publish a read-only view from `public/`
- keep research execution local / agent-driven
- preserve theme, company, portfolio, evidence, scenario, and next-action state in files that other agents can inspect

Status: experimental demo scaffold. Do not treat the included theme content as investment research, portfolio advice, or a completed underwriting packet.

## Current demo theme

- `themes/active/ai-infrastructure-power-demand/`

## File conventions

- Markdown: thesis, causal chain, disconfirming evidence, memos
- YAML: registries, scenarios, expression universe, linked companies, portfolio exposure
- JSONL: evidence log and review history
- CSV: exposure maps and portfolio ledgers

## Publishing convention

Cloudflare currently serves `public/`. The static `/research/` page is manually curated from this folder until a proper renderer exists.
