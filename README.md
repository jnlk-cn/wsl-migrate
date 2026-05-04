<h1 align="center">WSL2 Distribution Migration</h1>

<p align="center">
  <a href="https://github.com/jnlk-cn/wsl-migrate/stargazers"><img src="https://img.shields.io/github/stars/jnlk-cn/wsl-migrate?style=flat-square" alt="GitHub stars"></a>
  <a href="https://github.com/jnlk-cn/wsl-migrate/network/members"><img src="https://img.shields.io/github/forks/jnlk-cn/wsl-migrate?style=flat-square" alt="GitHub forks"></a>
  <a href="https://github.com/jnlk-cn/wsl-migrate/issues"><img src="https://img.shields.io/github/issues/jnlk-cn/wsl-migrate?style=flat-square" alt="GitHub issues"></a>
  <a href="LICENSE"><img src="https://img.shields.io/github/license/jnlk-cn/wsl-migrate?style=flat-square" alt="License"></a>
  <a href="https://github.com/jnlk-cn/wsl-migrate/commits/main"><img src="https://img.shields.io/github/last-commit/jnlk-cn/wsl-migrate?style=flat-square" alt="Last commit"></a>
</p>

<p align="center">
  <a href="README.md">English</a> |
  <a href="README.zh-CN.md">简体中文</a> |
  <a href="README.ja.md">日本語</a> |
  <a href="README.ko.md">한국어</a>
</p>

---

A concise guide and skill for migrating WSL2 distributions from the system drive to another drive.

## Why Migrate?

WSL2 distributions can consume tens of gigabytes over time. Moving them off the system drive (usually `C:`) frees up space and makes backups easier.

## About This Skill

This repository is an **AI Agent Skill** — a structured, reusable workflow that AI agents can follow to perform WSL2 migrations safely and consistently.

The core instruction file is [`SKILL.md`](SKILL.md), written in a standard format with YAML frontmatter and Markdown body. AI agents load it to understand the migration workflow, safety checks, and edge cases without reinventing the procedure each time.

### Agent Compatibility

| Agent / Platform | How to Use | Compatibility |
|------------------|------------|---------------|
| **Kimi Code CLI** | Place in `~/.kimi/skills/` or project `.agents/skills/`. Frontmatter auto-triggers on WSL migration queries. | ✅ Native |
| **Claude / Cursor** | Copy `SKILL.md` content into system prompt or `@` context. Agent follows the step-by-step workflow. | ✅ Full |
| **ChatGPT / Copilot** | Paste `SKILL.md` as context before asking for WSL migration help. | ✅ Full |
| **GitHub Copilot Chat** | Reference the file in chat or include it in the workspace for inline assistance. | ✅ Full |

The workflow relies only on standard PowerShell / WSL CLI commands, so any agent with shell access on Windows can execute it.

## How to Install

### Kimi Code CLI

1. Clone or download this repository.
2. Copy the `wsl-migrate` folder (the one containing `SKILL.md`) into your skills directory:
   - **User-level**: `~/.kimi/skills/wsl-migrate/`
   - **Project-level**: `.agents/skills/wsl-migrate/`
3. Restart Kimi or run with `--skills-dir` pointing to your skills folder.
4. Ask Kimi anything about WSL migration — the skill auto-triggers.

### Claude / Cursor / ChatGPT / Copilot

1. Open [`SKILL.md`](SKILL.md).
2. Copy the entire content into your agent's context (system prompt, custom instruction, or chat message).
3. Ask the agent to migrate your WSL distributions.

> **Tip**: For repeated use, save `SKILL.md` as a "custom instruction" or "knowledge base" in your agent's settings.

## Quick Start

### 1. Check Current Distributions

```powershell
wsl -l -v
```

### 2. Export & Import

```powershell
# Shut down WSL
wsl --shutdown

# Export
wsl --export Ubuntu-24.04 D:\WSL\ubuntu.tar

# Import to new location
wsl --import ubuntu D:\WSL\Ubuntu D:\WSL\ubuntu.tar
```

### 3. Restore Default User

After import, WSL defaults to `root`. Fix this by creating `/etc/wsl.conf`:

```powershell
wsl -d ubuntu -u root -e bash -c "tee /etc/wsl.conf << 'EOF'
[user]
default=<your-linux-username>
EOF"
wsl --terminate ubuntu
```

### 4. Clean Up Old Distribution

Once verified, unregister the old distribution and delete the tar:

```powershell
wsl --unregister Ubuntu-24.04
Remove-Item D:\WSL\ubuntu.tar
```

## Full Guide

See [`SKILL.md`](SKILL.md) for the complete step-by-step workflow, including multi-distribution migration, renaming distributions, and safety tips.

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=jnlk-cn/wsl-migrate&type=Date)](https://star-history.com/#jnlk-cn/wsl-migrate&Date)

## License

This project is licensed under the [MIT License](LICENSE).
