# 审查报告模板

## Markdown 格式

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

---

## HTML 格式

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Code Review Report</title>
    <style>
        :root {
            --bg-primary: #ffffff;
            --bg-secondary: #f6f8fa;
            --bg-tertiary: #f0f0f0;
            --text-primary: #24292f;
            --text-secondary: #57606a;
            --border-color: #d0d7de;
            --blocking: #d1242f;
            --important: #bf8700;
            --nit: #1a7f37;
            --suggestion: #8250df;
            --learning: #0550ae;
            --praise: #1f883d;
            --severity-block: #ffeef0;
            --severity-imp: #fff8c5;
            --severity-nit: #dafbe1;
            --severity-sug: #fbefff;
            --severity-lea: #ddf4ff;
            --severity-pra: #dafbe1;
        }

        * { box-sizing: border-box; margin: 0; padding: 0; }

        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, Arial, sans-serif;
            line-height: 1.6;
            color: var(--text-primary);
            background: var(--bg-primary);
            padding: 2rem;
            max-width: 1200px;
            margin: 0 auto;
        }

        .header {
            border-bottom: 1px solid var(--border-color);
            padding-bottom: 1rem;
            margin-bottom: 2rem;
        }

        .title {
            font-size: 1.75rem;
            font-weight: 600;
            margin-bottom: 0.5rem;
        }

        .meta {
            color: var(--text-secondary);
            font-size: 0.875rem;
        }

        .summary {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
            gap: 1rem;
            margin-bottom: 2rem;
            padding: 1rem;
            background: var(--bg-secondary);
            border-radius: 6px;
        }

        .summary-item {
            display: flex;
            flex-direction: column;
        }

        .summary-label {
            font-size: 0.75rem;
            text-transform: uppercase;
            color: var(--text-secondary);
            margin-bottom: 0.25rem;
        }

        .summary-value {
            font-size: 1.25rem;
            font-weight: 600;
        }

        .summary-value.blocking { color: var(--blocking); }
        .summary-value.important { color: var(--important); }
        .summary-value.nit { color: var(--nit); }

        .section {
            margin-bottom: 2rem;
            border: 1px solid var(--border-color);
            border-radius: 6px;
            overflow: hidden;
        }

        .section-header {
            background: var(--bg-secondary);
            padding: 0.75rem 1rem;
            font-weight: 600;
            border-bottom: 1px solid var(--border-color);
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }

        .section-body {
            padding: 1rem;
        }

        .severity-tag {
            display: inline-flex;
            align-items: center;
            padding: 0.125rem 0.5rem;
            border-radius: 12px;
            font-size: 0.75rem;
            font-weight: 500;
            margin-right: 0.5rem;
        }

        .severity-tag.blocking {
            background: var(--severity-block);
            color: var(--blocking);
        }

        .severity-tag.important {
            background: var(--severity-imp);
            color: var(--important);
        }

        .severity-tag.nit {
            background: var(--severity-nit);
            color: var(--nit);
        }

        .severity-tag.suggestion {
            background: var(--severity-sug);
            color: var(--suggestion);
        }

        .severity-tag.learning {
            background: var(--severity-lea);
            color: var(--learning);
        }

        .severity-tag.praise {
            background: var(--severity-pra);
            color: var(--praise);
        }

        .issue {
            padding: 1rem;
            border-bottom: 1px solid var(--border-color);
        }

        .issue:last-child { border-bottom: none; }

        .issue-header {
            display: flex;
            align-items: center;
            margin-bottom: 0.5rem;
        }

        .issue-title {
            font-weight: 500;
        }

        .issue-location {
            font-size: 0.875rem;
            color: var(--text-secondary);
            margin-left: auto;
        }

        .issue-description {
            margin-left: 1.5rem;
            color: var(--text-secondary);
        }

        .issue-suggestion {
            margin-left: 1.5rem;
            margin-top: 0.5rem;
            padding: 0.5rem;
            background: var(--bg-secondary);
            border-radius: 4px;
            font-size: 0.875rem;
        }

        .issue-suggestion strong { color: var(--text-primary); }

        .verdict {
            padding: 1.5rem;
            border-radius: 6px;
            text-align: center;
        }

        .verdict.approve {
            background: var(--severity-nit);
            border: 1px solid var(--nit);
        }

        .verdict.comment {
            background: var(--severity-imp);
            border: 1px solid var(--important);
        }

        .verdict.request-changes {
            background: var(--severity-block);
            border: 1px solid var(--blocking);
        }

        .verdict-label {
            font-size: 1.25rem;
            font-weight: 600;
            margin-bottom: 0.5rem;
        }

        .verdict.approve .verdict-label { color: var(--nit); }
        .verdict.comment .verdict-label { color: var(--important); }
        .verdict.request-changes .verdict-label { color: var(--blocking); }

        .stats-table {
            width: 100%;
            border-collapse: collapse;
            margin: 1rem 0;
        }

        .stats-table th, .stats-table td {
            padding: 0.75rem;
            text-align: left;
            border-bottom: 1px solid var(--border-color);
        }

        .stats-table th {
            background: var(--bg-secondary);
            font-weight: 600;
        }

        .code-block {
            background: var(--bg-tertiary);
            padding: 1rem;
            border-radius: 4px;
            font-family: "SFMono-Regular", Consolas, "Liberation Mono", Menlo, monospace;
            font-size: 0.875rem;
            overflow-x: auto;
            margin: 0.5rem 0;
        }

        .code-block .line-number {
            color: var(--text-secondary);
            margin-right: 1rem;
            user-select: none;
        }

        .inline-code {
            background: var(--bg-tertiary);
            padding: 0.125rem 0.375rem;
            border-radius: 4px;
            font-family: "SFMono-Regular", Consolas, monospace;
            font-size: 0.875rem;
        }

        @media (max-width: 768px) {
            body { padding: 1rem; }
            .summary { grid-template-columns: repeat(2, 1fr); }
        }
    </style>
</head>
<body>
    <div class="header">
        <h1 class="title">Code Review Report</h1>
        <div class="meta">
            <span>Repository: </span><strong>owner/repo</strong>
            <span> | </span>
            <span>Branch: </span><strong>feature/xyz</strong>
            <span> | </span>
            <span>Date: </span><strong>2026-05-23</strong>
        </div>
    </div>

    <div class="summary">
        <div class="summary-item">
            <span class="summary-label">Files Reviewed</span>
            <span class="summary-value">12</span>
        </div>
        <div class="summary-item">
            <span class="summary-label">Lines Changed</span>
            <span class="summary-value">+245 / -89</span>
        </div>
        <div class="summary-item">
            <span class="summary-label">
                <span class="severity-tag blocking">blocking</span>
            </span>
            <span class="summary-value blocking">1</span>
        </div>
        <div class="summary-item">
            <span class="summary-label">
                <span class="severity-tag important">important</span>
            </span>
            <span class="summary-value important">3</span>
        </div>
        <div class="summary-item">
            <span class="summary-label">
                <span class="severity-tag nit">nit</span>
            </span>
            <span class="summary-value nit">5</span>
        </div>
    </div>

    <div class="section">
        <div class="section-header">🔍 Security Review</div>
        <div class="section-body">
            <div class="issue">
                <div class="issue-header">
                    <span class="severity-tag blocking">🔴 blocking</span>
                    <span class="issue-title">SQL Injection Vulnerability</span>
                    <span class="issue-location">src/db/queries.ts:42</span>
                </div>
                <div class="issue-description">
                    User input is directly concatenated into SQL query. An attacker could execute arbitrary SQL commands.
                </div>
                <div class="issue-suggestion">
                    <strong>Suggestion:</strong> Use parameterized queries instead.
                    <div class="code-block">
<span class="line-number">42</span><code>// ❌ Avoid
const query = `SELECT * FROM users WHERE id = ${userId}`;

// ✅ Recommended
const result = await db.query('SELECT * FROM users WHERE id = $1', [userId]);</code>
                    </div>
                </div>
            </div>

            <div class="issue">
                <div class="issue-header">
                    <span class="severity-tag important">🟡 important</span>
                    <span class="issue-title">Missing Rate Limiting</span>
                    <span class="issue-location">src/api/auth.ts:15</span>
                </div>
                <div class="issue-description">
                    Login endpoint has no rate limiting, making it vulnerable to brute force attacks.
                </div>
                <div class="issue-suggestion">
                    <strong>Suggestion:</strong> Add rate limiting middleware (e.g., express-rate-limit).
                </div>
            </div>
        </div>
    </div>

    <div class="section">
        <div class="section-header">⚡ Performance Review</div>
        <div class="section-body">
            <div class="issue">
                <div class="issue-header">
                    <span class="severity-tag important">🟡 important</span>
                    <span class="issue-title">N+1 Query Problem</span>
                    <span class="issue-location">src/service/user.ts:78</span>
                </div>
                <div class="issue-description">
                    Loop is causing N+1 database queries. Loading 100 users triggers 101 queries.
                </div>
                <div class="issue-suggestion">
                    <strong>Suggestion:</strong> Use eager loading or batch queries.
                </div>
            </div>
        </div>
    </div>

    <div class="section">
        <div class="section-header">💻 Code Quality Review</div>
        <div class="section-body">
            <div class="issue">
                <div class="issue-header">
                    <span class="severity-tag nit">🟢 nit</span>
                    <span class="issue-title">Type Safety</span>
                    <span class="issue-location">src/utils/helper.ts:23</span>
                </div>
                <div class="issue-description">
                    Using <span class="inline-code">any</span> type reduces type safety.
                </div>
                <div class="issue-suggestion">
                    <strong>Suggestion:</strong> Replace <span class="inline-code">any</span> with <span class="inline-code">unknown</span> and add type guards.
                </div>
            </div>

            <div class="issue">
                <div class="issue-header">
                    <span class="severity-tag praise">🎉 praise</span>
                    <span class="issue-title">Excellent Error Handling</span>
                    <span class="issue-location">src/service/payment.ts:56</span>
                </div>
                <div class="issue-description">
                    The error handling here is comprehensive and follows best practices!
                </div>
            </div>
        </div>
    </div>

    <div class="section">
        <div class="section-header">🧪 Testing Review</div>
        <div class="section-body">
            <div class="issue">
                <div class="issue-header">
                    <span class="severity-tag important">🟡 important</span>
                    <span class="issue-title">Insufficient Test Coverage</span>
                    <span class="issue-location">src/utils/validator.ts</span>
                </div>
                <div class="issue-description">
                    Only happy path tested. Missing edge cases and error scenarios.
                </div>
                <div class="issue-suggestion">
                    <strong>Suggestion:</strong> Add tests for invalid inputs, empty strings, and boundary values.
                </div>
            </div>
        </div>
    </div>

    <div class="verdict request-changes">
        <div class="verdict-label">Request Changes</div>
        <div>1 blocking issue must be fixed before merge. Please address the SQL injection vulnerability.</div>
    </div>
</body>
</html>
```

---

## 输出格式选择

审查报告支持两种输出格式：

| 格式 | 适用场景 | 触发关键词 |
|------|----------|-----------|
| Markdown | GitHub PR 评论、文档 | `--markdown` / `-m` |
| HTML | 报告展示、邮件发送 | `--html` / `-h` |

### 使用示例

```
review src/**/*.ts --format html
review PR #123 --output html > report.html
```