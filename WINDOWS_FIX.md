# Windows 运行 Plexus 问题修复

## ❌ 遇到的问题

在 Windows 上运行 `plexus` 命令时出现错误：
```
Error: spawn npm ENOENT
```

这是因为全局安装的 CLI 无法找到 npm 可执行文件。

## ✅ 解决方案

### 方法一：从源码运行（推荐）

```bash
# 1. 进入 Plexus 目录
cd "d:\MCP project\Plexus"

# 2. 安装依赖（如果还没有）
npm ci

# 3. 构建 core
node ./node_modules/typescript/bin/tsc -p packages/core/tsconfig.json

# 4. 启动 Web Dashboard
cd apps\web
node ../../node_modules/next/dist/bin/next dev --webpack --port 7777
```

或者使用一行命令：
```bash
cd "d:\MCP project\Plexus\apps\web" && node ../../node_modules/next/dist/bin/next dev --webpack --port 7777
```

然后打开浏览器访问：**http://localhost:7777**

### 方法二：使用 npx（如果可用）

```bash
npx -y plexus-agent-config@latest start
```

### 方法三：修复全局安装（高级）

如果想在命令行直接使用 `plexus` 命令：

1. **确保 npm 在 PATH 中**
   ```powershell
   # PowerShell
   $env:Path += ";C:\Program Files\nodejs"

   # 或永久添加
   [Environment]::SetEnvironmentVariable("Path", $env:Path + ";C:\Program Files\nodejs", "User")
   ```

2. **重新安装**
   ```bash
   npm uninstall -g plexus-agent-config
   npm install -g plexus-agent-config
   ```

3. **测试**
   ```bash
   plexus detect
   ```

## 🔧 常见问题

### Q: 出现 "Turbopack is not supported" 错误？

**A**: 使用 webpack 代替：
```bash
next dev --webpack --port 7777
```

### Q: 端口 7777 被占用？

**A**: 更换端口：
```bash
next dev --webpack --port 8080
```

### Q: 找不到 next 命令？

**A**: 使用完整路径：
```bash
node ../../node_modules/next/dist/bin/next dev --webpack --port 7777
```

## 📝 创建快捷方式

为了方便以后使用，可以创建一个批处理文件：

**start-plexus.bat**:
```batch
@echo off
cd /d "d:\MCP project\Plexus\apps\web"
node ../../node_modules/next/dist/bin/next dev --webpack --port 7777
```

双击运行即可启动！

## 🎯 下一步

Dashboard 启动后：

1. 打开浏览器访问 http://localhost:7777
2. 进入 **Team** 页面
3. 点击"加入团队配置"
4. 输入你的团队仓库地址：
   ```
   https://github.com/your-org/team-skills-market.git
   ```
5. 点击"同步配置"

完成！🎉

---

**提示**: 将此文件保存在方便的位置，以备将来参考。
