# Getting started with Git and GitHub (Windows)

**Git** is a program on your computer that saves snapshots ("commits") of a project folder.
**GitHub** is a website that stores a copy of that folder online, so you can back it up, share it and publish it.

You do Part 1 **once per computer**, Part 2 **once per project**, and Part 3 **every time you work**.
To publish your project as a website with GitHub Pages, see `RENDER.md`.

---

## Part 1 – One-time setup

### 1.1 Create a GitHub account
Go to <https://github.com> and sign up. Remember your **username**: you will need it later.

### 1.2 Install Git for Windows
1. Download it from <https://git-scm.com/download/win>.
2. Run the installer. **Clicking "Next" on every screen is fine.**
3. When it's done, open the Start menu and launch **Git Bash**.

> Use **Git Bash** for all commands in this guide. It understands the same commands as Mac and Linux, so tutorials online will work as written.

### 1.3 Tell Git who you are
Type these in Git Bash and press Enter after each. Use your own name and the email you used for GitHub.

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
```

- The first two lines: your name and email are attached to every commit you make.
- The last line: new projects will use the branch name `main`, which is what GitHub expects.

Check that it worked:
```bash
git config --global --list
```

---

## Part 2 – Create a project and link it to GitHub

### 2.1 On GitHub (in the browser)
1. Click the **+** in the top right, then **New repository**.
2. Give it the same name as your project folder, e.g. `quarto-test`.
3. Choose **Public** (needed for free GitHub Pages websites).
4. Leave it **empty**: do **not** tick README, .gitignore or license.
5. Click **Create repository**. Keep this page open; it shows the repository address.

### 2.2 On your computer (in Git Bash)

**Go to your project folder.**
```bash
cd ~/Documents/quarto-test
```
`~` means your home folder (`C:\Users\YourName`). Use forward slashes `/`, not backslashes.
Tip: in File Explorer you can right-click inside a folder → **Open Git Bash here** (on Windows 11: **Show more options** first).

**Turn the folder into a Git repository.**
```bash
git init
```

**Choose which files to save.** The dot means "everything in this folder".
```bash
git add .
```

**Save a snapshot with a short description.**
```bash
git commit -m "First commit"
```

**Link the folder to your GitHub repository.** Copy the **HTTPS** address from the GitHub page (it ends in `.git`).
```bash
git remote add origin https://github.com/USERNAME/quarto-test.git
```

**Upload to GitHub.**
```bash
git push -u origin main
```
The first time, a window opens asking you to **sign in to GitHub in your browser**. Do that, and the upload continues.
Windows remembers the login, so you only do this once.
The `-u` means that from now on, plain `git push` is enough.

Refresh the GitHub page: your files are there.

---

## Part 3 – Everyday use

After you change files:

```bash
git add .
git commit -m "Describe what you changed"
git push
```

---

## Handy checks

| Command | Question it answers |
|---|---|
| `git status` | What has changed since my last commit? |
| `git log --oneline` | What commits have I made? |
| `git remote -v` | Which GitHub repository is this folder linked to? |
| `git branch` | Which branch am I on? |


