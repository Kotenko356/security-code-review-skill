# Security Code Review Skill

Скилл для AI-агентов, выполняющий автоматический аудит безопасности кодовой базы. При загрузке агент анализирует проект, подгружает доменные скиллы из [Anthropic Cybersecurity Skills](https://github.com/mukul975/Anthropic-Cybersecurity-Skills) и [Trail of Bits](https://github.com/VoltAgent/awesome-agent-skills), затем выдаёт отчёт по уязвимостям.

## Поддерживаемые платформы

Любой агент, совместимый со стандартом [agentskills.io](https://agentskills.io):
- **Kilo** — скопировать в `~/.kilocode/skills/security-code-review/`
- **Claude Code** — скопировать в `~/.claude/skills/security-code-review/`
- **Cursor** — скопировать в `.cursor/skills/security-code-review/`
- **Codex CLI** — скопировать в `~/.codex/skills/security-code-review/`
- **OpenCode** — скопировать в `~/.opencode/skills/security-code-review/`

## Установка

```bash
# Клонировать и скопировать в директорию скиллов
git clone https://github.com/Kotenko356/security-code-review-skill.git
cp -r security-code-review-skill/skills/security-code-review ~/.kilocode/skills/

# Или для Claude Code
cp -r security-code-review-skill/skills/security-code-review ~/.claude/skills/
```

## Использование

```
проверь проект на уязвимости
проревьюй код на безопасность
найди криптографические проблемы в проекте
```

Через команду (`/security-review`):
```
/security-review
/security-review src/handshake.rs
```

## Источники скиллов

При ревью подгружаются скиллы из двух репозиториев:

| Источник | Скиллы | Фокус |
|----------|--------|-------|
| [Anthropic Cybersecurity Skills](https://github.com/mukul975/Anthropic-Cybersecurity-Skills) | `cryptographic-audit`, `network-covert`, `command-and-control`, `ransomware-encryption` | Доменный (крипто, сеть, протоколы) |
| [Trail of Bits / VoltAgent](https://github.com/VoltAgent/awesome-agent-skills) | `constant-time-analysis`, `insecure-defaults`, `static-analysis`, `differential-review`, `sharp-edges`, `variant-analysis` | Уровень кода (timing, secrets, SAST) |

## Что находит

| Домен | Примеры |
|-------|---------|
| Криптография | Закодированные секреты, нет forward secrecy, timing-атаки, слабый RNG |
| Протоколы | Replay-атаки, нет аутентификации, нет challenge-response |
| Сеть | SSRF, DoS, нет rate limiting, JA3-фингерпринтинг |
| Код | Паника на вводе, небезопасные unwrap, утечки ресурсов |
| Инфраструктура | Root в Docker, нет health checks, нет graceful shutdown |

## Формат отчёта

```
| # | Severity | Vulnerability | File:Line | Root Cause |
|---|----------|--------------|-----------|------------|
```

## Лицензия

Apache 2.0 — как и исходные скиллы.
