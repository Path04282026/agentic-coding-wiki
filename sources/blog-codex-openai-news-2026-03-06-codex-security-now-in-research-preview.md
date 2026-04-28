---
tags: [source, blog]
product: Codex
date: 2026-03-06
dimensions: [03]

---

# Blog — Codex — Codex Security Preview

**Source:** `raw/blogs/codex/openai-news__2026-03-06__codex-security-now-in-research-preview.md`
**Product:** [Codex](references/entities/codex.md)
**Published:** March 6, 2026
**URL:** https://openai.com/index/codex-security-now-in-research-preview/

## Summary

Launch of Codex Security, an application security agent that combines agentic reasoning with automated validation to find real vulnerabilities — not SAST-style pattern matching.

## Key Technical Details

### How It Works (3 Phases)
1. **Build system context + threat model** — Analyzes repository, generates editable threat model capturing what the system does, trusts, and where it's exposed
2. **Prioritize and validate** — Uses threat model to search for vulnerabilities, categorizes by real-world impact, pressure-tests findings in sandboxed validation environments
3. **Patch with system context** — Proposes fixes aligned with system intent, minimizing regressions

### Iterative Learning
- Learns from user feedback to refine threat models
- Adjusting criticality of findings improves precision on subsequent runs

### Scale Metrics (30-day beta)
- 1.2M+ commits scanned across external beta repos
- 792 critical findings, 10,561 high-severity
- Critical issues in <0.1% of scanned commits
- Noise reduction: 84% cut in one repo, 50%+ reduction in false positives, 90%+ reduction in over-reported severity

### Real CVEs Discovered
- 14 CVEs assigned across OpenSSH, GnuTLS, GOGS, Thorium, libssh, PHP, Chromium
- Found critical SSRF, cross-tenant auth bypass internally

### Open Source Support
- "Codex for OSS" program — free accounts + security scanning for maintainers
- Key insight from maintainers: "the challenge isn't a lack of vulnerability reports, but too many low-quality ones"

## Relevant Dimensions

- [03 - Security and Sandboxing](references/dimensions/03-security-and-sandboxing.md) — Codex Security as an agentic approach to application security
