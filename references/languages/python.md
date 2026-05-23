# Python 代码审查指南

## 基础检查

### 类型注解

- [ ] 函数有返回类型注解
- [ ] 复杂类型使用 `typing` 模块
- [ ] 泛型正确使用 `TypeVar`

```python
# ✅ 推荐
def process_items(items: list[str]) -> dict[str, int]:
    return {item: len(item) for item in items}

# 使用 Protocol 定义结构化类型
from typing import Protocol

class UserRepository(Protocol):
    def get_user(self, user_id: str) -> User | None: ...
```

### 异步编程

- [ ] 异步函数使用 `async def`
- [ ] 异步上下文管理器正确使用 `async with`
- [ ] 避免在同步函数中调用异步函数

---

## 常见问题

### 可变默认参数

```python
# ❌ 避免
def add_item(item: str, items: list = []):
    items.append(item)
    return items

# ✅ 推荐
def add_item(item: str, items: list[str] | None = None):
    if items is None:
        items = []
    items.append(item)
    return items
```

### 异常处理

```python
# ❌ 避免 - 裸 except
try:
    do_something()
except:
    pass

# ✅ 推荐 - 具体异常
try:
    do_something()
except ValueError as e:
    handle_validation_error(e)
except ConnectionError as e:
    handle_connection_error(e)
except Exception as e:
    logger.error(f"Unexpected error: {e}")
    raise  # 重新抛出未知异常
```

---

## 安全检查

### 命令注入

```python
# ❌ 避免
os.system(f"git commit -m '{message}'")

# ✅ 推荐
subprocess.run(["git", "commit", "-m", message])
```

### SQL 注入

```python
# ❌ 避免
query = f"SELECT * FROM users WHERE id = {user_id}"

# ✅ 推荐 - 使用参数化查询
cursor.execute("SELECT * FROM users WHERE id = ?", (user_id,))
```

### 敏感数据

- [ ] 不在日志中输出敏感信息
- [ ] 环境变量管理 secrets
- [ ] 使用 `.env` 而非硬编码

---

## 性能考量

### 数据结构

```python
# ✅ 查找操作使用 set 而非 list
users = {"alice", "bob", "charlie"}
if "alice" in users:  # O(1)

# ❌ 避免
users = ["alice", "bob", "charlie"]
if "alice" in users:  # O(n)
```

### 异步并发

```python
# ✅ 推荐 - 并发执行
import asyncio

async def fetch_all(urls: list[str]) -> list[Response]:
    async with asyncio.TaskGroup() as tg:
        tasks = [fetch(url) for url in urls]
        return [await task for task in tasks]
```

---

## 代码风格

- [ ] 遵循 PEP 8
- [ ] 使用 `black` 格式化
- [ ] 使用 `ruff` 或 `flake8` 检查
- [ ] 类型注解使用 `mypy` 验证

---

## 测试

- [ ] 使用 `pytest`
- [ ] Mock 使用 `@pytest.fixture`
- [ ] 异步测试使用 `pytest-asyncio`