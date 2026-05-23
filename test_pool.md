# 测试用例池

## 单元测试审查测试用例

### TC-001: TypeScript 类型安全检查

**输入代码**:
```typescript
function processData(input: any): string {
  return input.name.toUpperCase();
}
```

**预期发现**:
- `any` 类型使用 → 🟡 important
- `input.name` 可能为 undefined → 🔴 blocking

---

### TC-002: Python 错误处理检查

**输入代码**:
```python
def divide(a, b):
    try:
        return a / b
    except:
        return 0
```

**预期发现**:
- 裸 except 子句 → 🟡 important
- 未处理 `b` 为 0 的情况 → 🟢 nit (边界条件)

---

### TC-003: Go 并发安全检查

**输入代码**:
```go
var counter int

func increment() {
    counter++
}
```

**预期发现**:
- 非线程安全的计数器 → 🔴 blocking

---

### TC-004: SQL 注入检测

**输入代码**:
```typescript
const query = `SELECT * FROM users WHERE id = ${userId}`;
```

**预期发现**:
- SQL 注入漏洞 → 🔴 blocking

---

### TC-005: 复杂度检查

**输入代码**:
```typescript
function complexFunction(a: number, b: number, c: number): number {
  if (a > 0) {
    if (b > 0) {
      if (c > 0) {
        if (a > b) {
          if (b > c) {
            return a + b;
          } else {
            return b + c;
          }
        }
      }
    }
  }
  return 0;
}
```

**预期发现**:
- 嵌套过深（5层）→ 🟡 important
- 可维护性问题 → 🟡 important

---

### TC-006: 测试覆盖率检查

**输入代码**:
```typescript
// user.ts
export function validateEmail(email: string): boolean {
  return email.includes('@');
}

// user.test.ts
test('valid email', () => {
  expect(validateEmail('test@example.com')).toBe(true);
});
```

**预期发现**:
- 缺少边界用例测试 → 🟡 important
- 未测试无效输入

---

### TC-007: 安全认证检查

**输入代码**:
```typescript
app.get('/api/admin', (req, res) => {
  res.json({ secret: 'admin-data' });
});
```

**预期发现**:
- 缺少认证检查 → 🔴 blocking
- 敏感数据泄露风险 → 🔴 blocking

---

### TC-008: 资源泄漏检查

**输入代码**:
```python
def read_file(path: str) -> str:
    f = open(path, 'r')
    return f.read()
```

**预期发现**:
- 文件未关闭 → 🟡 important
- 建议使用 with 语句 → 🟢 nit

---

### TC-009: 性能热点检查

**输入代码**:
```python
def find_duplicates(items: list) -> list:
    duplicates = []
    for i in items:
        for j in items:
            if i == j and i not in duplicates:
                duplicates.append(i)
    return duplicates
```

**预期发现**:
- O(n²) 复杂度 → 🟡 important
- 建议使用 set → 💡 suggestion

---

### TC-010: API 速率限制检查

**输入代码**:
```typescript
app.post('/api/login', (req, res) => {
  const user = authenticate(req.body);
  res.json({ token: generateToken(user) });
});
```

**预期发现**:
- 缺少速率限制 → 🟡 important（可被暴力破解）

---

## 集成测试用例

### TC-201: 多 Agent 协作测试

**场景**: 提交一个包含 TypeScript API 代码的 PR

**预期流程**:
1. 主编分析任务
2. Architecture Reviewer 检查设计
3. Security Reviewer 检查认证和注入
4. Performance Reviewer 检查查询效率
5. Code Quality Reviewer 检查代码风格
6. Testing Reviewer 检查测试覆盖

**预期输出**: 完整的 Markdown 审查报告

---

### TC-202: Go 微服务测试

**场景**: 提交一个包含 Go HTTP 服务的 PR

**预期发现**:
- Context 传播检查
- Goroutine 泄漏检测
- 错误处理完整性

---

## 边界条件测试

### TC-301: 空输入处理

**输入**: 空字符串、空数组、null、undefined

**检查点**:
- 是否有空值检查
- 错误信息是否友好

### TC-302: 超大输入

**输入**: 超长字符串、超大数组

**检查点**:
- 是否有长度限制
- 是否会导致内存问题

### TC-303: 特殊字符

**输入**: SQL 注入、XSS、命令注入尝试

**检查点**:
- 是否正确转义
- 是否有输入验证