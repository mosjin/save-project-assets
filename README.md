# save-project-assets

> **Proactively save all project knowledge at commit time or on demand.**  
> **在提交代码时或按需主动保存所有项目知识。**

A [Claude Code](https://claude.ai/code) skill — captures and persists everything learned during a session:

- 📋 **TECH_LOG** — engineering lessons & root-cause analyses  
- 📝 **CHANGELOG** — feature status & fix records  
- 💬 **GitHub Issues** — auto-comment confirmed fixes  
- 💡 **IDEAS** — future work & improvement notes  
- 🧠 **Memory** — persistent cross-session context  

Bilingual: **中文 (zh)** + **English (en)** + **auto-detect (default)**

---

## Install / 安装

### ✅ Easiest: one command + `/reload-plugins` (2 steps)

```bash
# clone once, run once
git clone https://github.com/mosjin/save-project-assets
cd save-project-assets
python install.py
```

Then inside Claude Code: **`/reload-plugins`** — done. ✓

To uninstall: `python install.py --remove` + `/reload-plugins`

---

### Option B: manual JSON (for power users)

Add to `~/.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "save-project-assets": {
      "source": { "source": "github", "repo": "mosjin/save-project-assets" }
    }
  },
  "enabledPlugins": {
    "save-project-assets@save-project-assets": true
  }
}
```

Then `/reload-plugins`.

---

### Option C: copy one file (no plugin system)

```bash
mkdir -p ~/.claude/skills/save-project-assets
curl -o ~/.claude/skills/save-project-assets/SKILL.md \
  https://raw.githubusercontent.com/mosjin/save-project-assets/main/.claude/skills/save-project-assets/SKILL.md
```

No reload needed — available immediately.

---

## Usage / 使用

```
/save-project-assets        # auto language / 自动语言
/save-project-assets-zh     # 中文
/save-project-assets-en     # English
```

**Auto-trigger** (no command needed) when you say:  
`save` · `record` · `保存` · `记录` · `update docs` · `update memory` · `保存资产`

---

## What It Does (7 Steps) / 七步工作流

| Step | Action |
|------|--------|
| 1 | Harvest session knowledge / 采集会话知识 |
| 2 | Update `docs/TECH_LOG.md` with lessons / 更新技术日志 |
| 3 | Update `docs/CHANGELOG.md` with feature status / 更新变更记录 |
| 4 | Comment confirmed-fixed GitHub issues / 评论已修复 Issue |
| 5 | Update `docs/IDEAS.md` / 更新创意列表 |
| 6 | Update Claude Code auto-memory (MEMORY.md) / 更新持久记忆 |
| 7 | `git add docs/ && git commit && git push` |

---

## Configuration / 配置

Open the installed SKILL.md and fill in the CONFIG block:

```markdown
<!-- CONFIG
  GITHUB_REPO = owner/repo   e.g. myorg/myrepo
  DOCS_DIR    = docs/
  BRANCH      = main
-->
```

Memory path is auto-detected by Claude Code — no setup needed.

---

## Optional: Hooks (auto-remind on commit)

Copy entries from `hooks/settings-example.json` into `~/.claude/settings.json` to get:
- **SessionStart** — injects skill reminder into Claude's context every session
- **PreToolUse** — nudges Claude to save before `git commit`

---

## Directory Structure

```
save-project-assets/
├── install.py                              ← one-command installer
├── .claude-plugin/
│   ├── plugin.json
│   └── marketplace.json
├── .claude/skills/
│   ├── save-project-assets/SKILL.md        ← bilingual default
│   ├── save-project-assets-zh/SKILL.md     ← 中文
│   └── save-project-assets-en/SKILL.md     ← English
├── hooks/settings-example.json
└── README.md
```

---

## Requirements

- [Claude Code](https://claude.ai/code) CLI  
- `gh` CLI — for GitHub issue comments (optional)  
- `git`  
- A `docs/` folder in your project (or adjust paths in SKILL.md)

---

## License

MIT — free to use, fork, and adapt.

---

*Built from real-world usage on [DiskCleaner](https://github.com/mosjin/DiskCleanerSimple).*
