# multi-agent-code-review

Multi-agent collaborative code review system with parallel agent architecture.

## Overview

This skill coordinates multiple specialized review agents to provide comprehensive code review coverage across multiple dimensions:

- **Architecture Review** - System design and module organization
- **Security Review** - Vulnerability detection and security best practices
- **Performance Review** - Performance hotspots and optimization opportunities
- **Code Quality Review** - Code style, readability, and best practices
- **Testing Review** - Test coverage and edge case analysis

## Supported Languages

- TypeScript / JavaScript
- Python
- Go

## Usage

Activate by asking for code review:

```
/review <files or PR URL>
```

Or describe your review request:

```
"帮我审查这些代码"
"review the PR for security issues"
"代码审查"
```

## Output Format

The skill generates a structured Markdown report with severity-tagged findings:

- 🔴 [blocking] - Must fix before merge
- 🟡 [important] - Should fix
- 🟢 [nit] - Non-blocking suggestion
- 💡 [suggestion] - Alternative approach
- 📚 [learning] - Educational comment
- 🎉 [praise] - Well done

## Architecture

```
┌─────────────────────────────────────────┐
│         Review Commander                 │
│    (Task coordination & reporting)     │
└───────────┬─────────────────────────────┘
            │
    ┌───────┼───────┬───────┬───────┐
    ▼       ▼       ▼       ▼       ▼
 ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐
 │ Arch │ │Secur │ │Perform│ │Quality│ │Test  │
 │      │ │      │ │      │ │      │ │      │
 └──────┘ └──────┘ └──────┘ └──────┘ └──────┘
```

## License

MIT