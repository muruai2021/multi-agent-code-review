# TypeScript 代码审查指南

## 基础检查

### 类型安全

- [ ] 禁止使用 `any`，使用 `unknown` + 类型守卫
- [ ] 启用 `strict: true`
- [ ] `noUncheckedIndexedAccess` 启用
- [ ] 泛型约束正确使用

### 常见问题

```typescript
// ❌ 避免
const data: any = response;

// ✅ 推荐
const data: unknown = response;
if (isUserResponse(data)) {
  // 使用 data
}

// ❌ 避免 - 可变默认参数
function addItem(item: string, list: string[] = []) {
  list.push(item);
  return list;
}

// ✅ 推荐
function addItem(item: string, list: string[] = []): string[] {
  return [...list, item];
}
```

---

## TypeScript 特有检查

### 泛型

```typescript
// ❌ 避免 - 缺少约束
function getProperty<T, K>(obj: T, key: K) {
  return obj[key]; // Error
}

// ✅ 推荐 - 正确约束
function getProperty<T extends object, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}
```

### 条件类型

```typescript
// 检查是否正确使用条件类型
type NonNullable<T> = T extends null | undefined ? never : T;
```

### 装饰器

- [ ] 装饰器语法符合实验规范要求
- [ ] 类属性装饰器正确使用

---

## React/Next.js 特定

### Hooks 规则

- [ ] `useEffect` 依赖数组正确
- [ ] 不在循环中使用 Hooks
- [ ] 自定义 Hooks 以 `use` 开头

### TypeScript 与 React

```typescript
// ❌ 避免 - any 类型 props
interface Props {
  user: any;
}

// ✅ 推荐 - 具体类型
interface User {
  id: string;
  name: string;
}

interface Props {
  user: User;
}
```

---

## 模块系统

- [ ] 使用 ES 模块语法
- [ ] 导出类型而非实现细节
- [ ] barrel export (`index.ts`) 合理使用

---

## 错误处理

```typescript
// ✅ 推荐 - 完整的错误类型
try {
  await fetchData();
} catch (error) {
  if (error instanceof NetworkError) {
    // 处理网络错误
  } else if (error instanceof ValidationError) {
    // 处理验证错误
  } else {
    // 未知错误
    throw error;
  }
}
```

---

## 性能考量

- [ ] 避免不必要的类型转换
- [ ] 合理使用 `readonly` 修饰符
- [ ] 避免在渲染中创建新函数/对象

---

## 框架特定 (NestJS)

### 依赖注入

- [ ] 模块导入正确
- [ ] 服务作用域合理（default/request）
- [ ] 避免循环依赖

### DTO 和验证

- [ ] DTO 有完整类型定义
- [ ] 使用 class-validator 进行验证
- [ ] 避免 any 类型