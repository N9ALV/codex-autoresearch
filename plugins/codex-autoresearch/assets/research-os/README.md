# Research OS Asset Pack

These files are starter assets for a GitHub-native thematic research workspace. Copy them into the root of the workspace that Codex Autoresearch will operate on.

Suggested command from the plugin root:

```bash
cp -R assets/research-os/{schemas,rubrics,packets,templates} <workspace>/
```

Then create or update `autoresearch.research/<slug>/quality-gaps.md` and run a quality-gap recipe from `assets/research-os/recipes.json`.

```bash
node scripts/autoresearch.mjs setup --cwd <workspace> --catalog assets/research-os/recipes.json --recipe theme-research-gap
```
