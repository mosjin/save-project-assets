# save-project-assets

> **在提交代码时或按需主动保存所有项目知识。**  
> **Proactively save all project knowledge at commit time or on demand.**

一个 [Claude Code](https://claude.ai/code) skill，自动持久化 AI 助手在会话中产生的所有知识：

- 📋 **TECH_LOG** — 工程经验 & 根因分析 / Engineering lessons & root-cause analyses
- 📝 **CHANGELOG** — 功能状态 & 修复记录 / Feature status & fix records
- 💬 **GitHub Issues** — 自动评论已修复 Issue / Auto-comment confirmed fixes
- 💡 **IDEAS** — 未来工作 & 改进想法 / Future work & improvement notes
- 🧠 **Memory** — 跨会话持久上下文 / Persistent cross-session context

双语：**中文 (zh)** + **English (en)** + **自动识别 (default)**

---

## 安装 / Install

### ✅ 最简方式：2 条命令（推荐）

save-project-assets 以 Claude Code 插件市场的形式托管在 GitHub 上。

**第一步：添加插件市场**

```
/plugin marketplace add mosjin/save-project-assets
```

**第二步：安装插件**

```
/plugin install save-project-assets
```

搞定。插件已在你的 Claude Code 会话中可用。

---

### 备用方式 B：一键脚本（无需在 Claude Code 内操作）

```bash
git clone https://github.com/mosjin/save-project-assets
cd save-project-assets
python install.py
```

然后在 Claude Code 中运行 `/reload-plugins`。

卸载：`python install.py --remove` + `/reload-plugins`

---

### 备用方式 C：手动 JSON

在 `~/.claude/settings.json` 中添加：

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

然后 `/reload-plugins`。

---

## 使用 / Usage

```
/save-project-assets        # 自动识别语言 / auto language
/save-project-assets-zh     # 中文
/save-project-assets-en     # English
```

**自动触发**（无需输入命令）当你说：  
`保存` · `记录` · `保存资产` · `更新文档` · `更新记忆`  
`save` · `record` · `assets` · `update docs` · `update memory`

---

## 工作流（7 步）/ What It Does

| 步骤 | 操作 |
|------|------|
| 1 | 采集会话知识 / Harvest session knowledge |
| 2 | 更新 `docs/TECH_LOG.md` 经验教训 |
| 3 | 更新 `docs/CHANGELOG.md` 功能状态 |
| 4 | 评论已确认修复的 GitHub Issue |
| 5 | 更新 `docs/IDEAS.md` 新想法 |
| 6 | 更新 Claude Code 自动记忆 (MEMORY.md) |
| 7 | `git add docs/ && git commit && git push` |

---

## 配置 / Configuration

安装后打开 SKILL.md，填写顶部 CONFIG 块：

```markdown
<!-- CONFIG
  GITHUB_REPO = owner/repo   例：myorg/myrepo
  DOCS_DIR    = docs/
  BRANCH      = main
-->
```

记忆路径由 Claude Code 自动检测，无需配置。

---

## 可选：Hooks（自动提醒）

将 `hooks/settings-example.json` 中的内容复制到 `~/.claude/settings.json`，可获得：
- **SessionStart hook** — 每次会话注入 skill 提醒
- **PreToolUse hook** — `git commit` 前自动提醒保存

---

## 目录结构 / Directory Structure

```
save-project-assets/
├── install.py                              ← 备用一键安装脚本
├── .claude-plugin/
│   ├── plugin.json                         ← 插件元数据
│   └── marketplace.json                    ← GitHub 插件市场清单
├── .claude/skills/
│   ├── save-project-assets/SKILL.md        ← 双语默认版
│   ├── save-project-assets-zh/SKILL.md     ← 中文版
│   └── save-project-assets-en/SKILL.md     ← 英文版
├── hooks/settings-example.json             ← Hook 配置示例
└── README.md
```

---

## 前置条件 / Requirements

- [Claude Code](https://claude.ai/code) CLI
- `gh` CLI — 用于 GitHub Issue 评论（可选）
- `git`
- 项目中有 `docs/` 目录（或在 SKILL.md 中调整路径）

---

## License

MIT — 自由使用、fork 和修改。

---

*从 [DiskCleaner](https://github.com/mosjin/DiskCleanerSimple) 实战中提炼而来。*
