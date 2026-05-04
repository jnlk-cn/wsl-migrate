<h1 align="center">WSL2 ディストリビューション移行</h1>

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

WSL2 ディストリビューションをシステムドライブから別のドライブに移行するための簡潔なガイドと Skill です。

## なぜ移行するのか？

WSL2 ディストリビューションは、使用時間の経過とともに数十 GB の容量を消費することがあります。システムドライブ（通常は `C:`）から移行することで、空き容量を確保し、バックアップも容易になります。

## この Skill について

このリポジトリは **AI Agent Skill** です — AI エージェントが WSL2 の移行を安全かつ一貫して実行できるよう、構造化された再利用可能なワークフローを提供します。

核心となる指示ファイルは [`SKILL.md`](SKILL.md) で、YAML frontmatter と Markdown 本文の標準形式で記述されています。AI エージェントはこれを読み込むことで、移行ワークフロー、安全チェック、境界ケースの処理を把握でき、毎回手順を再発明する必要がありません。

### エージェント互換性

| エージェント / プラットフォーム | 使用方法 | 互換性 |
|------------------------------|---------|--------|
| **Kimi Code CLI** | `~/.kimi/skills/` またはプロジェクトの `.agents/skills/` に配置。Frontmatter が WSL 移行に関するクエリで自動的にトリガーされます。 | ✅ ネイティブ |
| **Claude / Cursor** | `SKILL.md` の内容をシステムプロンプトまたは `@` コンテキストにコピー。エージェントがステップバイステップで実行します。 | ✅ 完全互換 |
| **ChatGPT / Copilot** | `SKILL.md` をコンテキストとして貼り付け、WSL 移行の支援を依頼します。 | ✅ 完全互換 |
| **GitHub Copilot Chat** | チャットでファイルを参照するか、ワークスペースに含めてインライン支援を受けます。 | ✅ 完全互換 |

ワークフローは標準の PowerShell / WSL CLI コマンドのみを使用するため、Windows 上でシェルアクセスを持つ任意のエージェントが実行できます。

## クイックスタート

### 1. 現在のディストリビューションを確認

```powershell
wsl -l -v
```

### 2. エクスポートとインポート

```powershell
# WSL をシャットダウン
wsl --shutdown

# エクスポート
wsl --export Ubuntu-24.04 D:\WSL\ubuntu.tar

# 新しい場所にインポート
wsl --import ubuntu D:\WSL\Ubuntu D:\WSL\ubuntu.tar
```

### 3. デフォルトユーザーの復元

インポート後、WSL はデフォルトで `root` でログインします。`/etc/wsl.conf` を作成して修正します：

```powershell
wsl -d ubuntu -u root -e bash -c "tee /etc/wsl.conf << 'EOF'
[user]
default=<あなたのLinuxユーザー名>
EOF"
wsl --terminate ubuntu
```

### 4. 古いディストリビューションの削除

正常に動作することを確認したら、古いディストリビューションを登録解除し、tar ファイルを削除します：

```powershell
wsl --unregister Ubuntu-24.04
Remove-Item D:\WSL\ubuntu.tar
```

## 完全なガイド

複数ディストリビューションの移行、ディストリビューションの名前変更、安全上の注意事項など、完全なステップバイステップのワークフローについては [`SKILL.md`](SKILL.md) を参照してください。

## Star 履歴

[![Star History Chart](https://api.star-history.com/svg?repos=jnlk-cn/wsl-migrate&type=Date)](https://star-history.com/#jnlk-cn/wsl-migrate&Date)

## ライセンス

このプロジェクトは [MIT ライセンス](LICENSE) の下でライセンスされています。
