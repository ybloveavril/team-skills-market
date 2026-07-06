# 如何上传本地 Skill 到团队仓库

本指南将教你如何将已经创建好的本地 Skill 上传到团队 Skills Market。

## 📋 前置条件

- ✅ 你已经有一个可用的 Skill（在 `~/.config/plexus/personal/skills/<skill-id>/` 或其他地方）
- ✅ 你已 Clone 了团队仓库到本地
- ✅ 你有 GitHub 账号

---

## 🚀 方法一：从 Plexus Personal Layer 上传（推荐）

如果你的 Skill 已经在 Plexus 中：

### Step 1: 找到你的 Skill

```bash
# Windows PowerShell
cd ~/.config/plexus/personal/skills/
ls

# 或者查看具体 Skill
ls <your-skill-id>/
```

### Step 2: 复制到团队仓库

```bash
# 假设你的 Skill 叫 "my-awesome-skill"
cp -r ~/.config/plexus/personal/skills/my-awesome-skill/ \
      "d:\MCP project\team-skills-market\skills\my-awesome-skill\"
```

**Windows PowerShell 命令**:
```powershell
Copy-Item -Path "$env:USERPROFILE\.config\plexus\personal\skills\my-awesome-skill" `
          -Destination "d:\MCP project\team-skills-market\skills\my-awesome-skill" `
          -Recurse
```

### Step 3: 验证文件结构

```bash
cd "d:\MCP project\team-skills-market"
tree skills\my-awesome-skill

# 应该看到：
# skills\my-awesome-skill\
# └── SKILL.md
#     [可选] ├── scripts/
#     [可选] ├── references/
#     [可选] └── assets/
```

### Step 4: 测试 Skill 格式

确保 `SKILL.md` 包含正确的 frontmatter：

```markdown
---
name: My Awesome Skill
description: 简短描述（50字以内）
plexus_id: my-awesome-skill
plexus_enabled_agents:
  - claude-code
  - cursor
  - codex
---

# My Awesome Skill

## 何时使用
...

## 使用方法
...
```

### Step 5: 提交到 Git

```bash
cd "d:\MCP project\team-skills-market"

# 添加新 Skill
git add skills/my-awesome-skill/

# 提交
git commit -m "Add my-awesome-skill: brief description"

# 推送
git push
```

完成！✅ 你的 Skill 现在已经上传到团队仓库了！

---

## 🔧 方法二：从头创建新 Skill

如果你还没有 Skill，想直接在团队仓库中创建：

### Step 1: 创建 Skill 目录

```bash
cd "d:\MCP project\team-skills-market"

# 创建目录（替换为你的 skill id）
mkdir -p skills/my-new-skill
```

### Step 2: 创建 SKILL.md

```bash
# Windows PowerShell
@"
---
name: My New Skill
description: 这个 Skill 的简短描述
plexus_id: my-new-skill
plexus_enabled_agents:
  - claude-code
  - cursor
---

# My New Skill

## 何时使用

说明在什么场景下使用这个 Skill。

## 使用方法

详细描述如何使用，包括步骤和示例。

## 输出格式

期望的输出结果。

## 示例

提供实际使用示例。
"@ | Out-File -FilePath "skills\my-new-skill\SKILL.md" -Encoding utf8
```

### Step 3: 编辑和完善

用你喜欢的编辑器打开并完善内容：

```bash
# VS Code
code skills\my-new-skill\SKILL.md

# 或其他编辑器
notepad skills\my-new-skill\SKILL.md
```

### Step 4: 本地测试（可选但推荐）

```bash
# 将团队仓库作为 personal layer 测试
cp -r "d:\MCP project\team-skills-market\skills\my-new-skill" \
      ~/.config/plexus/personal/skills/

# 同步到 AI 工具
plexus sync

# 在 Claude Code 或 Cursor 中测试
# "使用 my-new-skill 做某事"
```

### Step 5: 提交并推送

```bash
cd "d:\MCP project\team-skills-market"

git add skills/my-new-skill/
git commit -m "Add my-new-skill: description"
git push
```

---

##  方法三：通过 Fork + PR 贡献（适合非维护者）

如果你不是仓库维护者，应该通过 Pull Request 贡献：

### Step 1: Fork 仓库

1. 访问 https://github.com/ybloveavril/team-skills-market
2. 点击右上角 **Fork** 按钮
3. 选择你的 GitHub 账号

### Step 2: Clone 你的 Fork

```bash
# 替换为你的用户名
git clone https://github.com/YOUR_USERNAME/team-skills-market.git
cd team-skills-market
```

### Step 3: 添加你的 Skill

按照**方法一**或**方法二**的步骤添加 Skill。

### Step 4: 提交到你的 Fork

```bash
git add skills/my-skill/
git commit -m "Add my-skill: description"
git push origin main
```

### Step 5: 创建 Pull Request

1. 访问你的 Fork 页面：https://github.com/YOUR_USERNAME/team-skills-market
2. 点击 **Compare & pull request**
3. 填写 PR 描述：
   - Skill 名称和功能
   - 使用场景
   - 示例输出
4. 点击 **Create pull request**

### Step 6: 等待审核

仓库维护者会审核你的 PR，可能会提出修改建议。审核通过后，你的 Skill 就会合并到主仓库！

---

## ✅ 上传前检查清单

在提交之前，请确认：

- [ ] `SKILL.md` 文件存在且格式正确
- [ ] Frontmatter 包含所有必需字段（name, description, plexus_id）
- [ ] `plexus_id` 与目录名一致
- [ ] `description` 简洁清晰（50字以内）
- [ ] 内容使用中文（或团队约定的语言）
- [ ] 没有包含敏感信息（API keys、密码、个人数据等）
- [ ] 已在本地测试过，能正常工作
- [ ] 遵循了团队的代码风格和规范

---

## 🐛 常见问题

### Q: 上传后其他人看不到？

**A**: 确保你已经 `git push` 成功。检查 GitHub 仓库页面是否显示了你的 Skill。

### Q: 如何更新已上传的 Skill？

```bash
# 修改 Skill 文件
vim skills/my-skill/SKILL.md

# 提交更新
git add skills/my-skill/
git commit -m "Update my-skill: what changed"
git push
```

### Q: 如何删除 Skill？

```bash
# 删除目录
rm -rf skills/my-skill/

# 提交删除
git add -A
git commit -m "Remove my-skill: reason"
git push
```

⚠️ **注意**: 删除前请确认没有其他成员依赖此 Skill！

### Q: 可以上传带脚本的 Skill 吗？

**A**: 可以！只需将整个目录复制过去：

```
skills/my-skill/
── SKILL.md
├── scripts/
│   └── helper.sh
└── references/
    └── guide.md
```

Plexus 会同步整个目录。

### Q: Frontmatter 格式错误怎么办？

**A**: 确保 YAML 格式正确：

```yaml
---
name: Skill Name        # 字符串
description: 描述       # 字符串
plexus_id: skill-id     # 字符串
plexus_enabled_agents:  # 列表
  - claude-code
  - cursor
---
```

注意：
- 使用 2 个空格缩进
- 冒号后要有空格
- 列表项前有 `- `

---

##  最佳实践

1. **从小处开始**: 先上传一个简单的 Skill 熟悉流程
2. **文档清晰**: 提供详细的使用说明和示例
3. **专注单一职责**: 一个 Skill 解决一个问题
4. **收集反馈**: 关注团队成员的使用体验
5. **持续改进**: 根据反馈不断优化

---

## 📞 需要帮助？

- 查看 [CONTRIBUTING.md](CONTRIBUTING.md) 了解更多规范
- 在 [Issues](https://github.com/ybloveavril/team-skills-market/issues) 中提问
- 联系仓库维护者

祝你上传顺利！🎉
