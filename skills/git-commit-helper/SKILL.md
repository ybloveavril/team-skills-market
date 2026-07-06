---
name: Git 提交助手
description: 生成规范的 Git 提交信息，遵循 Conventional Commits 标准
plexus_id: git-commit-helper
plexus_enabled_agents:
  - claude-code
  - cursor
  - codex
  - gemini-cli
  - qwen-code
---

# Git 提交助手

## 何时使用

- 完成代码修改，准备提交时
- 不确定如何写清晰的 commit message
- 团队要求遵循 Conventional Commits 规范
- 需要自动生成 changelog

## 使用方法

告诉 AI："请使用 git-commit-helper 为当前更改生成提交信息"。

AI 会：
1. 分析 `git diff` 或暂存区的更改
2. 识别修改类型（feat、fix、docs 等）
3. 生成符合规范的 commit message

## Conventional Commits 规范

### 基本格式

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Type 类型

| Type | 说明 | 示例 |
|------|------|------|
| `feat` | 新功能 | `feat(auth): add JWT login` |
| `fix` | Bug 修复 | `fix(api): handle null pointer in user endpoint` |
| `docs` | 文档更新 | `docs(readme): update installation guide` |
| `style` | 代码格式（不影响逻辑） | `style: fix indentation` |
| `refactor` | 重构 | `refactor(utils): simplify date formatting` |
| `perf` | 性能优化 | `perf(query): add database index` |
| `test` | 测试相关 | `test(auth): add login failure cases` |
| `chore` | 构建/工具链 | `chore: upgrade typescript to 5.0` |
| `ci` | CI/CD 配置 | `ci: add coverage report` |

### Scope（可选）

表示修改影响的范围，如：
- 模块名：`auth`、`api`、`ui`
- 文件名：`user.ts`、`config.yaml`
- 功能点：`login`、`payment`

### Subject

- 使用祈使句（add 而不是 added/adds）
- 首字母小写
- 不超过 50 个字符
- 结尾不加句号

### Body（可选）

- 解释**为什么**做这个修改，而不是做了什么
- 每行不超过 72 个字符
- 可以与 subject 之间空一行

### Footer（可选）

用于：
- Breaking Changes：以 `BREAKING CHANGE:` 开头
- 引用 Issue：`Closes #123`、`Fixes #456`

## 输出示例

### 示例 1：新功能

```
feat(auth): add social login with GitHub

Implement OAuth2 flow for GitHub authentication.
Users can now link their GitHub account and login
with one click.

Added:
- GitHub OAuth provider configuration
- Social account linking in user profile
- Login button in UI

Closes #78
```

### 示例 2：Bug 修复

```
fix(api): handle empty result in user search

The API returned 500 when search query matched
no users. Now returns 200 with empty array.

Added null check before mapping results.

Fixes #142
```

### 示例 3：Breaking Change

```
refactor(config): migrate to environment-based config

Replace hardcoded configuration with environment
variables. This allows different settings for
dev/staging/prod environments.

BREAKING CHANGE: Config file format changed.
Old format (config.json) is no longer supported.
Use .env file instead. See README for migration guide.

Refs: ARCH-001
```

### 示例 4：简单修改

```
docs(readme): fix typo in installation section
```

```
style: remove unused imports
```

## 完整工作流程

### Step 1: 查看更改

```bash
git status
git diff --staged
```

### Step 2: 让 AI 生成 commit message

告诉 AI："请根据以下更改生成 commit message"，然后粘贴 `git diff` 输出。

或者直接使用：

```bash
# 如果使用 Claude Code 或 Cursor
git add .
# 然后让 AI 助手生成提交信息
```

### Step 3: 提交

```bash
git commit -m "feat(auth): add JWT login" -m "Implemented OAuth2 flow..."
```

或使用多行提交：

```bash
git commit
# 在编辑器中粘贴生成的完整 message
```

## 自动化脚本（可选）

创建 `.husky/commit-msg` hook 验证格式：

```bash
#!/bin/sh

commit_msg=$(cat "$1")

# 检查是否符合 Conventional Commits
if ! echo "$commit_msg" | grep -qE '^(feat|fix|docs|style|refactor|perf|test|chore|ci)(\(.+\))?: .+'; then
  echo "❌ Commit message 不符合 Conventional Commits 规范"
  echo "格式: <type>(<scope>): <subject>"
  echo ""
  echo "例如: feat(auth): add login button"
  exit 1
fi

echo "✅ Commit message 格式正确"
```

## VS Code 插件推荐

- **Conventional Commits**: 智能提示 commit type
- **GitLens**: 增强 Git 功能
- **Commit Message Editor**: 方便的编辑器

## CLI 工具

### commitizen

交互式生成 commit message：

```bash
npm install -g commitizen
cz
```

### commitlint

验证 commit message：

```bash
npm install -g @commitlint/cli
commitlint check
```

## 最佳实践

1. **原子提交**: 一个 commit 只做一件事
2. **及时提交**: 完成一个小功能就提交
3. **清晰描述**: 让别人（和未来的你）能理解为什么这样改
4. **引用 Issue**: 关联到任务追踪系统
5. **保持一致**: 团队统一使用相同规范

## 常见错误

❌ **不清晰的 message**:
```
update code
fix bug
wip
```

✅ **清晰的 message**:
```
fix(api): validate email format in user registration
feat(search): add full-text search with Elasticsearch
docs(readme): add API usage examples
```

❌ **混合多个改动**:
```
feat: add login, fix logout bug, update docs
```

✅ **分开提交**:
```
feat(auth): add login functionality
fix(auth): resolve session cleanup on logout
docs(auth): document authentication flow
```

## 生成 Changelog

使用 [conventional-changelog](https://github.com/conventional-changelog/conventional-changelog) 自动生成：

```bash
npm install -g conventional-changelog-cli
conventional-changelog -p angular -i CHANGELOG.md -s
```

## 相关资源

- [Conventional Commits 官方规范](https://www.conventionalcommits.org/)
- [Angular Commit Guidelines](https://github.com/angular/angular/blob/main/CONTRIBUTING.md#commit)
- [Keep a Changelog](https://keepachangelog.com/)
