---
name: wsl-migrate
description: Migrate WSL2 distributions from the system drive (typically C:) to another drive to free up space. Use when the user wants to move WSL2 instances to a different disk, relocate WSL distributions, reduce C: drive usage, or migrate WSL2 Ubuntu/Debian/other distros to an external or secondary drive.
---

# WSL2 Distribution Migration

Migrate WSL2 distributions from the default system drive to a target drive.

## Prerequisites

- WSL2 installed with at least one distribution
- Target drive has sufficient free space (roughly 1.5x the current distribution size)
- PowerShell or Windows Command Prompt
- WSL distributions should be stopped before export

## Workflow

### 1. Inspect Current Distributions

List all WSL distributions to identify targets:

```powershell
wsl -l -v
```

Note the distribution names and their current states.

### 2. Prepare Target Directories

Create a directory on the target drive for each distribution:

```powershell
mkdir D:\WSL\Ubuntu
mkdir D:\WSL\Debian
```

Replace paths and names as appropriate for the environment.

### 3. Shut Down WSL

Ensure all distributions are fully stopped before exporting:

```powershell
wsl --shutdown
```

### 4. Export Distributions to tar Archives

Export each distribution to a temporary tar file on the target drive:

```powershell
wsl --export Ubuntu-24.04 D:\WSL\ubuntu.tar
wsl --export Debian D:\WSL\debian.tar
```

This may take several minutes for large distributions.

### 5. Import to New Location

Import each tar into the target directory with a new distribution name:

```powershell
wsl --import ubuntu D:\WSL\Ubuntu D:\WSL\ubuntu.tar
wsl --import debian D:\WSL\Debian D:\WSL\debian.tar
```

If a name collision occurs (e.g., `ERROR_ALREADY_EXISTS`), use a temporary name first, then rename after unregistering the original.

### 6. Restore Default Login User

After import, WSL defaults to `root`. Restore the original user by creating `/etc/wsl.conf` inside each new distribution:

```powershell
wsl -d ubuntu -u root -e bash -c "tee /etc/wsl.conf << 'EOF'
[user]
default=<your-linux-username>
EOF"
```

Repeat for each migrated distribution, replacing `<your-linux-username>` with the actual username.

### 7. Apply Configuration

Terminate the distribution so WSL picks up the new configuration:

```powershell
wsl --terminate ubuntu
```

Verify the default user:

```powershell
wsl -d ubuntu -e bash -c "whoami"
```

### 8. Set Default Distribution (Optional)

If the original default distribution was migrated, reassign the default:

```powershell
wsl --set-default ubuntu
```

### 9. Unregister Old Distributions

After confirming the new distributions work correctly (user login, files, tools), remove the old registrations:

```powershell
wsl --unregister Ubuntu-24.04
wsl --unregister Debian
```

### 10. Clean Up Temporary Files

Delete the exported tar archives to reclaim space:

```powershell
Remove-Item D:\WSL\ubuntu.tar
Remove-Item D:\WSL\debian.tar
```

### 11. Final Verification

List distributions to confirm the migration:

```powershell
wsl -l -v
```

## Renaming Distributions

WSL has no native rename command. To rename a distribution:

1. Export the current distribution:
   ```powershell
   wsl --export old-name D:\WSL\temp.tar
   ```
2. Unregister the old name:
   ```powershell
   wsl --unregister old-name
   ```
3. Import with the new name:
   ```powershell
   wsl --import new-name D:\WSL\new-dir D:\WSL\temp.tar
   ```
4. Restore the default user via `/etc/wsl.conf`.
5. Delete the temporary tar.

## Safety Checklist

- Do not delete tar files or unregister old distributions until the new ones are fully verified.
- Back up critical data before migration.
- Ensure the target drive has enough space for both tar files and imported distributions.
- If the import name conflicts with an existing distribution, use a temporary name and rename afterward.
