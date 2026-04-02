# save-project-assets

> **Proactively save all project knowledge at commit time or on demand.**  
> **在提交代码时或按需主动保存所有项目知识。**

A [Claude Code](https://claude.ai/code) skill that captures and persists everything your AI assistant learns during a session:

- 📋 **TECH_LOG** — engineering lessons & root-cause analyses  
- 📝 **CHANGELOG** — feature status & fix records  
- 💬 **GitHub Issues** — auto-comment confirmed fixes  
- 💡 **IDEAS** — future work & improvement notes  
- 🧠 **Memory** — persistent cross-session context  

Bilingual: **中文 (zh)** + **English (en)** + **auto-detect (default)**

---

## Quick Install / 快速安装

### Option A — Plugin (one command)

Add to your `~/.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "save-project-assets": {
      "source": {
        "source": "github",
        "repo": "mosjin/save-project-assets"
      }
    }
  },
  "enabledPlugins": {
    "save-project-assets@save-project-assets": true
  }
}
```

Then run `/reload-plugins` in Claude Code.

### Option B — Manual copy

Copy the skill file(s) you want into `~/.claude/skills/`:

```bash
# Default (bilingual, auto-detect)
mkdir -p ~/.claude/skills/save-project-assets
curl -o ~/.claude/skills/save-project-assets/SKILL.md \
  https://raw.githubusercontent.com/mosjin/save-project-assets/main/.claude/skills/save-project-assets/SKILL.md

# Chinese only / 仅中文
mkdir -p ~/.claude/skills/save-project-assets-zh
curl -o ~/.claude/skills/save-project-assets-zh/SKILL.md \
  https://raw.githubusercontent.com/mosjin/save-project-assets/main/.claude/skills/save-project-assets-zh/SKILL.md

# English only
mkdir -p ~/.claude/skills/save-project-assets-en
curl -o ~/.claude/skills/save-project-assets-en/SKILL.md \
  https://raw.githubusercontent.com/mosjin/save-project-assets/main/.claude/skills/save-project-assets-en/SKILL.md
```

---

## Usage / 使用方式

### Manual invocation

```
/save-project-assets        # auto-detect language
/save-project-assets-zh     # Chinese / 中文
/save-project-assets-en     # English
```

### Auto-trigger (no command needed)

The skill fires automatically when:

- You say: **save · record · assets · update docs · update memory**  
- 你说：**保存 · 记录 · 保存资产 · 更新文档 · 更新记忆**

### Optional: Hooks for commit-time reminders

Copy `hooks/settings-example.json` entries into your `~/.claude/settings.json` to get:

- **SessionStart hook** — injects a reminder into Claude's context every session
- **PreToolUse hook** — reminds Claude to save before `git commit`

---

## What It Does (7 Steps) / 七步工作流

| Step | Action |
|------|--------|
| 1 | Harvest session knowledge |
| 2 | Update `docs/TECH_LOG.md` with lessons |
| 3 | Update `docs/CHANGELOG.md` with feature status |
| 4 | Comment on confirmed-fixed GitHub issues |
| 5 | Update `docs/IDEAS.md` with new ideas |
| 6 | Update Claude Code auto-memory (MEMORY.md) |
| 7 | `git add docs/ && git commit && git push` |

---

## Configuration / 配置

Open the SKILL.md you installed and fill in the CONFIG block at the top:

```markdown
<!-- CONFIG
  GITHUB_REPO  = owner/repo       e.g. myorg/myrepo
  DOCS_DIR     = docs/
  BRANCH       = main
-->
```

That's it. The memory path is auto-detected by Claude Code.

---

## Directory Structure / 目录结构

```
save-project-assets/
├── .claude-plugin/
│   ├── plugin.json          # Plugin metadata
│   └── marketplace.json     # Self-hosted marketplace manifest
├── .claude/
│   └── skills/
│       ├── save-project-assets/       # Default (bilingual)
│       ├── save-project-assets-zh/    # Chinese / 中文
│       └── save-project-assets-en/    # English
├── hooks/
│   └── settings-example.json  # Example hook configuration
└── README.md
```

---

## Requirements / 前置条件

- [Claude Code](https://claude.ai/code) CLI
- `gh` CLI (for GitHub issue comments, optional)
- `git` (for the commit step)
- Your project should have a `docs/` folder (or adjust paths in SKILL.md)

---

## License

MIT — free to use, fork, and adapt.

---

*Built from real-world usage on [DiskCleaner](https://github.com/mosjin/DiskCleanerSimple) — a cross-platform disk cleaning tool.*
