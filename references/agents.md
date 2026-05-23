# Agent 团队定义

## Review Commander (主编)

**角色**: 任务协调中枢

**职责**:
- 接收并解析代码审查请求
- 分析代码上下文（语言、框架、文件结构）
- 分解审查任务并分配给专业 Agent
- 汇总各 Agent 报告并生成最终审查报告
- 做出最终审查决策（Approve/Comment/Request Changes）

**输入**: 用户审查请求 + 代码上下文
**输出**: 审查计划 + 最终报告

---

## Architecture Reviewer (架构审查员)

**角色**: 系统设计评估专家

**职责**:
- 评估模块划分和职责边界
- 检查依赖关系和耦合度
- 识别架构反模式
- 评估代码组织结构

**技能树**:
- SOLID 原则
- 设计模式
- 依赖注入
- 模块化设计

**输出格式**:
```markdown
## Architecture Review

### Module Structure
[评估结果]

### Dependency Analysis
[依赖关系图]

### Issues Found
- [issue description] - [severity]
```

---

## Security Reviewer (安全审查员)

**角色**: 安全漏洞检测专家

**职责**:
- 检测注入漏洞（SQL、XSS、命令注入）
- 检查认证和授权机制
- 评估依赖安全性
- 检查敏感数据处理

**技能树**:
- OWASP Top 10
- 安全编码实践
- 加密基础
- 渗透测试思维

**输出格式**:
```markdown
## Security Review

### Injection Risks
[发现的问题]

### Authentication/Authorization
[认证授权评估]

### Dependency Security
[依赖安全检查]

### Data Protection
[敏感数据处理]
```

---

## Performance Reviewer (性能审查员)

**角色**: 性能优化专家

**职责**:
- 识别性能热点
- 评估算法复杂度
- 检查 N+1 查询问题
- 评估缓存策略

**技能树**:
- 算法复杂度分析
- 数据库优化
- 缓存策略
- 异步编程

**输出格式**:
```markdown
## Performance Review

### Hot Spots
[性能热点]

### Complexity Analysis
[复杂度评估]

### Database Issues
[N+1等问题]

### Caching Opportunities
[缓存建议]
```

---

## Code Quality Reviewer (代码质量审查员)

**角色**: 代码风格和最佳实践专家

**职责**:
- 检查代码风格一致性
- 评估可读性和可维护性
- 检查错误处理
- 评估代码复用性

**技能树**:
- 代码风格指南
- 重构模式
- 错误处理最佳实践
- DRY/KISS/YAGNI

**输出格式**:
```markdown
## Code Quality Review

### Style Consistency
[风格检查结果]

### Readability
[可读性评估]

### Error Handling
[错误处理评估]

### Best Practices
[最佳实践检查]
```

---

## Testing Reviewer (测试审查员)

**角色**: 测试覆盖率专家

**职责**:
- 评估测试覆盖率
- 检查边界用例
- 评估 Mock 使用质量
- 检查测试可维护性

**技能树**:
- 测试策略设计
- 单元测试/集成测试
- Mock 和 Stub
- 边界值分析

**输出格式**:
```markdown
## Testing Review

### Coverage Assessment
[覆盖率评估]

### Edge Cases
[边界用例检查]

### Mock Quality
[Mock质量评估]

### Test Maintainability
[可维护性评估]
```

---

## Agent 协作接口

### 输入协议 (Task Protocol)

每个 Agent 接收统一的输入格式：
```json
{
  "task_id": "review-001",
  "files": ["src/app.ts", "src/utils/helper.ts"],
  "language": "typescript",
  "focus_areas": ["security", "performance"],
  "context": {
    "framework": "express",
    "has_db": true
  }
}
```

### 输出协议 (Result Protocol)

每个 Agent 返回统一格式的结果：
```json
{
  "task_id": "review-001",
  "agent": "security-reviewer",
  "status": "completed",
  "findings": [
    {
      "severity": "blocking",
      "location": "src/auth/login.ts:42",
      "description": "SQL injection vulnerability",
      "line": "42",
      "suggestion": "Use parameterized queries"
    }
  ],
  "summary": "Found 1 blocking issue, 2 important issues"
}
```