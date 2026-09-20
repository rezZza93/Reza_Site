# Reza Kahibavil — personal site (Quarto)

This is a [Quarto](https://quarto.org) website project: one `.qmd` (Quarto Markdown)
file per page — `index.qmd`, `research.qmd`, `teaching.qmd`, `projects.qmd`,
`cv.qmd` — plus `_quarto.yml`, which defines the navbar and ties the pages
together into one site. Quarto turns these into a full HTML site whenever you
render it.

Real files already live under `files/` (your FIN 323 slides, formula sheets,
practice questions, and resume) and are linked from `teaching.qmd` and
`cv.qmd` with normal relative links, e.g. `files/teaching/ch3/slides.pptx`.
That's the thing the Claude-hosted version couldn't do — a GitHub repo you own
has no restriction on hosting real files publicly.

## One-time setup

1. **Install Quarto**: <https://quarto.org/docs/get-started/> (adds a `quarto`
   command to your terminal).
2. **Install Git** if you don't have it: <https://git-scm.com/downloads>.
3. **Create the GitHub repo**: on github.com, click **New repository**. Name
   it anything (e.g. `reza-site`). Leave it empty (no README/license) — you're
   pushing an existing project into it.

## Option A — simplest, no CI (recommended to start)

Render locally, commit the rendered output, push. GitHub Pages just serves
the `docs/` folder as-is.

```bash
cd quarto-site          # this folder
git init
git add .
git commit -m "Initial site"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/YOUR-REPO.git
git push -u origin main
```

Then on GitHub: **Settings → Pages → Build and deployment → Source: "Deploy
from a branch"** → branch **main**, folder **/docs** → **Save**. GitHub gives
you a URL like `https://YOUR-USERNAME.github.io/YOUR-REPO/` within a minute
or two.

**Every time you edit a `.qmd` file**, re-render and push again:

```bash
quarto render
git add .
git commit -m "Update content"
git push
```

## Option B — auto-rebuild on every push (set up later, if you want it)

`.github/workflows/publish.yml` is already in this project but inactive by
default — it only runs once you switch Pages over to it. It renders the site
on GitHub's servers on every push, so you never run `quarto render` yourself.

To switch to it: **Settings → Pages → Source: "GitHub Actions"** instead of
"Deploy from a branch." From then on, just edit and push — no local render
step. (You can also add `docs/` to `.gitignore` at that point, since the
Action regenerates it and you no longer need to commit it.) Don't run both
options at once — pick one.

## Editing content later

- Text: edit the relevant `.qmd` file directly — it's just Markdown with a
  YAML header.
- New teaching material: drop the file under `files/teaching/chXX/` and add
  a link to it in the table in `teaching.qmd`.
- New paper: add it under the right heading in `research.qmd`. Once a paper
  has a real SSRN/OSF link, turn its title into a markdown link:
  `[Title](https://...)`.
- Site title, navbar items, or the placeholder `site-url` at the top of
  `_quarto.yml` — update those to match your actual GitHub username/repo
  once you know them (`site-url` isn't required, but it's used for the
  sitemap and social-preview tags).

## Custom domain (optional)

If you ever want `rezakahibavil.com` instead of the `github.io` address,
add a `CNAME` file containing just the domain to the `docs/` folder (or the
project root if using Option B) and point your domain's DNS at GitHub Pages
per <https://docs.github.com/pages/configuring-a-custom-domain-for-your-github-pages-site>.
