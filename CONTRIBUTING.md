# 贡献指南

欢迎为团队 Skills Market 贡献你的 Skill！本指南将帮助你快速上手。

## 📋 前置要求

1. **安装 Plexus**: `npm install -g plexus-agent-config`
2. **GitHub 账号**: 用于 Fork 仓库和提交 PR
3. **Git 基础**: 了解基本的 Git 操作

## 🎯 创建新 Skill

### Step 1: 规划你的 Skill

在开始之前，思考：
- 这个 Skill 解决什么问题？
- 目标用户是谁？
- 是否已有类似的 Skill？

建议先在 Issues 中提出你的想法，获取反馈。

### Step 2: 创建目录结构

```bash
# Fork 并 Clone 仓库
git clone https://github.com/your-username/team-skills-market.git
cd team-skills-market

# 创建 Skill 目录
mkdir -p skills/<your-skill-id>
```

**Skill ID 命名规范**:
- 使用小写字母、数字和连字符
- 简洁明了，如：`code-reviewer`、`api-tester`
- 避免特殊字符和空格

### Step 3: 编写 SKILL.md

创建 `skills/<your-skill-id>/SKILL.md`：

```markdown
---
name: 代码审查助手
description: 自动审查代码质量，提供改进建议
plexus_id: code-reviewer
plexus_enabled_agents:
  - claude-code
  - cursor
  - codex
---

# 代码审查助手

## 何时使用

当你完成代码修改，准备提交前，使用此 Skill 进行代码审查。

## 使用方法

1. 告诉 AI："请使用 code-reviewer 审查我的代码"
2. 指定要审查的文件或目录
3. AI 会检查以下方面：
   - 代码风格和一致性
   - 潜在的性能问题
   - 安全性漏洞
   - 最佳实践遵循情况

## 审查标准

### 代码质量
- 函数长度不超过 50 行
- 变量命名清晰有意义
- 避免嵌套过深（最多 3 层）

### 性能考虑
- 避免不必要的循环
- 合理使用缓存
- 注意内存泄漏风险

### 安全要点
- 不硬编码敏感信息
- 验证所有外部输入
- 使用参数化查询防止 SQL 注入

## 输出格式

审查结果应按以下格式输出：

```
🔍 代码审查报告

✅ 优点:
- ...

⚠️ 需要改进:
- [严重] ...
- [建议] ...

💡 建议:
- ...
```

## 示例

**输入**:
"请审查 src/utils/auth.ts"

**输出**:
```
🔍 代码审查报告

✅ 优点:
- 使用了 TypeScript 类型定义
- 错误处理完善

⚠️ 需要改进:
- [严重] JWT secret 应从环境变量读取，不要硬编码
- [建议] 添加 token 过期时间验证

💡 建议:
- 考虑添加 refresh token 机制
```
```

### Step 4: 添加可选资源（高级）

如果你的 Skill 需要配套脚本或参考文件：

```
skills/<your-skill-id>/
├── SKILL.md           # 必需
├── scripts/           # 可选：辅助脚本
│   └── check.sh
├── references/        # 可选：参考文档
│   └── style-guide.md
└── assets/            # 可选：其他资源
    └── template.txt
```

Plexus 会自动同步整个目录到 Agent 的 skill 目录。

### Step 5: 本地测试

```bash
# 将你的 fork 作为 team repo 测试
plexus join /path/to/your/fork
plexus pull
plexus sync

# 在 Claude Code 或 Cursor 中测试
# 尝试触发你的 Skill，确保它能正常工作
```

### Step 6: 提交 PR

```bash
# 提交更改
git add skills/<your-skill-id>
git commit -m "Add <skill-name>: brief description"

# Push 到你的 fork
git push origin main

# 在 GitHub 创建 Pull Request
```

PR 描述应包含：
- Skill 的功能说明
- 使用场景示例
- 截图或输出示例（如果适用）

## ✅ PR 审核清单

提交前自查：

- [ ] `SKILL.md` 包含正确的 frontmatter
- [ ] `plexus_id` 与目录名一致
- [ ] `description` 简洁清晰（50字以内）
- [ ] 内容使用中文（除非团队另有约定）
- [ ] 没有包含敏感信息（API keys、密码等）
- [ ] 已在本地测试过
- [ ] README 中的 Skill 列表已更新（如果适用）

## 🔧 更新现有 Skill

```bash
# 拉取最新代码
git pull upstream main

# 修改你的 Skill
vim skills/<skill-id>/SKILL.md

# 提交更新
git add skills/<skill-id>
git commit -m "Update <skill-name>: what changed"
git push
```

## ❌ 删除 Skill

如果需要删除自己发布的 Skill：

1. 确认没有其他成员依赖此 Skill
2. 在 Issue 中说明删除原因
3. 提交 PR 删除对应目录

## 💬 获取帮助

- 遇到问题？创建 [Issue](https://github.com/your-org/team-skills-market/issues)
- 想讨论想法？发起 [Discussion](https://github.com/your-org/team-skills-market/discussions)

## 🎓 学习资源

- [Plexus Skills 文档](https://minilv.github.io/Plexus/)
- [优秀 Skill 示例](skills/code-reviewer/SKILL.md)
- [Markdown Frontmatter 语法](https://yaml.org/)

感谢你的贡献！🙏
