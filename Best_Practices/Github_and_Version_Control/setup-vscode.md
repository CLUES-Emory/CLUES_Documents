# Setup Guide: VS Code

**Version 1.0 | Created: February 12, 2026**

This guide covers installing Git, VS Code, the Claude Code extension, and getting started on either Mac or Windows. Once setup is complete, follow the main guide (`github-claude-code-manual.md`) for the standard workflow.

VS Code is recommended if you prefer working in a visual editor rather than the command line. It provides Claude Code in a side panel alongside your files, terminal, and source control — everything in one window. The experience is identical on Mac and Windows.

## Prerequisites

- macOS 10.15+ or Windows 10/11
- A GitHub account
- A Claude subscription (Pro, Max, Team, or Enterprise) or an Anthropic API key

## Step 1: Install Git

**Mac:**

Open Terminal and run:

```bash
xcode-select --install
```

**Windows:**

Download Git from git-scm.com and run the installer. Accept the default settings.

**Both platforms — configure your identity:**

Open a terminal (Terminal on Mac, PowerShell on Windows) and run:

```
git config --global user.name "Your Name"
git config --global user.email "your.email@emory.edu"
```

## Step 2: Install Node.js

Claude Code requires Node.js even when running as a VS Code extension. Check if you already have it by opening a terminal and running:

```
node --version
```

If not installed, download the LTS version from nodejs.org and run the installer. On Windows, make sure the option to add Node.js to your PATH is checked.

## Step 3: Install VS Code

Download VS Code from code.visualstudio.com and run the installer for your platform.

After installation, open VS Code. You can verify it is working by checking that the Welcome tab appears.

## Step 4: Install the Claude Code Extension

1. Open VS Code.
2. Click the Extensions icon in the left sidebar (it looks like four squares, or press Cmd+Shift+X on Mac / Ctrl+Shift+X on Windows).
3. In the search bar, type "Claude Code".
4. Find the official extension by Anthropic and click "Install".
5. After installation, a Claude icon will appear in the left sidebar (Activity Bar).
6. Click the Claude icon to open the Claude panel. You will be prompted to sign in the first time.

The Claude panel can be repositioned by dragging its tab:

- **Secondary sidebar (right side):** Keeps Claude visible while you code. This is the most common setup.
- **Primary sidebar (left side):** Alongside the Explorer, Search, and Source Control panels.
- **Editor area:** Opens Claude as a tab next to your files, useful for side tasks.

## Cloning a Repository

1. Press Cmd+Shift+P (Mac) or Ctrl+Shift+P (Windows) to open the Command Palette.
2. Type "Git: Clone" and select it.
3. Paste the repository URL: `https://github.com/clues-lab/repo-name.git`
4. Choose a local folder to save it in.
5. When prompted, click "Open" to open the cloned repository in VS Code.

To update an already-cloned repo, open the Source Control panel (branch icon in the left sidebar) and click the sync button in the bottom status bar. Or open the VS Code terminal (Ctrl+` or Cmd+`) and run:

```
git checkout main
git pull origin main
```

## Using Claude Code in VS Code

### Starting a session

Click the Claude icon in the sidebar to open the Claude panel. Claude Code automatically knows which project you have open and will read the `CLAUDE.md` file at the start of the session.

Type your requests in natural language in the Claude panel, just as you would in the terminal version.

### Reviewing changes

When Claude makes changes to files, you can see them in two places:

- **Claude panel:** Claude will describe what it changed.
- **Source Control panel:** Click the branch icon in the left sidebar to see all changed files. Click any file to see a side-by-side diff with additions in green and deletions in red.

This visual diff view is one of the main advantages of VS Code over the terminal — you can review Claude's changes file by file before committing.

### Running multiple sessions

You can open additional Claude panels as editor tabs for side tasks while keeping your main session in the sidebar. Each session can work independently.

### Using the integrated terminal

VS Code has a built-in terminal (open with Ctrl+` or Cmd+`). You can run R scripts, Git commands, or any other terminal commands here without leaving VS Code. This is useful for testing code that Claude has written.

## Troubleshooting

### Claude Code extension does not appear after installation

Restart VS Code completely (close all windows and reopen). Verify that Node.js is installed on your system by opening the VS Code terminal and running `node --version`.

### Claude panel is empty or shows an error

Check that you are signed in. Click the Claude icon in the sidebar — if there is a sign-in prompt, follow it. If you are signed in and still see errors, try running "Claude Code: Restart" from the Command Palette (Cmd+Shift+P or Ctrl+Shift+P).

### Git operations fail in VS Code

Make sure Git is installed and available in your system PATH. Open the VS Code terminal and run `git --version`. If Git is not found, reinstall it (see Step 1) and restart VS Code.

### Permission errors

If Claude Code cannot read or write files in your project, make sure you opened the project folder directly in VS Code (File > Open Folder) rather than opening individual files. Claude Code needs access to the full project directory.

### Git asks for a password every time you push

**Mac:**
```bash
git config --global credential.helper osxkeychain
```

**Windows:**
```powershell
git config --global credential.helper manager
```

Alternatively, set up an SSH key with GitHub (see docs.github.com/en/authentication).

## Next Steps

You are ready to go. Open the main guide (`github-claude-code-manual.md`) and follow the Standard Workflow starting at Step 2.
