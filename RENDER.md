# Rendering your first Quarto page (Windows)

**Quarto** turns a plain text file (`.qmd`) into a finished document, such as a web page (`.html`), a PDF or a Word file.
You write in the `.qmd` file; Quarto produces the result. This step is called **rendering**.

```
index.qmd  ──quarto render──▶  index.html  (+ index_files/ folder)
 you write                      you open in a browser
```

You do Part 1 **once per computer**, then Parts 2–4 for every page.

---

## Part 1 – One-time setup

### 1.1 Install Quarto
1. Download the Windows installer from <https://quarto.org/docs/get-started/>.
2. Run it and click through with the default options.

### 1.2 Check that it works
Open **Git Bash** (close and reopen it if it was already open, so it finds the new program) and type:
```bash
quarto --version
```
You should see a version number, e.g. `1.10.19`.

---

## Part 2 – Write a page

Create a folder, e.g. `Documents/quarto-test`, and inside it a file called `index.qmd` with this content:

```markdown
---
title: "My first page"
author: "Your Name"
format: html
---

## Hello

This is **bold** and this is *italic*.

- a list item
- another item

[A link to Quarto](https://quarto.org)
```

A `.qmd` file has two parts:

| Part | What it is |
|---|---|
| Between the `---` lines | The **header** (called YAML): settings such as the title and output format |
| Everything below | The **content**, written in Markdown |

> Save the file as `index.qmd`. In Notepad, set **Save as type** to **All files**, otherwise Windows saves it as `index.qmd.txt`.
> Tip: Windows hides file extensions by default. Show them in File Explorer under **View → Show → File name extensions** (Windows 10: **View → File name extensions**).

---

## Part 3 – Render the page

### Step 1: render
In Git Bash, go to your folder and render the file:
```bash
quarto render index.qmd
```
Quarto creates two new things next to your file:

- `index.html`: the finished page.
- `index_files/`: the styling and scripts the page needs. Keep it next to the `.html`, or the page will look broken.

### Step 2: preview
```bash
quarto preview index.qmd
```
This opens the page in your browser. While it runs, the page **updates every time you save** the `.qmd`.
Press `Ctrl + C` in Git Bash to stop it.

---

## Part 4 – Edit and render again

1. Change something in `index.qmd`, e.g. the title.
2. Save the file.
3. Render and preview again (Part 3).

**Remember:** you only ever edit the `.qmd`. The `.html` is recreated every time you render, so any change you make directly in the `.html` will be overwritten.

---

## Adding Python code

Quarto can run code and put the results (numbers, tables, plots) into the page. This needs **Python** and **Jupyter**.

1. Install Python from <https://www.python.org/downloads/>. In the installer, tick **"Add python.exe to PATH"**.
2. In Git Bash, install Jupyter:
   ```bash
   py -m pip install jupyter
   ```
3. Add a code block to your `.qmd`:
   ````markdown
   ```{python}
   1 + 1
   ```
   ````
4. Render again. The page shows the code **and** its result, `2`.

Check your setup at any time with:
```bash
quarto check
```

---

## Publishing your page with GitHub Pages

GitHub can show the `.html` files in your repository as a website, at
`https://USERNAME.github.io/quarto-test/`.

Before you start:
1. Your project folder must be linked to a GitHub repository (see `GITHUB.md`, Parts 1–2).
2. Your main page must be called **`index.qmd`**. A website always opens the file called `index.html` first, so this page becomes your home page.
3. Render it and push **both** `index.html` and `index_files/`:
   ```bash
   quarto render index.qmd
   git add .
   git commit -m "First page"
   git push
   ```

### Step 1 – Make sure the repository is public
On a free GitHub account, Pages only works for public repositories.
- On your repository page, next to its name, it says **Public** or **Private**- If it says Private: **Settings** → scroll to the bottom (**Danger Zone**) → **Change visibility** → **Make public**.
- If it says Private: **Settings** → **Change visibility** → **Make public**.
### Step 2 – Switch on Pages
1. On your repository page, click **Settings** (top menu, gear icon).
2. In the left menu, click **Pages**.
3. Under **Build and deployment**:
   - **Source:** Deploy from a branch
   - **Branch:** `main`
   - **Folder:** `/ (root)`
4. Click **Save**.

### Step 3 – Wait and find your link
- GitHub now builds the site, which takes about a minute. You can watch it in the **Actions** tab: a yellow dot turns into a green tick.
- Go back to **Settings → Pages** and refresh. At the top it says *"Your site is live at …"*.

Click it: your `index` page opens.
```
https://USERNAME.github.io/quarto-test/
```

### Step 4 – Show the link on the repository page
On the repository's main page, click the gear icon next to **About** (right side). Tick **Use your GitHub Pages website** and click **Save**. The link now appears at the top of the repository.

### Updating the page
The website shows the `.html` that is on GitHub, not your `.qmd`. After each change, render and push again:
```bash
quarto render index.qmd
git add .
git commit -m "Update page"
git push
```
The website updates by itself about a minute after the push.

---

## Rendering a website with several pages

So far each `.qmd` becomes its own separate page. To make **one website** with a **menu bar** (tabs) at the top that links all your pages together, you turn the folder into a **website project**.

```
index.qmd    ─┐
about.qmd    ─┼─ quarto render ─▶  docs/  (one website, menu bar on every page)
analysis.qmd ─┘
```

### Step 1 – Create the pages
Each tab is its own `.qmd` file in the project folder:
```
index.qmd      ← home page (always opens first)
about.qmd
analysis.qmd
```
Each page keeps its own header with a `title:`. A page that uses quarto-live keeps `format: live-html` in its own header.

### Step 2 – Create `_quarto.yml`
In the project folder, create a file called **`_quarto.yml`** (note the underscore at the start). This one file holds the settings for the whole website:

```yaml
project:
  type: website
  output-dir: docs

website:
  title: "My project"
  navbar:
    left:
      - href: index.qmd
        text: Home
      - about.qmd
      - analysis.qmd

format:
  html:
    theme: cosmo
```

| Part | What it does |
|---|---|
| `type: website` | Renders all pages together as one site, with a menu bar |
| `output-dir: docs` | Puts the finished website in a folder called `docs/` (GitHub Pages can publish from this folder) |
| `title:` | The site name, shown on the left of the menu bar |
| `navbar: left:` | The tabs, in order. Each tab shows the page's `title`; use `href:` + `text:` to choose a different label |
| `format: html:` | Settings for **every** page. A page's own header can still override them |
| `theme: cosmo` | Optional colour scheme. Others: `flatly`, `darkly`, `minty`, `lux` |

> **Indentation matters.** Use spaces (not Tab), and keep things at the same level lined up.

### Step 3 – Render the whole site
Run the command **without a file name**:
```bash
quarto render
```
Quarto renders every page into the `docs/` folder.

Then preview the whole site, again **without a file name**, and click between the tabs:
```bash
quarto preview
```

### Step 4 – Tidy up
If you rendered single pages earlier, delete the old `index.html` and `index_files/` from the **main** folder. The website now lives in `docs/`.

### Step 5 – Publish on GitHub Pages
1. Create an empty file called `.nojekyll` in the project folder. It stops GitHub from processing your files with its own website tool, which can break Quarto sites:
   ```bash
   touch .nojekyll
   ```
2. Commit and push everything, including `docs/`, `_quarto.yml`, `.nojekyll` and (if you use quarto-live) `_extensions/`:
   ```bash
   git add .
   git commit -m "Website"
   git push
   ```
3. On GitHub: **Settings → Pages**. Keep the branch as **`main`**, change the folder from `/ (root)` to **`/docs`**, and click **Save**. (First time using Pages? Follow "Publishing your page with GitHub Pages" above, but choose `/docs` as the folder.)

After about a minute, `https://USERNAME.github.io/quarto-test/` opens your home page, with the menu bar leading to the other pages. You can also type a page's file name at the end, e.g. `…/about.html` for `about.qmd`.

### Updating the website
```bash
quarto render
git add .
git commit -m "Update site"
git push
```
Always use `quarto render` **without** a file name, so every page and the menu bar stay in sync.

### Adding a page later
1. Create the new `.qmd`, e.g. `results.qmd`.
2. Add it under `navbar: left:` in `_quarto.yml`.
3. Render, commit and push.

### Tabs within one page
To have clickable tabs **inside** a single page (e.g. "Code" and "Plot"), you don't need a website. Use a tabset in the `.qmd`. Each `##` heading becomes one tab:

````markdown
::: {.panel-tabset}
## Code
Some text or a code block.

## Plot
Another code block.
:::
````

---

## Common problems

Click a problem to see the solution.

<details>
<summary><b><code>quarto: command not found</code></b></summary>

Git Bash was open before you installed Quarto. Close it and open it again.

</details>

<details>
<summary><b><code>Jupyter is not available in this Python installation</code></b></summary>

Your page contains Python code (or `jupyter: python3` in the header), but Jupyter isn't installed. Follow the Python section above.

</details>

<details>
<summary><b><code>python3: command not found</code> / Python opens the Microsoft Store</b></summary>

On Windows the Python command is usually `py` or `python`, not `python3`. Make sure "Add python.exe to PATH" was ticked during installation.

</details>

<details>
<summary><b>The page looks plain or broken when I send it to someone</b></summary>

The `index_files/` folder is missing. Send both, or add this to the header to pack everything into one file:
```yaml
format:
  html:
    embed-resources: true
```

</details>

<details>
<summary><b>Nothing changed after editing</b></summary>

You edited but didn't save, or you didn't render again. Save the `.qmd`, run `quarto render`, then `quarto preview`.

</details>

<details>
<summary><b>The menu bar is missing</b></summary>

- The file must be called exactly `_quarto.yml`, with the underscore, and be in the project folder (not in a subfolder).
- Render with `quarto render` **without** a file name.
- You're looking at an old `index.html` in the main folder. Open the one in `docs/`.

</details>

<details>
<summary><b>My GitHub Pages address shows "404"</b></summary>

- Wait a minute and refresh. The first build takes a while.
- The address without a file name (`https://USERNAME.github.io/quarto-test/`) looks for a file called `index.html`. Check that your page is called `index.qmd` (not e.g. `hello.qmd`), then render it and push.
- Check that `index.html` is actually on GitHub (in the main folder for a single page, in `docs/` for a website). If it isn't, you forgot to render or to push.
- Check the Pages folder setting: `/ (root)` for a single page, `/docs` for a website.

</details>

<details>
<summary><b>My GitHub Pages site looks plain or unstyled</b></summary>

For a single page, the `index_files/` folder wasn't pushed. For a website, the `docs/` folder or `.nojekyll` wasn't pushed. Run `git status`, then `git add .`, `git commit` and `git push`.

</details>

<details>
<summary><b>A tab leads to "404" on GitHub Pages</b></summary>

- The page isn't listed in `_quarto.yml`, or wasn't rendered. Run `quarto render`, then commit and push.
- GitHub Pages is still set to `/ (root)`. Change it to `/docs` under **Settings → Pages**.
- File names are case-sensitive online: `About.qmd` and `about.qmd` are different files.

</details>

---

## Next step
To learn what else you can write in a `.qmd` file (code, hidden code, figures, interactive code), see `QUARTO.md`.
