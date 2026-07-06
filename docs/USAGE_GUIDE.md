# 团队 Skill 使用完全指南

本指南将帮助你从零开始使用团队 Skills Market，包括订阅、浏览、使用和贡献 Skills。

## 📚 目录

1. [快速开始](#快速开始)
2. [订阅团队 Skills](#订阅团队-skills)
3. [浏览可用 Skills](#浏览可用-skills)
4. [在 AI 工具中使用 Skills](#在-ai-工具中使用-skills)
5. [管理本地 Skills](#管理本地-skills)
6. [贡献你的 Skill](#贡献你的-skill)
7. [常见问题](#常见问题)

---

## 快速开始

### 前置条件

确保你已经安装了：
- Node.js >= 20
- Git
- 至少一个支持的 AI 工具（Claude Code、Cursor、Codex 等）

### 5 分钟上手

```bash
# 1. 安装 Plexus
npm install -g plexus-agent-config

# 2. 启动 Dashboard
plexus

# 3. 在浏览器打开 http://localhost:7777
# 4. 进入 Team 页面，点击"加入团队"
# 5. 输入仓库地址：https://github.com/your-org/team-skills-market.git
# 6. 点击"同步配置"

完成！现在你可以在 AI 工具中使用团队 Skills 了。
```

---

## 订阅团队 Skills

### 方法一：使用 CLI

```bash
# 首次订阅
plexus join https://github.com/your-org/team-skills-market.git

# 拉取最新更新
plexus pull

# 同步到所有 AI 工具
plexus sync
```

### 方法二：使用 Web Dashboard

1. 打开 http://localhost:7777
2. 点击左侧菜单 **Team**
3. 点击"加入团队配置"
4. 粘贴仓库 URL
5. 等待克隆完成
6. 点击"同步配置"按钮

### 验证订阅成功

```bash
# 查看团队状态
plexus status

# 应该看到类似输出：
# Team: subscribed
# Repo: https://github.com/your-org/team-skills-market.git
# Skills: 4
# Behind: 0 commits
```

或在 Dashboard 的 Team 页面查看状态。

---

## 浏览可用 Skills

### 在 Web Dashboard 中浏览

1. 打开 http://localhost:7777
2. 点击左侧菜单 **Skills**
3. 你会看到两个标签页：
   - **Team**: 团队共享的 Skills
   - **Personal**: 你个人的 Skills

每个 Skill 显示：
- 名称和描述
- 启用的 Agent
- 来源（Team/Personal）
- 操作按钮（启用/禁用、编辑）

### 查看 Skill 详情

点击 Skill 行可以展开查看详情，包括完整的 SKILL.md 内容。

### 在 GitHub 上浏览

直接访问仓库的 `skills/` 目录：
```
https://github.com/your-org/team-skills-market/tree/main/skills
```

每个 Skill 是一个子目录，包含 `SKILL.md` 文件。

---

## 在 AI 工具中使用 Skills

### Claude Code

Skills 会自动出现在 `~/.claude/skills/` 目录。

**使用方法**:
```
请使用 code-reviewer 审查我的代码
```

或明确指定：
```
@code-reviewer 帮我检查 src/auth.ts 的安全性
```

### Cursor

Skills 会链接到 `~/.cursor/commands/` 目录。

**使用方法**:
- 按 `Cmd+K` (Mac) 或 `Ctrl+K` (Windows/Linux)
- 输入 Skill 名称，如 "code-reviewer"
- 按照提示操作

### Codex

Skills 位于 `~/.codex/skills/`。

**使用方法**:
在对话中提及 Skill 名称即可触发。

### 通用技巧

1. **明确意图**: 告诉 AI 你想做什么
   ```
   我想为 API 端点生成测试用例
   ```

2. **提供上下文**: 指定文件或功能
   ```
   使用 api-tester 为 userController.ts 生成测试
   ```

3. **调整参数**: 根据需要定制
   ```
   使用 doc-writer 生成英文版的 API 文档
   ```

---

## 管理本地 Skills

### 选择性启用/禁用

在 Dashboard 的 Skills 页面：
1. 找到要管理的 Skill
2. 点击右侧的开关按钮
3. 选择要在哪些 Agent 中启用

或者在命令行：
```bash
# 通过 Dashboard 操作后，运行
plexus sync
```

### 自定义 Team Skill

如果想修改某个 Team Skill：

1. 在 personal layer 创建同名 Skill：
   ```bash
   mkdir -p ~/.config/plexus/personal/skills/code-reviewer
   cp ~/.config/plexus/team/skills/code-reviewer/SKILL.md \
      ~/.config/plexus/personal/skills/code-reviewer/SKILL.md
   ```

2. 编辑个人版本：
   ```bash
   vim ~/.config/plexus/personal/skills/code-reviewer/SKILL.md
   ```

3. Personal layer 会自动覆盖 Team layer

### 添加个人 Skills

```bash
# 创建新 Skill
mkdir -p ~/.config/plexus/personal/skills/my-custom-skill
vim ~/.config/plexus/personal/skills/my-custom-skill/SKILL.md

# 同步
plexus sync
```

或在 Dashboard 的 Skills 页面点击"新建 Skill"。

### 更新团队 Skills

```bash
# 拉取最新更改
plexus pull

# 如果有新提交，会看到更新提示
# 然后同步到本地
plexus sync
```

---

## 贡献你的 Skill

### 准备工作

1. Fork 仓库到个人账号
2. Clone 到本地：
   ```bash
   git clone https://github.com/your-username/team-skills-market.git
   cd team-skills-market
   ```

### 创建 Skill

参考 [CONTRIBUTING.md](../CONTRIBUTING.md) 的详细步骤。

简要流程：
1. 创建目录：`mkdir -p skills/my-skill`
2. 编写 `SKILL.md`
3. 本地测试
4. 提交 PR

### 本地测试

```bash
# 将你的 fork 作为 team repo
plexus join /path/to/your/fork
plexus pull
plexus sync

# 在 AI 工具中测试
# 例如在 Claude Code 中：
# "使用 my-skill 做某事"
```

### 提交 PR

```bash
git add skills/my-skill
git commit -m "feat: add my-skill for code analysis"
git push origin main

# 在 GitHub 创建 Pull Request
# 填写 PR 描述，等待审核
```

### PR 审核标准

- ✅ Frontmatter 格式正确
- ✅ 描述清晰简洁
- ✅ 有实用价值
- ✅ 不包含敏感信息
- ✅ 已在本地测试过

---

## 常见问题

### Q: 订阅后看不到新 Skills？

**A**: 需要拉取最新更改并同步：
```bash
plexus pull
plexus sync
```

或在 Dashboard 点击"拉取更新"和"同步配置"。

### Q: 如何知道有哪些 Skills 可用？

**A**:
- Dashboard 的 Skills 页面查看所有已订阅 Skills
- GitHub 仓库的 `skills/` 目录查看完整列表
- 运行 `ls ~/.config/plexus/team/skills/` 查看本地副本

### Q: Team Skill 和个人 Skill 冲突怎么办？

**A**: Personal layer 优先级更高。如果你想用团队版本，删除个人版本：
```bash
rm -rf ~/.config/plexus/personal/skills/<skill-id>
plexus sync
```

### Q: 可以在多个团队间切换吗？

**A**: 当前 Plexus 只支持一个 team repo。如需切换：
```bash
# 删除当前 team layer
rm -rf ~/.config/plexus/team

# 加入新团队
plexus join https://github.com/new-org/new-skills-market.git
plexus pull
plexus sync
```

### Q: Skills 会占用很多空间吗？

**A**: 不会。每个 Skill 通常只有几 KB 到几十 KB。即使有几十个 Skills，总大小也在 MB 级别。

### Q: 如何报告 Skill 的问题？

**A**:
- 如果是团队 Skill，在仓库 Issues 中报告
- 如果是个人问题，检查：
  - Plexus 版本是否最新
  - Skill 格式是否正确
  - AI 工具是否正常加载

### Q: 可以离线使用 Skills 吗？

**A**: 可以。一旦同步到本地，Skills 就存储在 `~/.config/plexus/`，无需联网即可使用。只需定期 `plexus pull` 获取更新。

### Q: 如何创建带脚本的 Skill？

**A**: 在 Skill 目录下添加 `scripts/` 目录：
```
skills/my-skill/
├── SKILL.md
└── scripts/
    └── helper.sh
```

Plexus 会同步整个目录。在 SKILL.md 中引用脚本：
```markdown
运行配套脚本：
```bash
./scripts/helper.sh --option value
```
```

### Q: 团队 Skill 的最佳实践是什么？

**A**:
1. **专注通用场景**: 避免特定项目依赖
2. **保持更新**: 定期审查和改进
3. **文档清晰**: 让别人能快速上手
4. **小而精**: 一个 Skill 解决一个问题
5. **收集反馈**: 关注团队成员的使用体验

---

## 下一步

- 📖 阅读 [CONTRIBUTING.md](../CONTRIBUTING.md) 学习如何贡献
- 🔍 浏览现有 Skills 获取灵感
- 💡 创建你的第一个 Skill
- 🤝 邀请团队成员加入

有任何问题？在仓库 Issues 中提问！
