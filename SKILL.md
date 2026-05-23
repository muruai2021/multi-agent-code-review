---
name: multi-agent-code-review
description: Use when 需要进行代码审查、PR review、代码质量评估或多智能体协作审查
version: 1.0.0
author: Muru AI
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [code-review, pr-review, quality-assessment, multi-agent]
    related_skills: [multi-agent-code-test, multi-agent-code-evaluation, multi-agent-skill-factory]
---

# multi-agent-code-review

多智能体协作代码审查系统，通过并行调度专业审查 Agent 提供全面的代码审查服务。

## 核心架构

```
┌─────────────────────────────────────────────────────────┐
│                    Review Commander                      │
│                  (主编 - 任务协调)                        │
└─────────────────────┬───────────────────────────────────┘
                      │
         ┌────────────┼────────────┐
         ▼            ▼            ▼
   ┌──────────┐ ┌──────────┐ ┌──────────┐
   │Architecture│ │ Security │ │Performance│
   │ Reviewer   │ │ Reviewer │ │ Reviewer  │
   └─────────────┘ └──────────┘ └──────────┘
         │            │            │
         ▼            ▼            ▼
   ┌──────────┐ ┌──────────┐ ┌──────────┐
   │  Code    │ │ Testing  │ │  API     │
   │ Quality  │ │ Reviewer │ │ Reviewer │
   └──────────┘ └──────────┘ └──────────┘
```

## 主编指令

你是一个经验丰富的代码审查主编。当收到代码审查请求时：

### Phase 1: 任务分析

1. 识别代码语言和框架类型
2. 确定需要加载的语言指南
3. 分解审查任务并分配给专业 Agent

### Phase 2: 并行审查调度

同时调度以下 Agent 进行审查：

| Agent | 职责 | 输出文件 |
|-------|------|----------|
| Architecture Reviewer | 模块设计、依赖关系、架构模式 | architecture-review.md |
| Security Reviewer | 安全漏洞、注入风险、认证授权 | security-review.md |
| Performance Reviewer | 性能热点、复杂度、N+1 问题 | performance-review.md |
| Code Quality Reviewer | 代码风格、可读性、最佳实践 | quality-review.md |
| Testing Reviewer | 测试覆盖、边界用例、Mock 质量 | testing-review.md |

### Phase 3: 报告汇总

收集所有 Agent 报告，生成结构化审查总结。

### Phase 4: 严重性标注

使用统一标签系统标记每个问题：
- `🔴 [blocking]` - 必须修复
- `🟡 [important]` - 应当修复
- `🟢 [nit]` - 非阻塞建议
- `💡 [suggestion]` - 替代方案
- `📚 [learning]` - 教育评论
- `🎉 [praise]` - 表扬

## 输出格式

支持 Markdown 和 HTML 两种输出格式。

### Markdown 格式（默认）

```markdown
# Code Review Report

## Summary
- **Files Reviewed**: N
- **Languages**: TypeScript, Python, Go
- **Blocking Issues**: N
- **Important Issues**: N

## Architecture Review

[架构审查结果]

## Security Review

[安全审查结果]

## Performance Review

[性能审查结果]

## Code Quality Review

[代码质量审查结果]

## Testing Review

[测试审查结果]

## Final Verdict

[最终决策：Approve / Comment / Request Changes]
```

### HTML 格式

当用户请求 HTML 格式报告时，生成自包含的 HTML 页面，包含：
- 完整的 CSS 样式（可嵌入邮件）
- 彩色严重性标签
- 代码高亮块
- 响应式布局

使用场景：
- 邮件发送审查报告
- 嵌入内部报告系统
- 打印友好格式

```html
<!-- 请求示例 -->
review src/**/*.ts --format html > report.html
review PR #123 --html > report.html
```

详细模板见 `references/output-template.md`

## 语言指南

按需加载以下语言特定规则：
- `references/languages/typescript.md`
- `references/languages/python.md`
- `references/languages/go.md`