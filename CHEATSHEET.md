# 团队 Skills Market - 快速参考卡

## 📁 项目位置

```
d:\MCP project\team-skills-market\
```

## 🚀 发布到 GitHub（3步）

```bash
cd "d:\MCP project\team-skills-market"

# 1. 初始化 Git
git init && git add . && git commit -m "Initial commit"

# 2. 关联远程仓库（替换为你的地址）
git remote add origin https://github.com/your-org/team-skills-market.git

# 3. 推送
git branch -M main && git push -u origin main
```

## 📋 团队成员使用（4步）

```bash
# 1. 安装 Plexus
npm install -g plexus-agent-config

# 2. 订阅仓库
plexus join https://github.com/your-org/team-skills-market.git

# 3. 拉取并同步
plexus pull && plexus sync

# 4. 在 AI 工具中使用
# "使用 code-reviewer 审查代码"
```

## 🎯 现有 Skills（4个）

| Skill | 用途 | 命令示例 |
|-------|------|---------|
| code-reviewer | 代码审查 | "使用 code-reviewer 审查 src/auth.ts" |
| api-tester | API 测试生成 | "使用 api-tester 为 /api/users 生成测试" |
| doc-writer | 文档生成 | "使用 doc-writer 生成 README" |
| git-commit-helper | Git 提交信息 | "使用 git-commit-helper 生成 commit message" |

## 📖 文档导航

- **新手**: 阅读 `QUICKSTART.md`
- **使用者**: 阅读 `docs/USAGE_GUIDE.md`
- **贡献者**: 阅读 `CONTRIBUTING.md`
- **维护者**: 阅读 `DEPLOYMENT.md`
- **概览**: 阅读 `README.md` 和 `PROJECT_SUMMARY.md`

## 🔧 常用命令

```bash
plexus              # 启动 Dashboard (http://localhost:7777)
plexus detect       # 检测已安装的 AI 工具
plexus status       # 查看团队订阅状态
plexus pull         # 拉取最新 Skills
plexus sync         # 同步到 AI 工具
```

## 💡 创建新 Skill（模板）

```bash
# 1. 创建目录
mkdir -p skills/my-skill-id

# 2. 创建 SKILL.md
cat > skills/my-skill-id/SKILL.md << 'EOF'
---
name: My Skill Name
description: 简短描述（50字以内）
plexus_id: my-skill-id
plexus_enabled_agents:
  - claude-code
  - cursor
---

# My Skill

## 何时使用
...

## 使用方法
...

## 示例
...
EOF

# 3. 测试并提交
git add . && git commit -m "Add my-skill-id"
git push
# 在 GitHub 创建 PR
```

## ⚠️ 注意事项

- ✅ Skill ID 使用小写字母+连字符
- ✅ description 控制在 50 字以内
- ✅ 不要包含敏感信息（API keys、密码等）
- ✅ 每个 Skill 专注解决一个问题
- ❌ 不要硬编码个人配置或路径

## 🆘 遇到问题？

1. 检查 Plexus 版本：`plexus --version`
2. 查看完整文档：`docs/USAGE_GUIDE.md`
3. 报告 Issue：GitHub Issues
4. 寻求帮助：GitHub Discussions

---

**提示**: 将此文件打印或保存，方便随时查阅！
