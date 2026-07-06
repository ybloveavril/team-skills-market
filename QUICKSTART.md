# 快速开始指南

## 🚀 3 分钟上手团队 Skills Market

### 步骤 1: 安装 Plexus

```bash
npm install -g plexus-agent-config
```

验证安装：
```bash
plexus --version
```

### 步骤 2: 订阅团队仓库

```bash
plexus join https://github.com/your-org/team-skills-market.git
```

如果是私有仓库，确保你已配置 GitHub 认证。

### 步骤 3: 同步配置

```bash
plexus pull
plexus sync
```

这会：
- 拉取最新的团队 Skills
- 合并到你的个人配置
- 同步到所有已安装的 AI 工具

### 步骤 4: 在 AI 工具中使用

打开 Claude Code、Cursor 或其他支持的 AI 工具，尝试：

```
请使用 code-reviewer 审查我的代码
```

或

```
使用 api-tester 为这个端点生成测试
```

---

## 🎯 常用命令速查

```bash
# 查看团队状态
plexus status

# 拉取最新更新
plexus pull

# 同步到 AI 工具
plexus sync

# 检测已安装的 AI 工具
plexus detect

# 启动 Web Dashboard
plexus
# 然后访问 http://localhost:7777
```

---

## 📱 Web Dashboard 操作

1. 运行 `plexus` 启动服务
2. 浏览器打开 http://localhost:7777
3. 左侧菜单导航：
   - **Dashboard**: 概览和快速操作
   - **Skills**: 浏览和管理 Skills
   - **Team**: 团队配置状态
   - **Backups**: 查看和恢复备份

---

## 💡 第一个 Skill 试用

试试 `code-reviewer`：

1. 打开你的代码项目
2. 在 AI 工具中输入：
   ```
   请使用 code-reviewer 审查当前文件
   ```
3. AI 会分析代码并提供改进建议

---

## ❓ 遇到问题？

### 检查清单

- [ ] Node.js >= 20
- [ ] 已安装至少一个支持的 AI 工具
- [ ] Git 已正确配置
- [ ] 网络连接正常（首次订阅需要）

### 获取帮助

- 📖 [完整使用指南](docs/USAGE_GUIDE.md)
- 🤝 [贡献指南](CONTRIBUTING.md)
- 🐛 [报告问题](https://github.com/your-org/team-skills-market/issues)

---

## 🎉 完成！

现在你已经准备好使用团队 Skills 了。探索可用的 Skills，找到能提升你工作效率的工具！

下一步：
- 浏览所有可用 Skills
- 学习如何创建自己的 Skill
- 分享给团队成员
