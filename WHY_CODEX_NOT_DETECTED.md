# 为什么 Plexus 检测不到 Codex？

##  问题原因

Plexus 检测 Agent 的逻辑需要满足以下条件之一：

1. ✅ **配置目录存在**（`~/.codex/`）
2.  **CLI 命令在 PATH 中**（可以运行 `codex --version`）
3. **macOS App Bundle 存在**（仅 macOS）

### 你的情况

- ✅ `~/.codex/` 目录存在
- ❌ 没有安装 Codex CLI（无法运行 `codex` 命令）
- ❌ Windows 系统（没有 App Bundle）

由于第二个条件不满足，Plexus 认为 Codex 未安装。

---

## 💡 解决方案

### 方案一：使用环境变量强制检测（临时方案）

设置环境变量让 Plexus 只检查配置文件：

**Windows PowerShell**:
```powershell
$env:PLEXUS_DETECT_CONFIG_ONLY="1"
plexus
```

**或在启动 Dashboard 时**:
```bash
cd "d:\MCP project\Plexus\apps\web"
$env:PLEXUS_DETECT_CONFIG_ONLY="1"
node ../../node_modules/next/dist/bin/next dev --webpack --port 7777
```

这样 Plexus 会检测到 Codex（因为配置目录存在）。

### 方案二：安装 Codex CLI（推荐，永久解决）

#### 方法 A：使用 npm 安装

```bash
npm install -g @openai/codex-cli
```

#### 方法 B：从官网下载

访问 Codex 官方网站下载安装包。

#### 验证安装

```bash
codex --version
```

如果能看到版本号，说明安装成功。重启 Plexus Dashboard 后就能检测到 Codex 了。

### 方案三：手动创建假命令（高级，不推荐）

创建一个假的 `codex` 命令让 Plexus 检测到：

**Windows**:
```batch
# 在 PATH 中的某个目录创建 codex.cmd
echo @echo off > C:\Users\ivan_yang\AppData\Local\Microsoft\WindowsApps\codex.cmd
echo echo Codex CLI >> C:\Users\ivan_yang\AppData\Local\Microsoft\WindowsApps\codex.cmd
```

⚠️ **注意**: 这只是让 Plexus 检测到，实际使用 Codex 功能时还是会失败。

---

## 🎯 最佳实践

### 如果你只是想使用 Skills

**不需要安装 Codex CLI！**

Skills 是纯文本文件（Markdown），可以直接在任何 AI 工具中使用：

1. 订阅团队仓库：
   ```bash
   plexus join https://github.com/ybloveavril/team-skills-market.git
   plexus sync
   ```

2. 在已安装的 AI 工具中使用（如 Claude Code、Cursor）：
   ```
   "请使用 code-reviewer 审查我的代码"
   ```

Skills 会自动同步到所有已安装的 Agent，即使 Codex 未安装也不影响其他工具使用。

### 如果你想使用 Codex

**安装 Codex CLI**：

```bash
npm install -g @openai/codex-cli
```

然后 Plexus 会自动检测到 Codex，并将 Skills 同步到 `~/.codex/skills/`。

---

##  当前状态

### 已安装的 Agents

运行以下命令查看 Plexus 检测到的 Agents：

```bash
cd "d:\MCP project\Plexus"
node ./packages/core/dist/index.js detect
```

或在 Dashboard 的首页查看 "Detected Agents" 部分。

### 预期结果

- ✅ **Claude Code**: 已检测（如果安装了）
- ✅ **Cursor**: 已检测（如果安装了）
- ️ **Codex**: 可能未检测（如果没有安装 CLI）
- ✅ **Gemini CLI**: 已检测（如果安装了）
- ✅ **Qwen Code**: 已检测（如果安装了）

---

## 🔄 刷新检测

如果安装了 Codex CLI 后 Plexus 仍然检测不到：

1. **重启 Dashboard**：
   ```bash
   # 停止当前进程（Ctrl+C）
   # 重新启动
   cd "d:\MCP project\Plexus\apps\web"
   node ../../node_modules/next/dist/bin/next dev --webpack --port 7777
   ```

2. **硬刷新浏览器**：
   - Windows: `Ctrl + Shift + R`
   - Mac: `Cmd + Shift + R`

3. **清除缓存**（如果还不行）：
   ```bash
   rm -rf ~/.config/plexus/.cache
   ```

---

## ❓ 常见问题

### Q: 不安装 Codex CLI 会影响使用 Skills 吗？

**A**: 不会！Skills 会同步到其他已安装的 Agent（如 Claude Code、Cursor）。只是不能在 Codex 中使用而已。

### Q: 我必须安装所有 Agent 吗？

**A**: 不需要！只安装你实际使用的 Agent 即可。Plexus 只会同步到已安装的 Agent。

### Q: 如何知道哪些 Agent 已安装？

**A**: 
- 查看 Dashboard 首页的 "Detected Agents"
- 或运行：`plexus detect`

### Q: Codex CLI 和 Codex Web 版有什么区别？

**A**: 
- **Codex CLI**: 命令行工具，Plexus 可以集成
- **Codex Web**: 网页版，Plexus 无法直接管理

Plexus 只能管理本地安装的 CLI 版本。

---

## 🎓 总结

| 场景 | 是否需要安装 Codex CLI | 能否使用 Skills |
|------|---------------------|----------------|
| 只想在 Claude Code/Cursor 中使用 | ❌ 不需要 | ✅ 可以 |
| 想在 Codex 中使用 | ✅ 需要 | ✅ 可以 |
| 想管理所有 Agent 配置 | ✅ 建议安装 | ✅ 可以 |

**建议**：
- 如果你不使用 Codex，可以不安装 CLI
- Skills 仍然可以在其他 Agent 中使用
- Plexus 会正确同步到所有已安装的 Agent

---

**相关文档**:
- [FOR_TEAM_MEMBERS.md](FOR_TEAM_MEMBERS.md) - 团队成员快速上手
- [WINDOWS_FIX.md](WINDOWS_FIX.md) - Windows 问题修复
- [docs/USAGE_GUIDE.md](docs/USAGE_GUIDE.md) - 完整使用指南
