# Git & GitHub CLI Workflow Guide

A clean, no-nonsense guide for getting a local project onto GitHub using only the command line — SSH setup, Git init, staging, committing, and pushing. Written for Linux/macOS shells, with Windows notes throughout (Git Bash or WSL recommended).

---

## Table of Contents

1. [Status Check](#1-Status-check)
2. [Check SSH Key Setup](#2-check-ssh-key-setup)
3. [Configure Git Identity](#3-configure-git-identity)
4. [Initialize the Repository](#4-initialize-the-repository)
5. [Stage and Commit Changes](#5-stage-and-commit-changes)
6. [Connect to GitHub and Push](#6-connect-to-github-and-push)
7. [Everyday Workflow](#7-everyday-workflow)
8. [Troubleshooting](#8-troubleshooting)
9. [.gitignore Tip](#9-gitignore-tip)

---

## 1. Status Check

Check whether Git is installed:

```bash
git --version
```

- **Linux (Debian/Ubuntu):** `sudo apt update && sudo apt install git -y`
- **macOS:** `brew install git` (or install Xcode Command Line Tools)
- **Windows:** download [Git for Windows](https://git-scm.com/download/win) — this also gives you Git Bash, which supports every command in this guide.

---

## 2. Check SSH Key Setup

SSH keys let you push/pull from GitHub without typing a password every time.

### Check for an existing key

```bash
ls -al ~/.ssh
```

Look for a pair like:

```
id_ed25519
id_ed25519.pub
```

- If **both files exist**, you already have a key — skip to [Test the connection](#test-the-connection).
- If the folder doesn't exist or is empty, Copy the prompt and paste it to AI:

> I am a new github user and I want to upload code to github repos but I don't know how. I didn't setup the SSH key so please tell me the setup to show me the process to add SSH key to my computer

### Test the connection

```bash
ssh -T git@github.com
```

A successful response looks like:

```
Hi <username>! You've successfully authenticated, but GitHub does not provide shell access.
```

That message is expected and means the SSH link is working.

> **Windows note:** Git Bash handles all of the above identically. If you're using PowerShell/CMD instead, the commands are the same as long as OpenSSH is installed (it is by default on Windows 10/11).

---

## 3. Configure Git Identity

Before your first commit, tell Git who you are — this is what shows up as the commit author:

```bash
git config --global user.name "Your Name"
git config --global user.email "your_email@example.com"
```

Use the **same email** as your GitHub account so commits link to your profile.

---

## 4. Initialize the Repository

Inside your project folder:

```bash
git init
```

This creates a hidden `.git` folder that turns the directory into a Git repository. Only run this once per project.

---

## 5. Stage and Commit Changes

### Check status

```bash
git status
```

This shows two groups:
- **Untracked/modified files** — not yet staged
- **Changes staged for commit** — ready to be committed

### Stage files

```bash
git add <file_name>       # stage a single file
git add <folder_name>/    # stage an entire folder (note the trailing /)
git add .                 # stage everything in the current directory
```

### Commit

```bash
git commit -m "Your descriptive commit message"
```

A commit is a saved checkpoint. Good messages are short, present-tense, and specific — e.g. `"Add login form validation"` rather than `"update"`.

---

## 6. Connect to GitHub and Push

Create an empty repository on GitHub first (no README/license if you already have local files, to avoid merge conflicts). Then link it:

```bash
git remote add origin git@github.com:YOUR-USERNAME/Myapp.git
```

Rename your branch to `main` (GitHub's default) and push:

```bash
git branch -M main
git push -u origin main
```

The `-u` flag sets `origin main` as the default upstream, so future pushes just need `git push`.

---

## 7. Everyday Workflow

Once set up, your day-to-day loop is short:

```bash
git status
git add .
git commit -m "Describe what changed"
git push
```

---

## 8. Troubleshooting

| Problem | Likely Fix |
|---|---|
| `Permission denied (publickey)` on `ssh -T` | Key isn't added to GitHub, or `ssh-agent` doesn't have it loaded — redo [Step 2](#2-ssh-key-setup) |
| `fatal: remote origin already exists` | Run `git remote set-url origin <your-ssh-url>` instead of `add` |
| `error: failed to push some refs` | Someone else's changes exist remotely — run `git pull origin main --rebase` first |
| Git asks for a username/password instead of using SSH | Your remote URL is HTTPS, not SSH — check with `git remote -v` and fix with `git remote set-url origin git@github.com:USER/REPO.git` |

---

## 9. .gitignore Tip

Before your first commit, add a `.gitignore` file to keep junk (dependencies, build files, secrets) out of version control:

```bash
touch .gitignore
```

Common entries:

```
node_modules/
.env
__pycache__/
dist/
.DS_Store
```

GitHub maintains ready-made templates at [github.com/github/gitignore](https://github.com/github/gitignore) — grab the one matching your stack.

---

*A tidy Git history and a clean `.gitignore` are small things, but they're often the first thing a recruiter notices when they open a repo.*
