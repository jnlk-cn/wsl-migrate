# WSL2 Distribution Migration

A concise guide and skill for migrating WSL2 distributions from the system drive to another drive.

## Why Migrate?

WSL2 distributions can consume tens of gigabytes over time. Moving them off the system drive (usually `C:`) frees up space and makes backups easier.

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

## License

This project is licensed under the [MIT License](LICENSE).
