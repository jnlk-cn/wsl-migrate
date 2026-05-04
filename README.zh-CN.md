<h1 align="center">WSL2 发行版迁移</h1>

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

将 WSL2 发行版从系统盘迁移到其他磁盘的简洁指南与 Skill。

## 为什么要迁移？

WSL2 发行版会随着使用时间增长占用数十 GB 空间。将它们从系统盘（通常是 `C:`）迁移出去可以释放空间，也方便备份。

## 关于本 Skill

本仓库是一个 **AI Agent Skill** —— 一种结构化、可复用的工作流，供 AI 代理安全、一致地执行 WSL2 迁移操作。

核心指令文件是 [`SKILL.md`](SKILL.md)，采用标准格式编写（YAML frontmatter + Markdown 正文）。AI 代理加载它后即可掌握迁移流程、安全检查点和边界情况处理，无需每次重新摸索。

### 代理兼容性

| 代理 / 平台 | 使用方式 | 兼容性 |
|------------|---------|--------|
| **Kimi Code CLI** | 放入 `~/.kimi/skills/` 或项目 `.agents/skills/`。Frontmatter 在 WSL 迁移相关查询时自动触发。 | ✅ 原生支持 |
| **Claude / Cursor** | 将 `SKILL.md` 内容复制到系统提示或 `@` 上下文。代理按步骤执行工作流。 | ✅ 完全兼容 |
| **ChatGPT / Copilot** | 将 `SKILL.md` 粘贴为上下文，然后请求 WSL 迁移帮助。 | ✅ 完全兼容 |
| **GitHub Copilot Chat** | 在对话中引用该文件，或将其放入工作区以获得内联协助。 | ✅ 完全兼容 |

工作流仅依赖标准 PowerShell / WSL CLI 命令，任何在 Windows 上具备 shell 访问权限的代理均可执行。

## 如何导入

### Kimi Code CLI

1. 克隆或下载本仓库。
2. 将 `wsl-migrate` 文件夹（包含 `SKILL.md` 的文件夹）复制到 skill 目录：
   - **用户级**: `~/.kimi/skills/wsl-migrate/`
   - **项目级**: `.agents/skills/wsl-migrate/`
3. 重启 Kimi，或使用 `--skills-dir` 参数指向你的 skills 文件夹。
4. 向 Kimi 询问任何 WSL 迁移相关问题 —— skill 会自动触发。

### Claude / Cursor / ChatGPT / Copilot

1. 打开 [`SKILL.md`](SKILL.md)。
2. 将全部内容复制到代理的上下文（系统提示、自定义指令或对话消息）。
3. 让代理执行 WSL 迁移操作。

> **提示**: 如需重复使用，将 `SKILL.md` 保存为代理设置中的"自定义指令"或"知识库"。

## 快速开始

### 1. 查看当前发行版

```powershell
wsl -l -v
```

### 2. 导出与导入

```powershell
# 关闭 WSL
wsl --shutdown

# 导出
wsl --export Ubuntu-24.04 D:\WSL\ubuntu.tar

# 导入到新位置
wsl --import ubuntu D:\WSL\Ubuntu D:\WSL\ubuntu.tar
```

### 3. 恢复默认用户

导入后 WSL 默认以 `root` 登录。通过创建 `/etc/wsl.conf` 修复：

```powershell
wsl -d ubuntu -u root -e bash -c "tee /etc/wsl.conf << 'EOF'
[user]
default=<你的Linux用户名>
EOF"
wsl --terminate ubuntu
```

### 4. 清理旧发行版

验证无误后，注销旧发行版并删除 tar 文件：

```powershell
wsl --unregister Ubuntu-24.04
Remove-Item D:\WSL\ubuntu.tar
```

## 完整指南

完整的分步操作流程（包括多发行版迁移、重命名发行版、安全提示等）请参阅 [`SKILL.md`](SKILL.md)。

## Star 历史

[![Star History Chart](https://api.star-history.com/svg?repos=jnlk-cn/wsl-migrate&type=Date)](https://star-history.com/#jnlk-cn/wsl-migrate&Date)

## 许可证

本项目基于 [MIT 许可证](LICENSE) 开源。
