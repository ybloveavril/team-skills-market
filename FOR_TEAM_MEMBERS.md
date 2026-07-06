# 团队成员快速上手指南

**你不需要懂 Git，也不需要手动下载代码！**

Plexus 会帮你处理一切。只需 3 个命令：

## 🚀 3 步开始使用

### 1️⃣ 安装 Plexus

```bash
npm install -g plexus-agent-config
```

### 2️⃣ 订阅团队仓库

```bash
plexus join https://github.com/ybloveavril/team-skills-market.git
```

**这一步 Plexus 会自动：**
- ✅ 下载团队 Skills
- ✅ 保存到本地
- ✅ 准备同步

### 3️⃣ 同步到 AI 工具

```bash
plexus sync
```

**这一步 Plexus 会自动：**
- ✅ 将 Skills 复制到 Claude Code、Cursor 等工具
- ✅ 创建必要的链接
- ✅ 备份原有配置

---

##  完成！开始使用

打开你的 AI 工具（Claude Code、Cursor 等），尝试：

```
"请使用 code-reviewer 审查我的代码"
```

或

```
"使用 api-tester 生成测试用例"
```

---

## 🔄 获取最新 Skills

当团队添加了新 Skills 后，只需运行：

```bash
plexus pull    # 拉取最新内容
plexus sync    # 同步到 AI 工具
```

或者在 Web Dashboard 中点击"拉取更新"和"同步配置"。

---

## 📱 可选：使用图形界面

如果你喜欢可视化操作：

```bash
plexus
```

然后打开浏览器访问 http://localhost:7777

在 Dashboard 中你可以：
-  浏览所有可用 Skills
- 🔧 启用/禁用特定 Skills
- 📊 查看同步状态
- 💾 管理备份

---

## ❓ 常见问题

### Q: 我需要安装 Git 吗？

**A**: 是的，需要安装 Git，但不需要会使用 Git 命令。Plexus 会在后台自动使用 Git。

安装 Git：https://git-scm.com/downloads

### Q: 我需要 Fork 或 Clone 仓库吗？

**A**: **不需要！** Plexus 的 `join` 命令会自动处理。

### Q: 如何知道有哪些 Skills 可用？

**A**: 
- 方法 1: 运行 `plexus` 打开 Dashboard，查看 Skills 页面
- 方法 2: 访问 GitHub 仓库：https://github.com/ybloveavril/team-skills-market
- 方法 3: 查看本地目录：`ls ~/.config/plexus/team/skills/`

### Q: 可以只使用部分 Skills 吗？

**A**: 可以！在 Dashboard 的 Skills 页面，可以为每个 Skill 选择启用的 AI 工具。

### Q: 如何添加自己的 Skill？

**A**: 
1. 在 Dashboard 的 Skills 页面点击"新建 Skill"
2. 或在命令行创建：
   ```bash
   mkdir -p ~/.config/plexus/personal/skills/my-skill
   vim ~/.config/plexus/personal/skills/my-skill/SKILL.md
   ```
3. 运行 `plexus sync` 同步

如果想分享给团队，参考 [CONTRIBUTING.md](CONTRIBUTING.md)。

### Q: 遇到问题怎么办？

**A**:
- 查看 [HOW_TO_UPLOAD_SKILL.md](HOW_TO_UPLOAD_SKILL.md)
- 查看 [WINDOWS_FIX.md](WINDOWS_FIX.md)（Windows 用户）
- 在 [Issues](https://github.com/ybloveavril/team-skills-market/issues) 中报告

---

## 🎓 总结

| 你需要做的 | Plexus 帮你做的 |
|-----------|----------------|
| 运行 `plexus join` | 自动 Clone Git 仓库 |
| 运行 `plexus pull` | 自动 Git Pull 更新 |
| 运行 `plexus sync` | 自动同步到所有 AI 工具 |
| 在 AI 工具中使用 | 自动管理文件链接和备份 |

**你完全不需要手动操作 Git 或下载文件！**

---

**仓库地址**: https://github.com/ybloveavril/team-skills-market

**开始使用吧！** 🚀
