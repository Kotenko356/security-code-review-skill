---
name: security-code-review
description: |
  Full security audit of a project's codebase. Use when user asks to review code for vulnerabilities, check security, find exploits, or audit a project. Pulls domain-specific skills from Anthropic Cybersecurity Skills (mukul975/Anthropic-Cybersecurity-Skills) and code-level security review patterns from Trail of Bits (via VoltAgent/awesome-agent-skills). Works for any language/framework. Use webfetch to pull relevant SKILL.md files on-demand.
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash(git *)
  - webfetch
---

# Security Code Review Skill

## Workflow

### Phase 1: Project Analysis

1. Read the project structure (directories, Cargo.toml / package.json / go.mod / requirements.txt)
2. Identify the language, framework, and type of application (proxy, web service, CLI tool, etc.)
3. Read ALL source files to understand the architecture

### Phase 2: Skill Discovery

Pull skills from TWO sources. Fetch the most relevant based on project type.

#### Source A: Anthropic Cybersecurity Skills (domain-specific security)

**Raw URL**: `https://raw.githubusercontent.com/mukul975/Anthropic-Cybersecurity-Skills/main/skills/{skill-name}/SKILL.md`

| Project Type | Skills to Fetch |
|-------------|-----------------|
| Rust crypto/proxy | `performing-cryptographic-audit-of-application`, `analyzing-network-covert-channels-in-malware`, `analyzing-command-and-control-communication`, `analyzing-ransomware-encryption-mechanisms` |
| Web app (JS/TS) | `analyzing-api-security`, `analyzing-web-server-logs-for-intrusion`, `building-detection-rules-with-sigma` |
| Mobile | `analyzing-android-malware-with-apktool`, `analyzing-ios-app-security-with-objection` |
| Cloud/Terraform | `auditing-terraform-infrastructure-for-security`, `analyzing-kubernetes-audit-logs`, `auditing-cloud-with-cis-benchmarks` |
| Container/K8s | `analyzing-docker-container-forensics`, `auditing-kubernetes-cluster-rbac` |
| Supply chain/CI | `analyzing-sbom-for-supply-chain-vulnerabilities`, `building-devsecops-pipeline-with-gitlab-ci` |
| Auth/IAM | `auditing-azure-active-directory-configuration`, `building-identity-federation-with-saml-azure-ad` |
| C/C++ | `analyzing-linux-elf-malware`, `analyzing-heap-spray-exploitation` |

#### Source B: VoltAgent awesome-agent-skills (Trail of Bits security review skills)

**Raw URL**: `https://raw.githubusercontent.com/VoltAgent/awesome-agent-skills/main/skills/trailofbits/{skill-name}/SKILL.md`

**Always fetch these if doing code-level security review:**

| Skill | Use Case |
|-------|----------|
| `constant-time-analysis` | Detect compiler-induced timing side-channels in crypto code |
| `insecure-defaults` | Find hardcoded secrets, default credentials, weak crypto defaults |
| `static-analysis` | CodeQL, Semgrep, SARIF patterns for vulnerability detection |
| `differential-review` | Security-focused git diff analysis |
| `sharp-edges` | Identify error-prone APIs and dangerous configurations |
| `variant-analysis` | Pattern-based search for similar vulnerabilities across codebase |
| `semgrep-rule-creator` | Build Semgrep rules for project-specific vulnerability patterns |

**Additional context skills (fetch when applicable):**

| Skill | Use Case |
|-------|----------|
| `audit-context-building` | Deep architectural analysis before review |
| `property-based-testing` | Fuzzing patterns for Rust/Python/Crypto code |
| `building-secure-contracts` | If project has smart contracts |
| `testing-handbook-skills` | Fuzzers, sanitizers, static analysis integration |

### Phase 3: Apply Skills

Read the fetched SKILL.md files and apply their vulnerability checklists, workflow steps, and validation criteria to the project code. For each finding, reference:
- The specific file and line number
- The vulnerability pattern from the skill
- The severity
- The root cause
- The fix

### Phase 4: Report

Output a table:

```
| # | Severity | Vulnerability | File:Line | Root Cause |
|---|----------|--------------|-----------|------------|
```

## Key Domains Covered

### From Anthropic Cybersecurity Skills (operational/domain)
1. **cryptography** — key exchange, encryption, hashing, random generation, timing attacks
2. **network-security** — protocol design, replay protection, MITM resistance, DoS
3. **web-application-security** — injection, XSS, CSRF, auth bypass, session management
4. **api-security** — rate limiting, input validation, auth tokens, CORS
5. **container-security** — Dockerfile, root user, exposed ports, secrets
6. **incident-response** — logging, monitoring, alerting, forensic readiness
7. **vulnerability-management** — dependency scanning, CVE checking, patch management
8. **compliance-governance** — hardcoded secrets, audit logging, data protection
9. **devsecops** — CI/CD security, code signing, artifact integrity
10. **identity-access-management** — auth flows, password policy, MFA, session tokens

### From Trail of Bits / VoltAgent (code-level security review)
11. **constant-time-analysis** — timing side-channels in crypto/sensitive comparisons
12. **insecure-defaults** — hardcoded secrets, default credentials, weak crypto configs
13. **static-analysis** — CodeQL, Semgrep, SARIF integration patterns
14. **differential-review** — git diff security analysis, commit-by-commit review
15. **sharp-edges** — dangerous API patterns, unsafe function usage
16. **variant-analysis** — pattern-based search for similar bugs across codebase

## Quick Commands

Check dependencies for known CVEs:
```bash
cargo audit          # Rust
npm audit            # Node.js
pip-audit            # Python
go list -m -u all    # Go
```

Check for secrets in git history:
```bash
git log -p | grep -iE '(password|secret|key|token|api_key)'
```
