# OpenCode Skills

OpenSpec + SuperPowers fusion workflows for AI-assisted development.

---

## Skills | 技能

### [lsh-opsx-sp](./lsh-opsx-sp/) — OpenSpec + SuperPowers

| Property | Value |
|----------|-------|
| **Iron Law** | NO production code without TDD test first |
| **Phases** | explore → propose → apply |
| **OMO Agents** | ❌ Not included |
| **Best for** | Standard development, bug fixes |

```
/lsh-opsx-sp <topic>           # Full flow
/lsh-opsx-sp apply:<change>   # Apply with TDD
```

### [lsh-opsx-sp-omo](./lsh-opsx-sp-omo/) — OpenSpec + SuperPowers + OMO

| Property | Value |
|----------|-------|
| **Iron Law** | NO production code without TDD test first |
| **Phases** | explore → propose → apply |
| **OMO Agents** | ✅ Metis, Momus, Oracle, Hephaestus x5 |
| **Best for** | Complex changes, parallel TDD execution |

```
/lsh-opsx-sp-omo <topic>          # Full flow with OMO
/lsh-opsx-sp-omo apply:<change>  # Hephaestus x5 parallel
```

---

## 🚀 Quick Start | 快速开始

### 1. Install Prerequisites | 安装依赖

```bash
npm install -g opencode@latest
npm install -g @fission-ai/openspec@latest
openspec init --tools opencode
git init  # if not already a git repo
```

### 2. Choose Your Skill | 选择技能

| Scenario | Recommended |
|----------|-------------|
| Standard feature development | [lsh-opsx-sp](./lsh-opsx-sp/) |
| Bug fix with TDD | [lsh-opsx-sp](./lsh-opsx-sp/) |
| Complex multi-task changes | [lsh-opsx-sp-omo](./lsh-opsx-sp-omo/) |
| Parallel TDD execution | [lsh-opsx-sp-omo](./lsh-opsx-sp-omo/) |

### 3. Copy Skills | 复制技能

Copy the skill folder to your OpenCode skills directory:
```
~/.config/opencode/skill/
```

---

## 🤖 Compatible AI Tools | 兼容的 AI 工具

| AI Tool | Compatible | Note |
|---------|------------|------|
| **OpenCode** (oh-my-opencode) | ✅ Primary | Direct installation |
| Claude Desktop/CLI | ⚠️ Via OpenCode | Install OpenCode first |
| Cursor | ⚠️ Via OpenCode | Install OpenCode first |
| Windsurf | ⚠️ Via OpenCode | Install OpenCode first |

> These skills require OpenCode's skill system.

---

## 📁 File Structure | 文件结构

```
opencode-skills/
├── README.md                 # This file
├── LICENSE                   # MIT License
├── lsh-opsx-sp/             # OpenSpec + SuperPowers
│   ├── README.md           # Detailed documentation
│   ├── SKILL.md            # Skill definition
│   ├── phases-*.md         # Phase instructions
│   └── ...
└── lsh-opsx-sp-omo/        # OMO Enhanced
    ├── README.md           # Detailed documentation
    ├── SKILL.md            # Skill definition
    └── ...
```

---

## 💬 Feedback | 反馈

- **Issues**: [GitHub Issues](../../issues)
- **Discussions**: [GitHub Discussions](../../discussions)
- **PRs**: Contributions welcome!

---

## 📜 License

MIT License - See [LICENSE](./LICENSE)
