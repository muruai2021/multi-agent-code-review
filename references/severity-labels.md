# 严重性标签系统

## 标签定义

| 标签 | 符号 | 含义 | 行动 | 合并阻塞 |
|------|------|------|------|----------|
| `blocking` | 🔴 | 必须修复 | 阻止合并 | ✅ |
| `important` | 🟡 | 应当修复 | 建议修改 | ❌ |
| `nit` | 🟢 | 最好修复 | 非阻塞 | ❌ |
| `suggestion` | 💡 | 替代方案 | 考虑 | ❌ |
| `learning` | 📚 | 教育评论 | 无需操作 | ❌ |
| `praise` | 🎉 | 表扬 | 继续保持 | ❌ |

---

## 使用场景

### 🔴 [blocking] - 必须修复

**场景**:
- 安全漏洞（SQL 注入、XSS、CSRF）
- 崩溃风险（空指针、数组越界）
- 数据丢失风险
- 认证/授权漏洞
- 严重的性能问题

**示例**:
```markdown
🔴 [blocking] SQL注入漏洞
- 位置: `src/db/queries.ts:42`
- 问题: 用户输入直接拼接到 SQL 查询中
- 风险: 攻击者可执行任意 SQL 命令
- 建议: 使用参数化查询
```

---

### 🟡 [important] - 应当修复

**场景**:
- 逻辑错误但不会崩溃
- 错误处理不完整
- 可维护性问题
- 测试覆盖率不足
- 重复代码

**示例**:
```markdown
🟡 [important] 错误处理不完整
- 位置: `src/service/user.ts:78`
- 问题: 只捕获了 Exception，未处理 ConnectionError
- 建议: 增加多类型异常处理
```

---

### 🟢 [nit] - 非阻塞建议

**场景**:
- 代码风格不一致
- 命名不规范
- 注释不足
- 小型重构机会

**示例**:
```markdown
🟢 [nit] 变量命名可改进
- 位置: `src/utils/helper.ts:15`
- 当前: `x`
- 建议: `userId` 更有表达力
```

---

### 💡 [suggestion] - 替代方案

**场景**:
- 有更优的实现方式
- 可使用更现代的语法
- 可利用语言特性简化

**示例**:
```markdown
💡 [suggestion] 可使用解构赋值
- 位置: `src/components/UserCard.tsx:23`
- 当前: `const name = props.user.name; const age = props.user.age;`
- 建议: `const { name, age } = props.user;`
```

---

### 📚 [learning] - 教育评论

**场景**:
- 分享相关最佳实践
- 解释为什么某种写法更好
- 指向学习资源

**示例**:
```markdown
📚 [learning] 关于 Promise vs async/await
- 位置: `src/api/client.ts:56`
- 背景: async/await 通常比 .then() 更易读
- 参考: https://javascript.info/async-await
```

---

### 🎉 [praise] - 表扬

**场景**:
- 优秀的解决方案
- 创新的实现
- 超出了预期

**示例**:
```markdown
🎉 [praise] 优雅的缓存实现
- 位置: `src/cache/redis.ts:42`
- 这个缓存失效策略实现得非常巧妙！
```

---

## 输出格式示例

```markdown
## Review Summary

| Severity | Count |
|----------|-------|
| 🔴 blocking | 1 |
| 🟡 important | 3 |
| 🟢 nit | 5 |
| 💡 suggestion | 2 |
| 📚 learning | 1 |
| 🎉 praise | 1 |

### Blocking Issues

1. [🔴 SQL注入 - src/db/queries.ts:42](#)
2. [🔴 认证绕过 - src/auth/middleware.ts:15](#)

### Important Issues

1. [🟡 错误处理不完整 - src/service/user.ts:78](#)
2. [🟡 缺少单元测试 - src/utils/helper.ts](#)
3. [🟡 重复代码 - src/components/*.tsx](#)
```