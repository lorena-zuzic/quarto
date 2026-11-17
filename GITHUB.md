# Getting started with Git and GitHub (Windows)

**Git** is a program on your computer that saves snapshots ("commits") of a project folder.
**GitHub** is a website that stores a copy of that folder online, so you can back it up, share it and publish it.

You do Part 1 **once per computer**, Part 2 **once per project**, and Part 3 **every time you work**.
Part 4 shows how to turn your project into a website with GitHub Pages.

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
2. Give it a name, e.g. `myproject`.
3. Choose **Public** (needed for free GitHub Pages websites).
4. Leave it **empty**: do **not** tick README, .gitignore or license.
5. Click **Create repository**. Keep this page open; it shows the repository address.

### 2.2 On your computer (in Git Bash)

**Go to your project folder.**
```bash
cd ~/Documents/myproject
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
git remote add origin https://github.com/USERNAME/myproject.git
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

Think of it as: **select → save → upload**.

---

## Part 4 – Publish a web page with GitHub Pages

GitHub can show the `.html` files in your repository as a website, at
`https://USERNAME.github.io/REPOSITORY/`.

Before you start, name your main page **`index.qmd`**. A website always opens the file called `index.html` first, so this page becomes your home page.

Then render and push. How depends on what you are publishing (see `RENDER.md`):

| You are publishing… | Render with | Push | GitHub Pages folder |
|---|---|---|---|
| **A single page** | `quarto render index.qmd` | `index.html` and `index_files/` | `/ (root)` |
| **A website with several pages** (with `_quarto.yml`) | `quarto render` | `docs/`, `_quarto.yml`, `.nojekyll` (and `_extensions/` if you use quarto-live) | `/docs` |

### 4.1 Make sure the repository is public
On a free GitHub account, Pages only works for public repositories.
- On your repository page, next to its name, it says **Public** or **Private**.
- If it says Private: **Settings** → scroll to the bottom (**Danger Zone**) → **Change visibility** → **Make public**.

### 4.2 Switch on Pages
1. On your repository page, click **Settings** (top menu, gear icon).
2. In the left menu, click **Pages**.
3. Under **Build and deployment**:
   - **Source:** Deploy from a branch
   - **Branch:** `main`
   - **Folder:** `/ (root)` for a single page, or **`/docs`** for a website with several pages
4. Click **Save**.

### 4.3 Wait and find your link
- GitHub now builds the site, which takes about a minute. You can watch it in the **Actions** tab: a yellow dot turns into a green tick.
- Go back to **Settings → Pages** and refresh. At the top it says *"Your site is live at …"*.

Click it: your `index` page opens.
```
https://USERNAME.github.io/myproject/
```
For a website with several pages, use the menu bar to reach the other pages. You can also type a page's file name at the end, e.g. `https://USERNAME.github.io/myproject/about.html` for `about.qmd`.

### 4.4 Optional: show the link on the repository page
On the repository's main page, click the ⚙️ next to **About** (right side). Tick **Use your GitHub Pages website** and click **Save**. The link now appears at the top of the repository.

### 4.5 Updating the page
The website shows the `.html` that is on GitHub, not your `.qmd`. After each change, render and push again:
```bash
quarto render index.qmd     # single page
quarto render               # website with several pages
git add .
git commit -m "Update page"
git push
```
The website updates by itself about a minute after the push.

---

## Handy checks

| Command | Question it answers |
|---|---|
| `git status` | What has changed since my last commit? |
| `git log --oneline` | What commits have I made? |
| `git remote -v` | Which GitHub repository is this folder linked to? |
| `git branch` | Which branch am I on? |

---

## Common problems

Click a problem to see the solution.

<details>
<summary><b><code>Password authentication is not supported for Git operations</code></b></summary>

GitHub no longer accepts your account password in the terminal. Use the browser sign-in window that Git for Windows shows. If it doesn't appear, reinstall Git for Windows and keep the default "Git Credential Manager" option, or use SSH (see below).

</details>

<details>
<summary><b><code>error: src refspec main does not match any</code></b></summary>

Either you haven't made a commit yet (do `git add .` and `git commit` first), or your branch is called `master`. Check with `git branch`, and rename it with:
```bash
git branch -m master main
```

</details>

<details>
<summary><b><code>fatal: not a git repository</code></b></summary>

You are in the wrong folder. Use `pwd` to see where you are and `cd` to go to your project folder.

</details>

<details>
<summary><b>My GitHub Pages address shows "404"</b></summary>

- Wait a minute and refresh. The first build takes a while.
- The address without a file name (`https://USERNAME.github.io/myproject/`) looks for a file called `index.html`. Check that your page is called `index.qmd` (not e.g. `hello.qmd`), then render it and push.
- Check that `index.html` is actually on GitHub (in the main folder for a single page, in `docs/` for a website). If it isn't, you forgot to render or to push.
- Check the Pages folder setting: `/ (root)` for a single page, `/docs` for a website.

</details>

<details>
<summary><b>My GitHub Pages site looks plain or unstyled</b></summary>

For a single page, the `index_files/` folder wasn't pushed. For a website, the `docs/` folder or `.nojekyll` wasn't pushed. Run `git status`, then `git add .`, `git commit` and `git push`.

</details>

<details>
<summary><b><code>warning: LF will be replaced by CRLF</code></b></summary>

Harmless. Windows and Mac/Linux mark line endings differently, and Git is converting them for you.

</details>

<details>
<summary><b>The commit opens a strange text editor</b></summary>

You forgot `-m "message"`. If you're stuck in it, type `:q!` and press Enter, then run the commit again with `-m`.

</details>

---

## Optional: SSH instead of HTTPS

SSH uses a key file on your computer instead of the browser sign-in. It is useful on computers without a browser (e.g. servers).

1. Create a key (press Enter at every question):
   ```bash
   ssh-keygen -t ed25519 -C "you@example.com"
   ```
2. Copy the public key to the clipboard:
   ```bash
   clip < ~/.ssh/id_ed25519.pub
   ```
3. On GitHub: **Settings → SSH and GPG keys → New SSH key**, paste, and save.
4. Test it (type `yes` if asked whether to continue connecting):
   ```bash
   ssh -T git@github.com
   ```
   You should see *"Hi USERNAME! You've successfully authenticated"*.
5. Use the SSH address instead of HTTPS:
   ```bash
   git remote set-url origin git@github.com:USERNAME/myproject.git
   ```
