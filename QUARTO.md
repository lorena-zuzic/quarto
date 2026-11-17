# Rendering your first Quarto page (Windows)

**Quarto** turns a plain text file (`.qmd`) into a finished document, such as a web page (`.html`), a PDF or a Word file.
You write in the `.qmd` file; Quarto produces the result. This step is called **rendering**.

```
hello.qmd  ──quarto render──▶  hello.html  (+ hello_files/ folder)
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

### 1.3 Choose an editor
Any text editor works, but **VS Code** is the most comfortable:
1. Install it from <https://code.visualstudio.com>.
2. Open VS Code, click the **Extensions** icon (four squares) on the left, search for **Quarto** and click **Install**.

---

## Part 2 – Write a page

Create a folder, e.g. `Documents/quarto-test`, and inside it a file called `hello.qmd` with this content:

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
| Everything below | The **content**, written in Markdown: `#` for headings, `**` for bold, `-` for lists |

> In VS Code, save the file as `hello.qmd`. Make sure Windows hasn't added `.txt` to the end.

---

## Part 3 – Render the page

### Option A: in Git Bash
```bash
cd ~/Documents/quarto-test
quarto render hello.qmd
```
Quarto creates two new things next to your file:

- `hello.html`: the finished page. Double-click it to open it in your browser.
- `hello_files/`: the styling and scripts the page needs. Keep it next to the `.html`, or the page will look broken.

### Option B: live preview
```bash
quarto preview hello.qmd
```
This opens the page in your browser and **updates it every time you save** the `.qmd`.
Press `Ctrl + C` in Git Bash to stop it.

### Option C: in VS Code
Open `hello.qmd` and click the **Preview** button in the top right (or press `Ctrl + Shift + K`).
The page appears in a panel next to your file and updates when you save.

---

## Part 4 – Edit and render again

1. Change something in `hello.qmd`, e.g. the title.
2. Save the file.
3. Render again (Part 3).

**Remember:** you only ever edit the `.qmd`. The `.html` is recreated every time you render, so any change you make directly in the `.html` will be overwritten.

---

## Other formats

Change the `format:` line in the header:

| Header | Result | Extra requirement |
|---|---|---|
| `format: html` | `hello.html` web page | none |
| `format: docx` | `hello.docx` Word file | none |
| `format: pdf` | `hello.pdf` | run `quarto install tinytex` once |

Or choose the format when rendering, without changing the file:
```bash
quarto render hello.qmd --to docx
```

---

## Optional: adding Python code

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

## Common problems

**`quarto: command not found`**
Git Bash was open before you installed Quarto. Close it and open it again.

**`Jupyter is not available in this Python installation`**
Your page contains Python code (or `jupyter: python3` in the header), but Jupyter isn't installed. Follow the Python section above.

**`python3: command not found` / Python opens the Microsoft Store**
On Windows the Python command is usually `py` or `python`, not `python3`. Make sure "Add python.exe to PATH" was ticked during installation.

**The page looks plain or broken when I send it to someone**
The `hello_files/` folder is missing. Send both, or add this to the header to pack everything into one file:
```yaml
format:
  html:
    embed-resources: true
```

**Nothing changed after editing**
You edited but didn't save, or you didn't render again. With `quarto preview` this happens automatically.

---

## Next step
Once a page renders, you can publish it online with GitHub Pages. See `GITHUB.md` for getting the project onto GitHub first.
