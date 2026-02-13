# Setup Guide: Windows PowerShell

**Version 1.0 | Created: February 12, 2026**

This guide covers installing Git, Claude Code, and getting started on Windows using PowerShell. Once setup is complete, follow the main guide (`github-claude-code-manual.md`) for the standard workflow.

## Prerequisites

- Windows 10 (version 1809 or later) or Windows 11
- A GitHub account
- A Claude subscription (Pro, Max, Team, or Enterprise) or an Anthropic API key

## Step 1: Install Git

Download Git from git-scm.com and run the installer. Accept the default settings throughout the installation wizard. This installs Git and makes it available in PowerShell.

After installation, open a new PowerShell window and verify Git is installed:

```powershell
git --version
```

Configure your identity:

```powershell
git config --global user.name "Your Name"
git config --global user.email "your.email@emory.edu"
```

## Step 2: Install Node.js

Claude Code requires Node.js. Check if you already have it:

```powershell
node --version
```

If not installed, download the LTS version from nodejs.org and run the installer. Make sure the option to add Node.js to your PATH is checked during installation.

Close and reopen PowerShell after installing Node.js.

## Step 3: Install Claude Code

Open PowerShell (regular, not "Run as Administrator") and run:

```powershell
npm install -g @anthropic-ai/claude-code
```

After installation completes, close PowerShell and open a new window. This is required for the PATH changes to take effect.

Verify the installation:

```powershell
claude --version
```

If `claude` is not recognized, check that your npm global directory is in your PATH:

```powershell
npm config get prefix
```

This will return a path like `C:\Users\YourUsername\AppData\Roaming\npm`. If this path is not in your system PATH, add it:

1. Press the Windows key and search for "Environment Variables"
2. Click "Edit the system environment variables"
3. Click "Environment Variables"
4. Under "User variables", select "Path" and click "Edit"
5. Click "New" and paste the path from `npm config get prefix`
6. Click "OK" on all dialogs
7. Close and reopen PowerShell

## Step 4: Launch Claude Code

Navigate to a project directory and launch:

```powershell
cd C:\path\to\your\repo
claude
```

You will be prompted to sign in the first time. After signing in, Claude Code will read the project's `CLAUDE.md` file and you can begin working.

## Cloning a Repository

```powershell
git clone https://github.com/clues-lab/repo-name.git
cd repo-name
```

To update an already-cloned repo before starting new work:

```powershell
git checkout main
git pull origin main
```

## Windows-Specific Notes

### File paths

Windows uses backslashes in file paths (`C:\Users\...`) while Mac and Linux use forward slashes. Git and Claude Code handle this automatically in most cases. However, when writing R scripts, always use forward slashes or `here::here()` for file paths so the code works on any operating system. Never hardcode absolute paths with backslashes into shared scripts.

### Line endings

Git on Windows may convert line endings (LF to CRLF). This is usually harmless but can cause noisy diffs. If this becomes an issue, configure Git to handle it cleanly:

```powershell
git config --global core.autocrlf true
```

### WSL as a fallback

If you encounter persistent issues with the native Windows installation, Claude Code also runs through Windows Subsystem for Linux (WSL). This requires a separate setup process. Ask Doug for help if you need to go this route.

## Troubleshooting

### Claude Code is not recognized as a command

Close PowerShell and open a new window. If it still does not work, verify that your npm global directory is in your system PATH (see Step 3 above).

### Permission errors during npm install

Do not run PowerShell as Administrator. Instead, configure npm to use a user-writable directory:

```powershell
mkdir "$env:USERPROFILE\npm-global"
npm config set prefix "$env:USERPROFILE\npm-global"
```

Then add `C:\Users\YourUsername\npm-global` to your system PATH using the steps described in Step 3.

### Git asks for a password every time you push

Set up a credential helper:

```powershell
git config --global credential.helper manager
```

This uses the Windows Credential Manager to store your credentials securely. Alternatively, set up an SSH key with GitHub (see docs.github.com/en/authentication).

### PowerShell execution policy errors

If you see errors about script execution being disabled, run:

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

This allows locally created scripts to run while still requiring signatures on downloaded scripts.

## Next Steps

You are ready to go. Open the main guide (`github-claude-code-manual.md`) and follow the Standard Workflow starting at Step 2.
