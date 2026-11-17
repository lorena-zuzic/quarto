# Quarto websites with GitHub: a beginner's guide

By the end you will have:
- a project folder called `quarto-test` with Quarto pages (`.qmd` files)
- a copy of it on GitHub
- a website at `https://USERNAME.github.io/quarto-test/`

## The guides

| File | What it covers |
|---|---|
| [GITHUB.md](GITHUB.md) | Installing Git, creating a GitHub repository, uploading your work |
| [RENDER.md](RENDER.md) | Installing Quarto, turning `.qmd` files into web pages, publishing them |
| [QUARTO.md](QUARTO.md) | What you can write inside a `.qmd` file: text, code, figures |

## The logic

Once everything is set up, every change follows the same loop:

```bash
# 1. edit and save your .qmd files
quarto render        # 2. render
quarto preview       # 3. check the result in your browser (Ctrl + C to stop)
git add .            # 4. upload to GitHub
git commit -m "Describe what you changed"
git push
```

The website updates by itself about a minute after the push.
