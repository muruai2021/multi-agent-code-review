# Go 代码审查指南

## 基础检查

### 错误处理

```go
// ❌ 避免 - 忽略错误
result, _ := doSomething()

// ✅ 推荐 - 显式处理
result, err := doSomething()
if err != nil {
    return fmt.Errorf("doSomething failed: %w", err)
}
```

### Context 传播

```go
// ❌ 避免 - 未传递 context
func doSomething() error

// ✅ 推荐 - context 作为第一参数
func doSomething(ctx context.Context) error
```

---

## 并发安全

### Goroutine 泄漏

```go
// ❌ 避免 - 可能泄漏
func spawn() {
    go func() {
        time.Sleep(10 * time.Second)
    }()
}

// ✅ 推荐 - 使用 context 取消
func spawn(ctx context.Context) {
    go func() {
        select {
        case <-time.After(10 * time.Second):
        case <-ctx.Done():
            return
        }
    }()
}
```

### Channel 使用

- [ ] 关闭 channel 后不再写入
- [ ] 避免在 channel 上 range 死循环
- [ ] 使用 `sync.WaitGroup` 而非 select 盲目等待

```go
// ✅ 推荐 - WaitGroup
var wg sync.WaitGroup
for _, task := range tasks {
    wg.Add(1)
    go func(t Task) {
        defer wg.Done()
        process(t)
    }(task)
}
wg.Wait()
```

### Map 并发安全

```go
// ❌ 避免 - 非线程安全
var cache = make(map[string]string)

// ✅ 推荐 - 使用 sync.RWMutex 或 sync.Map
var mu sync.RWMutex
var cache = make(map[string]string)

func get(key string) string {
    mu.RLock()
    defer mu.RUnlock()
    return cache[key]
}
```

---

## 依赖管理

- [ ] 使用 Go Modules (`go.mod`)
- [ ] 定期 `go mod tidy`
- [ ] 避免 `replace` 本地路径（生产环境）

---

## 性能考量

### 切片预分配

```go
// ✅ 推荐 - 预分配容量
items := make([]Item, 0, len(expectedItems))
for _, item := range items {
    items = append(items, item)
}
```

### 字符串拼接

```go
// ❌ 避免 - 大量拼接
result := ""
for _, s := range parts {
    result += s
}

// ✅ 推荐 - strings.Builder
var builder strings.Builder
for _, s := range parts {
    builder.WriteString(s)
}
result := builder.String()
```

---

## 代码风格

- [ ] 使用 `go fmt`
- [ ] 使用 `go vet`
- [ ] 遵循 Effective Go 规范
- [ ] 接口设计：小型、专注

### 接口约定

```go
// ✅ 推荐 - 小接口
type Reader interface {
    Read(p []byte) (n int, err error)
}

// 避免大而全的接口
type EverythingDoer interface {
    Read(p []byte) (n int, err error)
    Write(p []byte) (n int, err error)
    Close() error
    // ... 20+ 方法
}
```

---

## 测试

- [ ] 使用标准库 `testing`
- [ ] Table-Driven 测试
- [ ] 使用 `httptest` 测试 HTTP
- [ ] 使用 `assert` 断言库提高可读性