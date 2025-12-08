---
title: Semgrep
layout: default
nav_order: 2
---

# Semgrep Integration

[Semgrep](https://semgrep.dev/) is a fast, lightweight static analysis tool designed to detect security vulnerabilities, anti-patterns, and enforce best practices across codebases. It operates using pattern-based matching, much like grep, but with semantic awareness of programming language structures. In the context of VulCAN, Semgrep is used as the core mechanism for identifying vulnerabilities during code scans.

## Key characteristics:

- Pattern-based matching: Semgrep rules define abstract code structures (AST patterns), enabling accurate detection without complex configuration.

- Language-aware: Supports 30+ languages (Python, JS/TS, Go, Java, C, etc.) with syntactic understanding.

- Custom and community rules: You can write custom YAML rules or use thousands of open-source security rules (OWASP, r2c, bandit-equivalent, etc.).

- Speed and CI-friendly design: Designed for incremental scans and seamless integration into CI pipelines.

## Internal Working
Semgrep operates by parsing the target source code into an abstract syntax tree (AST) and applying rule patterns to match structures of interest. When a rule matches, Semgrep emits a JSON "finding" that includes:
- The file path and line ranges
- The rule metadata (CWE, message, severity)
- The code snippet matched
- The rule ID and category
- Any additional context 
This output is deterministic and well-structured, making it suitable for custom vulnerability scanners.

## Integrating Semgrep in CI
VulCAN uses Semgrep CI to run scans as part of the automated pipeline. The CI workflow typically:
- Runs Semgrep with the official `--config auto`
- Outputs the results in JSON format using:
```
semgrep --json --output results.json
```
- The generated results.json is then processed by the worker service.
- Semgrep CI ensures consistent scanning across branches, PRs, and repositories, enabling systematic vulnerability analysis.

## JSON Output Structure
Semgrep's JSON output follows a predictable structure where each "finding" contains multiple nested fields. A typical finding looks like:
```
{
  "path": "src/app.py",
  "start": { "line": 14, "col": 5 },
  "end":   { "line": 18, "col": 10 },
  "extra": {
    "message": "Detected use of insecure function",
    "severity": "HIGH",
    "metadata": {
      "cwe": ["CWE-327"]
    }
  }
}
```

VulCAN extracts the relevant fields from each finding and normalizes them for internal use. When Semgrep CI generates its JSON report, the scanner processes each finding to extract structured vulnerability information.

The following fields are parsed:

1. File Path: Indicates where the vulnerability occurs within the repository.

2. Start and End Lines: These define the exact line range of the vulnerable code block. Defaults are used to ensure robustness if fields are missing. This information along with the file path is used to fetch the code blob to send as context for GenAI suggestions. 

3. Vulnerability Type (CWE): Semgrep rules often include CWE IDs. the scanner extracts the first CWE if multiple are present.
If no CWE is provided, "N/A" is assigned.

4. Message (Rule Explanation): The human-readable explanation from the Semgrep rule is stored to describe the issue.

5. Severity: Semgrep severities are normalized to lowercase (low, medium, high, critical), which simplifies downstream processing (UI display, sorting, alerting, etc.).


