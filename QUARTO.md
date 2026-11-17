# Editing Quarto documents (.qmd)

This guide shows what you can write **inside** a `.qmd` file.
To turn the file into a web page, see `RENDER.md`.

> **How to read the examples:** each grey box shows what to type in your `.qmd` file. Copy only what is **inside** the box.

A `.qmd` file always has two parts:

````markdown
---
title: "My page"
format: html
---

Everything below the header is the content.
````

- **Header** (between the `---` lines): settings for the whole document.
- **Content**: text written in Markdown, plus code blocks.

---

## 1 – Text

Quarto uses **Markdown**: plain text with a few symbols for formatting.

### Headings
````markdown
# Big heading
## Smaller heading
### Even smaller
````
### Bold, italic, code
````markdown
This is **bold**, this is *italic*, and this is `code`.
````

### Paragraphs
Leave an **empty line** between paragraphs. A single line break is ignored.

````markdown
This is the first paragraph.

This is the second paragraph.
````

### Lists
````markdown
- an item
- another item
  - an indented item (two spaces in front)

1. first
2. second
````
Leave an empty line before and after a list.

### Links and images
````markdown
[Quarto website](https://quarto.org)

![A short caption](images/photo.png)
````
The image path is relative to the `.qmd` file. Here the picture is in a folder called `images` next to it.

### Maths
````markdown
Inline: $E = mc^2$

On its own line:

$$
\bar{x} = \frac{1}{n}\sum_{i=1}^{n} x_i
$$
````

### Notes to yourself (hidden from the page)
````markdown
<!-- This comment does not appear on the rendered page. -->
````

---

## 2 – Making code work

### 2.1 What you need
Python code needs **Python** and **Jupyter** installed (see `RENDER.md`, "Adding Python code").
Packages you use, e.g. `numpy` or `matplotlib`, must be installed too:
```bash
py -m pip install numpy matplotlib
```
(On Mac/Linux: `python3 -m pip install …`)

### 2.2 Tell Quarto to use Python
Add `jupyter: python3` to the header:

````markdown
---
title: "My page"
format: html
jupyter: python3
---
````

### 2.3 Write a code block
A code block starts with three backticks and `{python}`, and ends with three backticks:

````markdown
```{python}
x = 5
print(x * 2)
```
````

When you render, Quarto **runs** the code and shows the code **and** its result (`10`) on the page.

> Without the curly brackets (` ```python `) the code is only **shown**, never run.

### 2.4 Check your code
Code only runs when you render. After each change:
1. Save the `.qmd`.
2. Run `quarto render index.qmd`.
3. Run `quarto preview index.qmd` and look at the result.

If the code has an error, rendering stops and Git Bash shows the error message and the line it happened on.

### 2.5 Blocks share their variables
Code blocks run from top to bottom, like one long script. A variable made in one block can be used in the next:

````markdown
```{python}
name = "Ana"
```

Some text in between.

```{python}
print("Hello", name)
```
````

---

## 3 – Showing or hiding code

You control each code block with **options**: lines starting with `#|` at the very top of the block.

### 3.1 Options for one block

| Option | Effect |
|---|---|
| `#| echo: false` | Hide the code, show the result |
| `#| output: false` | Show the code, hide the result |
| `#| include: false` | Hide both, but still run the code (e.g. for loading data) |
| `#| eval: false` | Show the code, but **don't run** it |
| `#| code-fold: true` | Hide the code behind a clickable **▶ Code** button |

Example: the reader sees only the result, `10`.
````markdown
```{python}
#| echo: false
x = 5
print(x * 2)
```
````

Example: the code is folded away, with a custom button text.
````markdown
```{python}
#| code-fold: true
#| code-summary: "Show the code"
x = 5
print(x * 2)
```
````

### 3.2 Options for the whole document
Put them in the header instead, and they apply to every block:

````markdown
---
title: "My page"
jupyter: python3
format:
  html:
    code-fold: true
execute:
  echo: true
---
````

- `code-fold` goes under `format: html:` because it only exists for web pages.
- Options that control running and showing code (`echo`, `eval`, `output`, …) go under `execute:`.
- An option inside a block **overrides** the header for that block.

> **Indentation matters in the header.** Use spaces (not Tab), and keep the same number of spaces for things at the same level.

---

## 4 – Labelling figures

### 4.1 A figure made by code
Give the block a `label` that **starts with `fig-`**, and a caption with `fig-cap`:

````markdown
```{python}
#| label: fig-squares
#| fig-cap: "Squares of the numbers 1 to 5."
#| echo: false
import matplotlib.pyplot as plt

x = [1, 2, 3, 4, 5]
plt.plot(x, [i**2 for i in x])
plt.show()
```
````

The figure is now numbered automatically, with the caption *"Figure 1: Squares of the numbers 1 to 5."*

### 4.2 Refer to the figure in your text
Use `@` plus the label:

````markdown
As @fig-squares shows, the values grow quickly.
````
This becomes *"As Figure 1 shows, …"*, with a link to the figure. If you add another figure earlier in the page, the numbers update by themselves.

### 4.3 A figure from an image file
Add `{#fig-…}` after the image:

````markdown
![A photo of the lab.](images/lab.png){#fig-lab}

See @fig-lab.
````

### 4.4 Useful extra options

| Option | Effect |
|---|---|
| `#| fig-alt: "Line rising steeply"` | Description for screen readers |
| `#| fig-width: 6` | Width in inches |
| `#| fig-height: 4` | Height in inches |

> Labels may only contain letters, numbers and `-`. **No spaces or underscores**: `fig-my-plot` works, `fig my_plot` does not.

---

## 5 – Code readers can edit and run

Normally the reader only **sees** your code and results. With the **quarto-live** extension, readers can **change the code and run it in their browser**: no Python needed on their computer.

### 5.1 Install the extension (once per project)
In Git Bash, inside your project folder:
```bash
quarto add r-wasm/quarto-live
```
Answer `Y` to the questions. This creates an `_extensions/` folder in your project.

### 5.2 Change the header
Use `format: live-html`, and remove the `jupyter: python3` line:

````markdown
---
title: "Try it yourself"
format: live-html
---
````

### 5.3 Use `{pyodide}` instead of `{python}`
````markdown
```{pyodide}
x = 5
print(x * 2)
```
````

On the rendered page this block becomes a small editor with a **Run Code** button. Readers can change `5` to another number and click Run.

### 5.4 Things to know
- **Open it through a web address**, not by double-clicking the `.html` file. Use `quarto preview index.qmd` on your computer, or the GitHub Pages link once it's online.
- **The first run is slow** (a few seconds): the browser downloads Python. It needs an internet connection.
- **Push the `_extensions/` folder to GitHub** along with everything else, or the page won't render properly.
- `numpy`, `pandas` and `matplotlib` work out of the box. Some other packages are not available in the browser.
- Normal `{python}` blocks and `{pyodide}` blocks run in different places (your computer vs. the reader's browser) and **don't share variables**.

More options are in the quarto-live documentation: <https://r-wasm.github.io/quarto-live/>

---

## Common problems

Click a problem to see the solution.

<details>
<summary><b>My code block appears as plain text and doesn't run</b></summary>

- Check the curly brackets: ` ```{python} `, not ` ```python `.
- Check that the block isn't inside another block (e.g. you copied the outer box from a guide).
- Check that the header has `jupyter: python3`.

</details>

<details>
<summary><b><code>ModuleNotFoundError: No module named 'numpy'</code></b></summary>

The package isn't installed. Run `py -m pip install numpy` (or the missing package's name).

</details>

<details>
<summary><b>My <code>#|</code> options are ignored</b></summary>

They must be the **first lines** of the block, with no empty line before them, and a space after `#|`.

</details>

<details>
<summary><b><code>@fig-…</code> shows as plain text or <code>?@fig-…</code></b></summary>

The label is wrong: it must start with `fig-`, match exactly, and contain no spaces or underscores. The figure also needs a `fig-cap`.

</details>

<details>
<summary><b><code>YAML parse error</code> when rendering</b></summary>

Something is wrong in the header, usually indentation or a missing space after a `:`.

</details>

<details>
<summary><b>The quarto-live editor doesn't appear or never finishes loading</b></summary>

You opened the file by double-clicking it. Use `quarto preview` or the GitHub Pages link instead.

</details>
