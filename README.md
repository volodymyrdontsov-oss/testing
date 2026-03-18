# Git Setup on Windows

## Installation

### Option 1: Official Installer (recommended)

Download from [git-scm.com](https://git-scm.com/install/windows.html) and run the installer.

### Option 2: winget

```powershell
winget install --id Git.Git -e --source winget
```

### Option 3: Chocolatey

```powershell
choco install git
```

After installation, open a **new** terminal and verify:

```powershell
git --version
```

If `git` is not recognized, the installer did not add it to your `PATH`. Reopen your terminal or add `C:\Program Files\Git\cmd` to your system `PATH` manually.

## Initial Configuration

These commands configure git globally (apply to all repositories for your user). Run them in PowerShell, Command Prompt, or Git Bash.

### Set your identity

```powershell
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

### Set the default branch name

```powershell
git config --global init.defaultBranch main
```

### Configure line endings

On Windows, use `core.autocrlf = true` so git checks out files with Windows-style line endings (CRLF) and converts them to LF on commit:

```powershell
git config --global core.autocrlf true
```

### Set up credential storage

This avoids re-entering your password on every push/pull:

```powershell
git config --global credential.helper manager
```

Git Credential Manager is bundled with Git for Windows and supports GitHub, Azure DevOps, Bitbucket, and GitLab.

## SSH Key Setup (optional, recommended)

If you prefer SSH over HTTPS for remote operations:

```powershell
ssh-keygen -t ed25519 -C "your.email@example.com"
```

The command will prompt you interactively:

| Prompt | What to do |
|--------|------------|
| `Enter file in which to save the key (.../.ssh/id_ed25519):` | Press **Enter** to accept the default location. |
| `Enter passphrase (empty for no passphrase):` | Type a passphrase for extra security, or press **Enter** to skip. |
| `Enter same passphrase again:` | Confirm the passphrase (or press **Enter** again). |

The following commands require an **Administrator** PowerShell (right-click PowerShell and choose "Run as administrator").

First, ensure the OpenSSH Client is installed (it is an optional Windows feature):

```powershell
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
```

> If it prints `State : Installed`, you already have it and can move on.

Then configure, start the SSH agent, and add your key:

```powershell
Set-Service -Name ssh-agent -StartupType Automatic
Start-Service -Name ssh-agent
ssh-add "$env:USERPROFILE\.ssh\id_ed25519"
```

> **Troubleshooting:** If `Set-Service` reports that `ssh-agent` was not found, the OpenSSH Client is not installed. Run the `Add-WindowsCapability` command above first.

Then add the public key to your GitHub/GitLab account:

```powershell
Get-Content "$env:USERPROFILE\.ssh\id_ed25519.pub" | Set-Clipboard
```

Paste the clipboard contents into your account's SSH key settings.

## Verify Everything Works

```powershell
git config --list --show-origin
```

This shows all config values and where they are set, which is useful for debugging.

## Common First Commands

```powershell
# Clone a repository
git clone https://github.com/user/repo.git

# Create a new repository
mkdir my-project
cd my-project
git init

# Stage, commit, and push
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/user/repo.git
git push -u origin main
```

## Updating Git

Git for Windows has a built-in update command:

```powershell
git update-git-for-windows
```
