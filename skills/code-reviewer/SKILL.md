---
name: 代码审查助手
description: 自动审查代码质量，提供改进建议和安全检查
plexus_id: code-reviewer
plexus_enabled_agents:
  - claude-code
  - cursor
  - codex
  - gemini-cli
  - qwen-code
---

# 代码审查助手

## 何时使用

- 完成功能开发后，提交代码前
- 重构代码后，确保没有引入新问题
- Code Review 环节，需要系统性检查
- 学习他人代码时，快速理解质量要点

## 使用方法

告诉 AI："请使用 code-reviewer 审查我的代码"，然后指定文件或目录。

AI 会从以下维度进行审查：

### 1. 代码质量
- 函数长度和复杂度
- 变量命名是否清晰
- 代码重复和冗余
- 嵌套深度（建议不超过 3 层）

### 2. 性能优化
- 不必要的循环或递归
- 内存泄漏风险
- 数据库查询优化
- 缓存使用合理性

### 3. 安全性
- 敏感信息硬编码
- SQL 注入风险
- XSS 攻击防护
- 输入验证缺失

### 4. 最佳实践
- 错误处理是否完善
- 日志记录是否充分
- 类型定义是否准确
- 注释是否清晰

## 输出格式

```
🔍 代码审查报告 | <文件名>

✅ 优点:
- [列出做得好的地方]

⚠️ 需要改进:
- [严重] <问题描述> at line X
- [建议] <问题描述> at line Y

💡 优化建议:
- [具体的改进方案]

📊 评分: <1-10分>
```

## 示例

**用户**: "请审查 src/auth/login.ts"

**AI**:
```
🔍 代码审查报告 | src/auth/login.ts

✅ 优点:
- 使用了 TypeScript 严格类型定义
- 密码哈希处理正确（bcrypt）
- 有完善的错误处理机制

⚠️ 需要改进:
- [严重] JWT secret 应从环境变量读取，不要硬编码 at line 15
- [严重] 缺少登录失败次数限制，可能被暴力破解 at line 23
- [建议] 添加 token 过期时间验证 at line 45
- [建议] 日志中不应记录完整用户对象 at line 67

💡 优化建议:
1. 将 JWT_SECRET 移到 .env 文件：
   ```typescript
   const secret = process.env.JWT_SECRET;
   if (!secret) throw new Error('JWT_SECRET is required');
   ```

2. 添加登录限流：
   ```typescript
   import rateLimit from 'express-rate-limit';
   const loginLimiter = rateLimit({
     windowMs: 15 * 60 * 1000, // 15分钟
     max: 5 // 最多5次尝试
   });
   ```

📊 评分: 7/10
```

## 注意事项

- 审查结果仅供参考，最终决策由开发者判断
- 对于遗留代码，优先关注安全性和严重问题
- 团队规范优先于通用最佳实践
- 如与团队 ESLint/Prettier 规则冲突，以工具配置为准

## 相关资源

- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [Clean Code 原则](https://gist.github.com/wojteklu/73c6914cc446146b8b533c0988cf8d29)
- 团队代码规范文档（见团队 Wiki）
