# 部署和发布指南

本指南帮助你将团队 Skills Market 仓库发布到 GitHub，让团队成员可以使用。

## 📋 前置准备

### 1. 创建 GitHub 仓库

1. 访问 https://github.com/new
2. 仓库名称：`team-skills-market`（或你喜欢的名称）
3. 可见性：
   - **Public**: 开源团队，任何人都可以查看
   - **Private**: 内部团队，需要邀请才能访问
4. 先**不要**初始化 README（我们已经有自己的）

### 2. 配置 Git

```bash
cd "d:\MCP project\team-skills-market"

# 初始化 Git 仓库
git init

# 添加所有文件
git add .

# 首次提交
git commit -m "Initial commit: team skills market with 4 example skills"

# 添加远程仓库（替换为你的仓库地址）
git remote add origin https://github.com/your-org/team-skills-market.git

# 推送到 GitHub
git branch -M main
git push -u origin main
```

## 🔧 仓库设置

### 1. 更新 README 中的链接

编辑 `README.md`，将所有 `your-org` 和 `your-username` 替换为实际的 GitHub 组织或个人名称。

### 2. 启用 Issues 和 Discussions

在 GitHub 仓库设置中：
- ✅ 启用 Issues（用于报告问题）
- ✅ 启用 Discussions（用于讨论和建议）

### 3. 添加仓库描述

在 GitHub 仓库主页添加描述：
```
🎯 Team Skills Market - 共享高质量的 AI Agent Skills，提升团队开发效率
```

### 4. 添加 Topics

在仓库侧边栏添加 topics：
- `plexus`
- `ai-agents`
- `skills`
- `claude-code`
- `cursor`
- `team-collaboration`

## 👥 邀请团队成员

### 方法一：GitHub 协作

1. 进入 Settings → Collaborators
2. 点击 "Add people"
3. 输入团队成员的 GitHub 用户名
4. 选择权限级别（Read 即可）

### 方法二：公开仓库

如果仓库是公开的，团队成员可以直接克隆，无需邀请。

## 📢 通知团队

创建一条团队消息，包含：

```markdown
🎉 团队 Skills Market 已上线！

我们创建了一个共享的 AI Agent Skills 仓库，帮助大家提升工作效率。

✅ 已包含 4 个实用 Skills：
- 代码审查助手
- API 测试生成器
- 技术文档生成器
- Git 提交助手

🚀 快速开始：
1. 安装 Plexus: npm install -g plexus-agent-config
2. 订阅仓库: plexus join https://github.com/your-org/team-skills-market.git
3. 同步配置: plexus sync
4. 在 AI 工具中使用: "使用 code-reviewer 审查我的代码"

📖 详细文档：
- 快速开始: QUICKSTART.md
- 使用指南: docs/USAGE_GUIDE.md
- 贡献指南: CONTRIBUTING.md

欢迎试用并提出建议！也欢迎大家贡献自己的 Skills 🙌
```

## 🔄 日常维护

### 审核 PR

当团队成员提交新的 Skill PR 时：

1. 检查格式是否符合规范
2. 验证 frontmatter 是否正确
3. 确认内容有价值且清晰
4. 在本地测试（可选但推荐）
5. 合并 PR

### 定期更新

```bash
# 拉取最新更改
git pull

# 如果有冲突，解决后提交
git add .
git commit -m "Merge pull request #X from contributor"
git push
```

### 版本标记（可选）

对于重要更新，可以打标签：

```bash
git tag v1.0.0
git push origin v1.0.0
```

## 📊 监控使用情况

### 通过 Plexus Dashboard

团队成员可以在 Dashboard 的 Team 页面看到：
- 已订阅的 Skills 数量
- 是否有更新可拉取
- 同步状态

### 通过 GitHub Insights

仓库所有者可以看到：
- Clone 次数
- 活跃贡献者
- 热门 Skills（通过文件访问统计）

## 🛠️ 高级配置

### 添加 CI 检查（可选）

创建 `.github/workflows/validate.yml`：

```yaml
name: Validate Skills

on:
  pull_request:
    paths:
      - 'skills/**'

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Check SKILL.md exists
        run: |
          for dir in skills/*/; do
            if [ ! -f "$dir/SKILL.md" ]; then
              echo "❌ Missing SKILL.md in $dir"
              exit 1
            fi
          done
          echo "✅ All skills have SKILL.md"

      - name: Validate frontmatter
        run: |
          # Add your frontmatter validation logic here
          echo "✅ Frontmatter validation passed"
```

### 添加 Issue 模板

创建 `.github/ISSUE_TEMPLATE/skill-request.md`：

```markdown
---
name: Skill 请求
about: 请求添加新的 Skill
title: '[Skill Request] '
labels: enhancement
---

## Skill 名称

<!-- 你想要什么 Skill？ -->

## 使用场景

<!-- 这个 Skill 应该在什么情况下使用？ -->

## 预期功能

<!-- 这个 Skill 应该做什么？ -->

## 参考资源

<!-- 有没有类似的示例或文档？ -->
```

### 自动化 Changelog

使用 GitHub Releases 自动生成更新日志：

1. 进入 Releases 页面
2. 点击 "Draft a new release"
3. 创建标签（如 `v1.1.0`）
4. GitHub 会自动收集 PR 和 commits
5. 发布 Release

## 🎓 最佳实践

### 质量控制

- ✅ 每个 PR 至少一个 reviewer
- ✅ 新 Skill 必须包含完整示例
- ✅ 定期清理过时或无人使用的 Skills
- ✅ 保持文档更新

### 社区建设

- 💬 鼓励团队成员提出 Skill 想法
- 🏆 认可优秀贡献者
- 📝 定期分享使用案例
- 🔄 收集反馈并改进

### 安全考虑

- ⚠️ 定期审查 Skills，确保不包含敏感信息
- ⚠️ 私有仓库要管理好访问权限
- ⚠️ 不要在 Skill 中硬编码 API keys 或密码

## 📞 获取支持

遇到问题？

- 📖 查阅文档
- 🐛 提交 Issue
- 💬 发起 Discussion
- 📧 联系仓库维护者

---

恭喜！你的团队 Skills Market 已经准备就绪，可以开始使用了！🎉
