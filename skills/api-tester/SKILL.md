---
name: API 测试助手
description: 自动生成和执行 API 测试用例，包括单元测试和集成测试
plexus_id: api-tester
plexus_enabled_agents:
  - claude-code
  - cursor
  - codex
---

# API 测试助手

## 何时使用

- 开发新的 API 端点后
- 修改现有 API 逻辑时
- 需要验证 API 边界情况
- 编写接口文档时需要同步生成测试

## 使用方法

告诉 AI："请使用 api-tester 为 [API 名称/文件] 生成测试"。

AI 会：
1. 分析 API 定义（路由、参数、响应）
2. 生成全面的测试用例
3. 提供可直接运行的测试代码

## 测试覆盖范围

### 基础测试
- ✅ 正常请求（Happy Path）
- ❌ 无效参数
- 🔐 认证/授权检查
- ⏱️ 超时处理

### 边界情况
- 空值/null 处理
- 极值测试（最大值、最小值）
- 特殊字符注入
- 并发请求

### 性能测试
- 响应时间验证
- 大数据量处理
- 内存使用情况

## 输出格式

根据项目技术栈生成对应测试代码：

### Node.js + Jest 示例

```typescript
import request from 'supertest';
import app from '../app';

describe('POST /api/users', () => {
  // 正常创建用户
  it('should create a new user successfully', async () => {
    const response = await request(app)
      .post('/api/users')
      .send({
        name: 'John Doe',
        email: 'john@example.com',
        password: 'SecurePass123!'
      })
      .expect(201);

    expect(response.body).toHaveProperty('id');
    expect(response.body.name).toBe('John Doe');
    expect(response.body.password).toBeUndefined(); // 不应返回密码
  });

  // 缺少必填字段
  it('should return 400 when required fields are missing', async () => {
    const response = await request(app)
      .post('/api/users')
      .send({ name: 'John' })
      .expect(400);

    expect(response.body.error).toContain('email is required');
  });

  // 邮箱格式无效
  it('should return 400 for invalid email format', async () => {
    const response = await request(app)
      .post('/api/users')
      .send({
        name: 'John',
        email: 'invalid-email',
        password: 'pass123'
      })
      .expect(400);
  });

  // 重复注册
  it('should return 409 when email already exists', async () => {
    // 先创建一个用户
    await request(app).post('/api/users').send({
      name: 'Existing User',
      email: 'exists@example.com',
      password: 'pass123'
    });

    // 尝试再次创建
    const response = await request(app)
      .post('/api/users')
      .send({
        name: 'Another User',
        email: 'exists@example.com',
        password: 'pass456'
      })
      .expect(409);

    expect(response.body.error).toContain('Email already registered');
  });
});
```

### Python + pytest 示例

```python
import pytest
from fastapi.testclient import TestClient
from app.main import app

client = TestClient(app)

def test_create_user_success():
    """正常创建用户"""
    response = client.post("/api/users", json={
        "name": "John Doe",
        "email": "john@example.com",
        "password": "SecurePass123!"
    })

    assert response.status_code == 201
    data = response.json()
    assert "id" in data
    assert data["name"] == "John Doe"
    assert "password" not in data

def test_create_user_missing_fields():
    """缺少必填字段"""
    response = client.post("/api/users", json={"name": "John"})

    assert response.status_code == 400
    assert "email" in response.json()["detail"]

def test_create_user_invalid_email():
    """无效邮箱格式"""
    response = client.post("/api/users", json={
        "name": "John",
        "email": "not-an-email",
        "password": "pass123"
    })

    assert response.status_code == 422

@pytest.mark.asyncio
async def test_concurrent_requests():
    """并发请求测试"""
    import asyncio

    tasks = []
    for i in range(10):
        task = asyncio.create_task(
            client.post("/api/users", json={
                "name": f"User {i}",
                "email": f"user{i}@test.com",
                "password": "pass123"
            })
        )
        tasks.append(task)

    responses = await asyncio.gather(*tasks)

    # 所有请求都应该成功或适当限流
    for resp in responses:
        assert resp.status_code in [201, 429]  # 201 成功, 429 限流
```

## 高级用法

### Mock 外部依赖

```typescript
jest.mock('../services/emailService', () => ({
  sendVerificationEmail: jest.fn().mockResolvedValue(true),
}));

it('should send verification email after registration', async () => {
  const { sendVerificationEmail } = require('../services/emailService');

  await request(app).post('/api/users').send(userData);

  expect(sendVerificationEmail).toHaveBeenCalledWith(
    expect.stringContaining('@')
  );
});
```

### 数据库清理

```typescript
beforeEach(async () => {
  await db.collection('users').deleteMany({});
});

afterAll(async () => {
  await db.close();
});
```

### 性能断言

```typescript
it('should respond within 200ms', async () => {
  const start = Date.now();

  await request(app).get('/api/users').expect(200);

  const duration = Date.now() - start;
  expect(duration).toBeLessThan(200);
});
```

## 注意事项

- 测试数据应使用 faker 库生成，避免硬编码
- 不要在测试中暴露真实 API keys 或敏感信息
- 每个测试用例应该独立，不依赖其他测试的执行顺序
- 使用 `describe` 和 `it` 清晰描述测试意图
- CI/CD 中应包含测试覆盖率要求（建议 >80%）

## 相关工具

- **Jest**: JavaScript/TypeScript 测试框架
- **Supertest**: HTTP 断言库
- **pytest**: Python 测试框架
- **Postman**: 手动 API 测试
- **k6**: 负载测试

## 参考资源

- [Jest 官方文档](https://jestjs.io/)
- [Testing Library](https://testing-library.com/)
- [API Testing Best Practices](https://swagger.io/docs/open-source-inspector/api-testing-best-practices/)
