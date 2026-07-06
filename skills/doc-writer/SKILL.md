---
name: 技术文档生成器
description: 根据代码自动生成清晰的技术文档，包括 API 文档、README 和架构说明
plexus_id: doc-writer
plexus_enabled_agents:
  - claude-code
  - cursor
  - codex
  - gemini-cli
---

# 技术文档生成器

## 何时使用

- 完成新功能开发，需要编写文档
- 重构代码后，更新相关文档
- 为新项目创建 README
- 生成 API 接口文档
- 编写技术方案或设计文档

## 使用方法

告诉 AI："请使用 doc-writer 为 [文件/模块/项目] 生成文档"。

你可以指定：
- 文档类型（README、API docs、架构文档等）
- 目标读者（开发者、产品经理、用户等）
- 语言偏好（中文/英文）

## 支持的文档类型

### 1. README 文档

适用于项目根目录或子模块。

**包含内容**:
- 项目简介
- 快速开始指南
- 安装步骤
- 使用示例
- 贡献指南
- License

**示例输出**:

```markdown
# Project Name

简短的项目描述（一句话说明这是什么，解决什么问题）

## ✨ 特性

- 🚀 高性能：...
- 🔒 安全可靠：...
- 📦 易于集成：...

## 🚀 快速开始

### 前置要求

- Node.js >= 18
- npm >= 9

### 安装

```bash
npm install project-name
```

### 基础用法

```javascript
import { createClient } from 'project-name';

const client = createClient({
  apiKey: 'your-api-key',
  endpoint: 'https://api.example.com'
});

const result = await client.getData();
console.log(result);
```

## 📖 文档

- [API 参考](docs/api.md)
- [使用指南](docs/guide.md)
- [常见问题](docs/faq.md)

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！详见 [CONTRIBUTING.md](CONTRIBUTING.md)。

## 📄 License

MIT
```

### 2. API 文档

基于代码注释或接口定义生成。

**示例输出**:

```markdown
# API 参考文档

## 认证

所有 API 请求需要在 Header 中包含 API Key：

```
Authorization: Bearer YOUR_API_KEY
```

## 用户管理

### POST /api/users

创建新用户。

**请求体**:

| 字段 | 类型 | 必填 | 说明 |
|------|------|------|------|
| name | string | 是 | 用户姓名 |
| email | string | 是 | 邮箱地址 |
| password | string | 是 | 密码（最少8位） |

**请求示例**:

```json
{
  "name": "张三",
  "email": "zhangsan@example.com",
  "password": "SecurePass123!"
}
```

**响应**:

```json
{
  "id": "usr_123456",
  "name": "张三",
  "email": "zhangsan@example.com",
  "createdAt": "2024-01-01T00:00:00Z"
}
```

**错误码**:

| 状态码 | 说明 |
|--------|------|
| 400 | 参数验证失败 |
| 409 | 邮箱已存在 |
| 429 | 请求过于频繁 |

### GET /api/users/:id

获取用户详情。

**路径参数**:

| 参数 | 类型 | 说明 |
|------|------|------|
| id | string | 用户 ID |

**响应**:

```json
{
  "id": "usr_123456",
  "name": "张三",
  "email": "zhangsan@example.com",
  "role": "user",
  "createdAt": "2024-01-01T00:00:00Z",
  "updatedAt": "2024-01-02T00:00:00Z"
}
```

## 限流策略

- 普通用户：100 请求/分钟
- VIP 用户：1000 请求/分钟

超过限制将返回 `429 Too Many Requests`。
```

### 3. 架构文档

描述系统整体设计和关键决策。

**示例结构**:

```markdown
# 系统架构文档

## 概述

本系统采用微服务架构，包含以下核心服务：

- **API Gateway**: 路由转发、认证、限流
- **User Service**: 用户管理、权限控制
- **Order Service**: 订单处理、支付集成
- **Notification Service**: 邮件、短信通知

## 技术栈

| 层级 | 技术选型 |
|------|----------|
| 前端 | React + TypeScript + Vite |
| 后端 | Node.js + Express |
| 数据库 | PostgreSQL + Redis |
| 消息队列 | RabbitMQ |
| 部署 | Docker + Kubernetes |

## 数据流

```
Client → API Gateway → Service → Database
                ↓
            Redis Cache
                ↓
         Message Queue → Worker
```

## 关键设计决策

### 为什么选择 PostgreSQL？

- ACID 事务支持
- JSONB 类型适合存储灵活配置
- 成熟的生态和工具链

### 缓存策略

- 用户会话：Redis，TTL 24h
- API 响应：Redis，TTL 5min
- 静态资源：CDN

## 扩展性考虑

- 水平扩展：无状态服务，可动态增减实例
- 数据库读写分离：主库写，从库读
- 异步处理：耗时操作通过消息队列解耦
```

### 4. 代码注释文档

为复杂函数或类生成 JSDoc/TSDoc 注释。

**示例**:

```typescript
/**
 * 创建并验证用户账户
 *
 * @param userData - 用户注册信息
 * @param userData.name - 用户姓名（2-50个字符）
 * @param userData.email - 邮箱地址（需符合 RFC 5322）
 * @param userData.password - 密码（最少8位，包含大小写字母和数字）
 *
 * @returns 创建成功的用户对象（不包含密码）
 *
 * @throws {ValidationError} 当参数验证失败时
 * @throws {ConflictError} 当邮箱已存在时
 * @throws {DatabaseError} 当数据库操作失败时
 *
 * @example
 * ```typescript
 * const user = await createUser({
 *   name: '张三',
 *   email: 'zhangsan@example.com',
 *   password: 'SecurePass123!'
 * });
 * console.log(user.id); // "usr_123456"
 * ```
 *
 * @see {@link validateEmail} 邮箱验证逻辑
 * @see {@link hashPassword} 密码哈希处理
 */
async function createUser(userData: CreateUserInput): Promise<User> {
  // ...
}
```

## 最佳实践

### 文档写作原则

1. **简洁明了**: 用最少的文字表达清楚
2. **示例驱动**: 每个功能都应有代码示例
3. **保持更新**: 代码变更时同步更新文档
4. **面向读者**: 根据目标读者调整技术深度
5. **结构化**: 使用标题、列表、表格提高可读性

### Markdown 技巧

- 使用 emoji 增加视觉层次（但不过度）
- 代码块指定语言以启用语法高亮
- 使用表格对比不同选项
- 重要信息用 **加粗** 或 > 引用块突出

### 自动化工具推荐

- **TypeDoc**: TypeScript 文档生成
- **JSDoc**: JavaScript 文档生成
- **Swagger/OpenAPI**: API 文档标准
- **Docusaurus**: 文档网站框架
- **MkDocs**: Python 文档工具

## 注意事项

- 不要暴露敏感信息（API keys、内部地址等）
- 避免过时的截图，优先使用文字描述
- 外部链接应定期检查有效性
- 多语言项目应考虑国际化（i18n）

## 相关资源

- [Markdown 官方指南](https://daringfireball.net/projects/markdown/)
- [Google Developer Documentation Style Guide](https://developers.google.com/style)
- [Documentation Driven Development](https://tom.preston-werner.com/)
