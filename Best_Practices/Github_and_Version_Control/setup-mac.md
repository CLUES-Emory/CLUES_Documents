# Setup Guide: Mac Terminal

**Version 1.0 | Created: February 12, 2026**

This guide covers installing Git, Claude Code, and getting started on macOS using the Terminal. Once setup is complete, follow the main guide (`github-claude-code-manual.md`) for the standard workflow.

## Prerequisites

- macOS 10.15 (Catalina) or later
- A GitHub account
- A Claude subscription (Pro, Max, Team, or Enterprise) or an Anthropic API key

## Step 1: Install Git

Open Terminal (Applications > Utilities > Terminal, or search "Terminal" in Spotlight) and run:

```bash
xcode-select --install
```

This installs Git along with other Apple command line tools. A dialog will appear asking you to confirm — click "Install" and wait for it to finish.

If you already have Homebrew, you can also install Git with:

```bash
brew install git
```

Verify Git is installed:

```bash
git --version
```

Configure your identity:

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@emory.edu"
```

## Step 2: Install Node.js

Claude Code requires Node.js. Check if you already have it:

```bash
node --version
```

If not installed, download the LTS version from nodejs.org and run the installer. Or install via Homebrew:

```bash
brew install node
```

## Step 3: Install Claude Code

Install via npm:

```bash
npm install -g @anthropic-ai/claude-code
```

Or via Homebrew:

```bash
brew install claude-code
```

Verify the installation:

```bash
claude --version
```

If `claude` is not recognized, ensure your npm global bin directory is in your PATH:

```bash
echo 'export PATH=$(npm config get prefix)/bin:$PATH' >> ~/.zshrc
source ~/.zshrc
```

## Step 4: Launch Claude Code

Navigate to a project directory and launch:

```bash
cd path/to/your/repo
claude
```

You will be prompted to sign in the first time. After signing in, Claude Code will read the project's `CLAUDE.md` file and you can begin working.

## Cloning a Repository

```bash
git clone https://github.com/clues-lab/repo-name.git
cd repo-name
```

To update an already-cloned repo before starting new work:

```bash
git checkout main
git pull origin main
```

## Troubleshooting

### Claude Code is not recognized as a command

Make sure Node.js is installed (`node --version`). If you installed Claude Code via Homebrew, try opening a new Terminal window. If you used npm, ensure your global npm bin directory is in your PATH:

```bash
export PATH=$(npm config get prefix)/bin:$PATH
```

Add this line to your `~/.zshrc` to make it permanent, then run `source ~/.zshrc`.

### Permission errors during npm install

Do not use `sudo`. Instead, configure npm to use a user-writable directory:

```bash
mkdir -p ~/.npm-global
npm config set prefix ~/.npm-global
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.zshrc
source ~/.zshrc
```

Then reinstall Claude Code with the npm command above.

### Git asks for a password every time you push

Set up a credential helper:

```bash
git config --global credential.helper osxkeychain
```

This stores your credentials in the macOS Keychain. Alternatively, set up an SSH key with GitHub (see docs.github.com/en/authentication).

## Next Steps

You are ready to go. Open the main guide (`github-claude-code-manual.md`) and follow the Standard Workflow starting at Step 2.
