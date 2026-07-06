# Team Skills Market

团队共享 Skill 市场 - 让每个成员都能轻松发现和使用高质量的 AI Agent Skills。

## 📖 这是什么？

这是一个基于 [Plexus](https://github.com/miniLV/Plexus) 的团队 Skill 分享平台。团队成员可以：

- **发布** 自己创建的实用 Skills
- **浏览** 其他人分享的 Skills
- **订阅** 整个仓库，一键同步到本地 AI 工具（Claude Code、Cursor、Codex 等）

## 🚀 快速开始

### 作为使用者：订阅团队 Skills

```bash
# 1. 安装 Plexus（如果还没有）
npm install -g plexus-agent-config

# 2. 订阅团队 Skill 仓库
plexus join https://github.com/ybloveavril/team-skills-market.git

# 3. 拉取最新 Skills
plexus pull

# 4. 同步到你的 AI 工具
plexus sync
```

或者使用 Web Dashboard：
1. 打开 http://localhost:7777
2. 进入 **Team** 页面
3. 点击"加入团队"，输入仓库地址
4. 点击"同步配置"

### 作为贡献者：发布你的 Skill

```bash
# 1. Fork 这个仓库
# 2. Clone 到本地
git clone https://github.com/ybloveavril/team-skills-market.git
cd team-skills-market

# 3. 创建你的 Skill
mkdir -p skills/my-awesome-skill
touch skills/my-awesome-skill/SKILL.md

# 4. 编辑 SKILL.md（参考下面的模板）
# 5. 提交 PR
git add .
git commit -m "Add my-awesome-skill"
git push
# 然后在 GitHub 上创建 Pull Request
```

## 📁 仓库结构

```
team-skills-market/
├── README.md                    # 本文件
├── CONTRIBUTING.md              # 贡献指南
└── skills/                      # Skills 目录
    ├── code-reviewer/           # Skill ID（唯一标识）
    │   └── SKILL.md             # Skill 定义文件
    ├── api-tester/
    │   ├── SKILL.md
    │   └── scripts/             # 可选：配套脚本
    └── doc-writer/
        └── SKILL.md
```

## 📝 Skill 文件格式

每个 Skill 必须包含 `SKILL.md` 文件，格式如下：

```markdown
---
name: skill-display-name
description: 简短描述这个 Skill 的用途（50字以内）
plexus_id: unique-skill-id
plexus_enabled_agents:
  - claude-code
  - cursor
  - codex
---

# Skill 名称

## 何时使用

说明在什么场景下应该触发这个 Skill。

## 使用方法

详细描述如何使用这个 Skill，包括：
- 步骤说明
- 示例代码
- 注意事项

## 输出格式

期望的输出格式或结果。
```

### Frontmatter 字段说明

| 字段 | 必填 | 说明 |
|------|------|------|
| `name` | 是 | Skill 显示名称 |
| `description` | 是 | 简短描述 |
| `plexus_id` | 是 | 唯一标识符（小写字母+连字符） |
| `plexus_enabled_agents` | 否 | 支持的 Agent 列表，默认全部支持 |

支持的 Agent IDs：
- `claude-code`
- `cursor`
- `codex`
- `gemini-cli`
- `qwen-code`
- `factory-droid`

## 🎯 现有 Skills

查看 [skills/](skills/) 目录浏览所有可用 Skills，或访问 Web Dashboard 的 Skills 页面查看详细信息。

## 🔧 管理 Skills

### 更新本地 Skills

```bash
plexus pull    # 拉取最新 Skills
plexus sync    # 同步到 AI 工具
```

### 选择性启用/禁用

在 Plexus Dashboard 的 **Skills** 页面，可以为每个 Skill 选择启用的 Agent。

### 个人覆盖

如果需要自定义某个 Team Skill，可以在 personal layer 创建同名 Skill：

```bash
~/.config/plexus/personal/skills/<skill-id>/SKILL.md
```

Personal layer 会覆盖 Team layer 的同名 Skill。

## 🤝 贡献流程

1. **Fork** 本仓库
2. **创建分支**: `git checkout -b feature/my-new-skill`
3. **添加 Skill**: 在 `skills/` 下创建新目录和 `SKILL.md`
4. **测试**: 确保格式正确，能在 Plexus 中正常加载
5. **提交**: `git commit -m "Add <skill-name>"`
6. **Push**: `git push origin feature/my-new-skill`
7. **PR**: 在 GitHub 创建 Pull Request

详细规范请查看 [CONTRIBUTING.md](CONTRIBUTING.md)。

## 💡 Skill 创意方向

好的 Skill 通常具备以下特点：

- ✅ **解决具体问题**: 如"代码审查"、"API 测试"、"文档生成"
- ✅ **可复用性强**: 适用于多个项目或场景
- ✅ **清晰的使用指南**: 让其他人能快速上手
- ✅ **专注单一职责**: 一个 Skill 做一件事并做好

避免：
- ❌ 过于宽泛的主题
- ❌ 依赖特定项目结构的 Skill
- ❌ 包含敏感信息或个人配置

## 📊 统计

- Total Skills: 见 Dashboard
- Contributors: 欢迎成为第一个！

## 🔗 相关链接

- [Plexus 官方文档](https://minilv.github.io/Plexus/)
- [如何创建 Skill](https://minilv.github.io/Plexus/how-to-sync-claude-code-and-codex-skills-mcp.html)
- [团队配置最佳实践](https://minilv.github.io/Plexus/how-to-sync-ai-agent-configs-without-drift-zh.html)

## 📄 License

MIT
