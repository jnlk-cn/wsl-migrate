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
