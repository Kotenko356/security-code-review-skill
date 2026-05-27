# Security Code Review Skill

AI-agent skill for automated security audits of codebases. When loaded, the agent analyzes the project, fetches domain-specific skills from [Anthropic Cybersecurity Skills](https://github.com/mukul975/Anthropic-Cybersecurity-Skills) and [Trail of Bits](https://github.com/VoltAgent/awesome-agent-skills), and produces a vulnerability report.

## Supported Platforms

Any agent supporting the [agentskills.io](https://agentskills.io) standard:
- **Kilo** — copy to `~/.kilocode/skills/security-code-review/`
- **Claude Code** — copy to `~/.claude/skills/security-code-review/`
- **Cursor** — copy to `.cursor/skills/security-code-review/`
- **Codex CLI** — copy to `~/.codex/skills/security-code-review/`
- **OpenCode** — copy to `~/.opencode/skills/security-code-review/`

## Installation

```bash
# Clone to your skills directory
git clone https://github.com/$USER/security-code-review-skill.git
cp -r security-code-review-skill/skills/security-code-review ~/.kilocode/skills/

# Or for Claude Code
cp -r security-code-review-skill/skills/security-code-review ~/.claude/skills/
```

## Usage

```
>> review this project for security vulnerabilities
>> run a security audit on src/
>> check this code for crypto weaknesses
```

Or with a custom command (`/security-review`):
```
/security-review
/security-review src/handshake.rs
```

## Skill Sources

The skill pulls from two external repositories at review time:

| Source | Skills | Focus |
|--------|--------|-------|
| [Anthropic Cybersecurity Skills](https://github.com/mukul975/Anthropic-Cybersecurity-Skills) | `cryptographic-audit`, `network-covert`, `command-and-control`, `ransomware-encryption` | Domain-specific (crypto, network, protocol) |
| [Trail of Bits / VoltAgent](https://github.com/VoltAgent/awesome-agent-skills) | `constant-time-analysis`, `insecure-defaults`, `static-analysis`, `differential-review`, `sharp-edges`, `variant-analysis` | Code-level (timing, secrets, SAST) |

## What It Finds

| Domain | Examples |
|--------|----------|
| Cryptography | Hardcoded secrets, no forward secrecy, timing side-channels, weak RNG |
| Protocol | Replay attacks, missing auth, no challenge-response |
| Network | SSRF, DoS, missing rate limits, JA3 fingerprinting |
| Code | Panic on input, unsafe unwraps, resource leaks |
| Infrastructure | Root user in Docker, no health checks, graceful shutdown |

## Output Format

```
| # | Severity | Vulnerability | File:Line | Root Cause |
|---|----------|--------------|-----------|------------|
```

## License

Apache 2.0 — same as source skills.
