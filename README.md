# Provenr-skills

A collection of skills for Provenr


## Install

** Claude Code Plugin (recommended)**

From within Claude Code, first add the marketplace:
```
/plugin marketplace add forrestchang/andrej-karpathy-skills
```

Then install the plugin:
```
/plugin install andrej-karpathy-skills@karpathy-skills
```

This installs the guidelines as a Claude Code plugin, making the skill available across all your projects.

**Option B: CLAUDE.md (per-project)**

New project:
```bash
curl -o CLAUDE.md https://raw.githubusercontent.com/forrestchang/andrej-karpathy-skills/main/CLAUDE.md
```

Existing project (append):
```bash
echo "" >> CLAUDE.md
curl https://raw.githubusercontent.com/forrestchang/andrej-karpathy-skills/main/CLAUDE.md >> CLAUDE.md
```

**codex plugin marketplace**

From within Codex, first add the marketplace:
```
/plugin marketplace add provenr/provenr-skills
```

Then install the plugin:
```
/plugin install provenr-skills@provenr-skills
```

## Using with Cursor

This repository includes a committed Cursor project rule ([`.cursor/rules/karpathy-guidelines.mdc`](.cursor/rules/karpathy-guidelines.mdc)) so the same guidelines apply when you open the project in Cursor. See **[CURSOR.md](CURSOR.md)** for setup, using the rule in other projects, and how this relates to Claude Code.

## License

MIT
