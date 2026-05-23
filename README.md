# multi-agent-code-review

[English](#english) | [中文](#中文)

---

## English

### Overview

**multi-agent-code-review** is an intelligent multi-agent collaborative code review system designed for AI coding assistants and development teams. It leverages a parallel agent architecture where specialized reviewers work simultaneously to provide comprehensive, multi-dimensional code analysis.

Unlike traditional single-reviewer code review, this system coordinates multiple expert agents—each focused on a specific domain—to deliver thorough coverage across architecture, security, performance, code quality, and testing.

### Key Features

#### Multi-Agent Parallel Review
- **Review Commander**: Task coordination and report aggregation
- **Architecture Reviewer**: System design, module organization, dependency analysis
- **Security Reviewer**: Vulnerability detection, injection prevention, auth/authorization
- **Performance Reviewer**: Hotspot identification, complexity analysis, N+1 detection
- **Code Quality Reviewer**: Style consistency, readability, best practices
- **Testing Reviewer**: Coverage assessment, edge cases, mock quality

#### 4-Phase Review Process
1. **Context Gathering** - Understand business context and scope
2. **High-Level Review** - Architecture and design assessment
3. **Line-by-Line Review** - Deep analysis of every code change
4. **Summary & Decision** - Structured report with verdict

#### Dual Output Format
- **Markdown**: GitHub PR comments, documentation, easy to share
- **HTML**: Embedded reports, email-friendly, print-ready

#### Severity Tagging System
| Tag | Meaning | Action |
|-----|---------|--------|
| 🔴 [blocking] | Critical issue | Must fix before merge |
| 🟡 [important] | Significant issue | Should fix |
| 🟢 [nit] | Minor issue | Non-blocking suggestion |
| 💡 [suggestion] | Alternative approach | Consider |
| 📚 [learning] | Educational comment | No action needed |
| 🎉 [praise] | Well done | Keep it up |

### Supported Languages

| Language | Framework Coverage |
|----------|-------------------|
| TypeScript | React, Vue, Angular, NestJS, Next.js |
| Python | Django, FastAPI, Flask, AsyncIO |
| Go | Standard library, Gin, Echo, gRPC |

### Usage

**Activate via command:**
```
/review <files or PR URL>
```

**Activate via description:**
```
"帮我审查这些代码"
"review the PR for security issues"
"审查这个函数的性能和安全性"
```

**Output format options:**
```
review src/**/*.ts --format markdown    # Default
review src/**/*.ts --format html        # HTML report
```

### Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                      Review Commander                        │
│                 (Task Coordination & Reporting)               │
└───────────────────────────┬─────────────────────────────────┘
                            │
         ┌──────────────────┼──────────────────┐
         ▼                  ▼                  ▼
  ┌────────────┐     ┌────────────┐     ┌────────────┐
  │Architecture│     │  Security  │     │Performance │
  │  Reviewer  │     │  Reviewer  │     │  Reviewer  │
  └────────────┘     └────────────┘     └────────────┘
         │                  │                  │
         ▼                  ▼                  ▼
  ┌────────────┐     ┌────────────┐     ┌────────────┐
  │   Code     │     │  Testing   │     │    API     │
  │  Quality   │     │  Reviewer  │     │  Reviewer  │
  └────────────┘     └────────────┘     └────────────┘
```

### Project Structure

```
multi-agent-code-review/
├── SKILL.md                      # Main entry point
├── README.md                     # This file
├── test_pool.md                  # Test cases
└── references/
    ├── agents.md                 # Agent definitions
    ├── pipeline.md               # Collaboration workflow
    ├── review-phases.md          # 4-phase review process
    ├── severity-labels.md         # Tagging system
    ├── output-template.md        # Markdown & HTML templates
    └── languages/
        ├── typescript.md         # TypeScript guidelines
        ├── python.md             # Python guidelines
        └── go.md                 # Go guidelines
```

### Review Decision

| Condition | Decision |
|-----------|----------|
| Has 🔴 blocking issues | **Request Changes** |
| Has 🟡 important issues | **Comment** |
| Only 🟢 nit or 💡 suggestion | **Approve** |
| Exceptional quality | **Approve with Praise** |

---

## 中文

### 概述

**multi-agent-code-review** 是一款智能多智能体协作代码审查系统，专为 AI 编程助手和开发团队设计。它采用并行智能体架构，让专业审查员同时工作，从架构、安全、性能、代码质量和测试等多个维度提供全面的代码分析。

不同于传统的单一审查员代码审查，本系统协调多个专家智能体——每个专注于特定领域——提供彻底、全面的代码审查覆盖。

### 核心功能

#### 多智能体并行审查
- **主编 (Review Commander)**: 任务协调与报告汇总
- **架构审查员 (Architecture Reviewer)**: 系统设计、模块组织、依赖分析
- **安全审查员 (Security Reviewer)**: 漏洞检测、注入防护、认证授权
- **性能审查员 (Performance Reviewer)**: 热点识别、复杂度分析、N+1 检测
- **代码质量审查员 (Code Quality Reviewer)**: 风格一致性、可读性、最佳实践
- **测试审查员 (Testing Reviewer)**: 覆盖率评估、边界用例、Mock 质量

#### 四阶段审查流程
1. **上下文收集** - 理解业务背景和审查范围
2. **高层审查** - 架构和设计评估
3. **逐行审查** - 深度分析每一处代码变更
4. **总结决策** - 生成结构化报告并给出 verdict

#### 双输出格式
- **Markdown**: GitHub PR 评论、文档、易于分享
- **HTML**: 内嵌报告、邮件友好、打印就绪

#### 严重性标签系统
| 标签 | 含义 | 行动 |
|------|------|------|
| 🔴 [blocking] | 严重问题 | 合并前必须修复 |
| 🟡 [important] | 重要问题 | 应当修复 |
| 🟢 [nit] | 小问题 | 非阻塞建议 |
| 💡 [suggestion] | 替代方案 | 考虑采纳 |
| 📚 [learning] | 教育评论 | 无需操作 |
| 🎉 [praise] | 优秀表现 | 继续保持 |

### 支持的语言和框架

| 语言 | 框架覆盖 |
|------|----------|
| TypeScript | React, Vue, Angular, NestJS, Next.js |
| Python | Django, FastAPI, Flask, AsyncIO |
| Go | 标准库, Gin, Echo, gRPC |

### 使用方式

**通过命令激活:**
```
/review <文件或 PR 地址>
```

**通过描述激活:**
```
"帮我审查这些代码"
"审查这个 PR 的安全问题"
"代码审查"
```

**输出格式选项:**
```
review src/**/*.ts --format markdown    # 默认 Markdown
review src/**/*.ts --format html        # HTML 报告
```

### 系统架构

```
┌─────────────────────────────────────────────────────────────┐
│                      主编 (Review Commander)                 │
│                    (任务协调与报告汇总)                        │
└───────────────────────────┬─────────────────────────────────┘
                            │
         ┌──────────────────┼──────────────────┐
         ▼                  ▼                  ▼
  ┌────────────┐     ┌────────────┐     ┌────────────┐
  │   架构     │     │   安全     │     │   性能     │
  │  审查员    │     │  审查员    │     │  审查员    │
  └────────────┘     └────────────┘     └────────────┘
         │                  │                  │
         ▼                  ▼                  ▼
  ┌────────────┐     ┌────────────┐     ┌────────────┐
  │   代码     │     │   测试     │     │   API      │
  │   质量     │     │  审查员    │     │  审查员    │
  └────────────┘     └────────────┘     └────────────┘
```

### 项目结构

```
multi-agent-code-review/
├── SKILL.md                      # 主入口
├── README.md                     # 项目文档
├── test_pool.md                  # 测试用例池
└── references/
    ├── agents.md                 # 智能体定义
    ├── pipeline.md               # 协作流程
    ├── review-phases.md          # 四阶段审查
    ├── severity-labels.md         # 标签系统
    ├── output-template.md        # 输出模板
    └── languages/
        ├── typescript.md         # TypeScript 指南
        ├── python.md             # Python 指南
        └── go.md                 # Go 指南
```

### 审查决策

| 条件 | 决策 |
|------|------|
| 存在 🔴 blocking 问题 | **Request Changes** - 必须修改后才能合并 |
| 存在 🟡 important 问题 | **Comment** - 建议修改 |
| 只有 🟢 nit 或 💡 suggestion | **Approve** - 可以合并 |
| 代码质量优秀 | **Approve with Praise** - 表扬 |

---

## License

MIT