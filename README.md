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

### Автоматически (через AI-агента)

Просто скажи своему агенту в чате:

> *«Установи скилл для security-ревью из репозитория Kotenko356/security-code-review-skill»*

Агент сам склонирует репозиторий и разместит скилл в нужной директории. Подходит для Kilo, Claude Code, Codex CLI и любого агента с доступом к терминалу.

### Вручную

```bash
git clone https://github.com/Kotenko356/security-code-review-skill.git
cp -r security-code-review-skill/skills/security-code-review ~/.kilocode/skills/

# Для Claude Code
cp -r security-code-review-skill/skills/security-code-review ~/.claude/skills/

# Для Cursor
cp -r security-code-review-skill/skills/security-code-review .cursor/skills/
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

При ревью подгружаются скиллы из двух репозиториев. Они дополняют друг друга на разных слоях:

| Источник | Скиллы | Роль |
|----------|--------|------|
| [Anthropic Cybersecurity Skills](https://github.com/mukul975/Anthropic-Cybersecurity-Skills) | `cryptographic-audit`, `network-covert`, `command-and-control`, `ransomware-encryption` | **«Что» искать** — доменные знания пентестера/криптографа: как должен работать handshake, какие атаки на ratchet, как DPI детектит трафик |
| [Trail of Bits / VoltAgent](https://github.com/VoltAgent/awesome-agent-skills) | `constant-time-analysis`, `insecure-defaults`, `static-analysis`, `differential-review`, `sharp-edges`, `variant-analysis` | **«Как» искать** — методология security-аудитора: паттерны insecure defaults, детект timing side-channels, variant analysis для поиска одинаковых багов |

На практике оба работают вместе: Anthropic говорит «ratchet должен иметь forward secrecy», Trail of Bits — «проверь каждый вызов SHA256 на месте KDF через variant analysis».

### Требование: доступ к Web Search

Для подгрузки скиллов из внешних репозиториев агенту нужна возможность веб-поиска или прямой загрузки URL. Например:
- **Kilo** — установлен скилл [Firecrawl](https://github.com/firecrawl/firecrawl) (`firecrawl-scrape` или `firecrawl-search`)
- **Claude Code** — встроенный `WebFetch`
- **Codex CLI** — встроенный `web_search`

Если у агента нет доступа к вебу, скилл всё равно сработает — проведёт ревью на основе встроенных в него чеклистов криптоаудита, протоколов и covert-каналов.

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
