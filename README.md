# `.editorconfig` and `CONTRIBUTING.md`

Both files go in the repo root. After them I've included the README patches so the structure and contributing sections stay accurate.

---

## 1. `.editorconfig`

```ini
# https://editorconfig.org
# Supported natively by most editors; VS Code needs the "EditorConfig" extension.

root = true

[*]
charset = utf-8
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true
indent_style = space
indent_size = 4

# Markdown: two trailing spaces are meaningful (hard line break), so don't strip them
[*.md]
trim_trailing_whitespace = false

# YAML (the Pages workflow) uses 2-space indent by convention
[*.{yml,yaml}]
indent_size = 2

# JSON (e.g. .htmlvalidate.json) uses 2-space indent by convention
[*.json]
indent_size = 2

# Don't reformat lockfiles or vendored code
[{package-lock.json,yarn.lock,pnpm-lock.yaml}]
insert_final_newline = false
```

**Why 4 spaces?** Your `index.html` already uses 4-space indentation throughout, so this matches the file as it stands rather than forcing a reformat. If you'd rather standardise on 2 (more common in modern front-end), change `indent_size = 4` — but then re-indent the HTML in one separate commit so the diff stays reviewable.

---

## 2. `CONTRIBUTING.md`

```markdown
# Contributing

Thanks for taking the time to look at this project.

**A note on scope:** this repository is my personal resume. The *content*
(name, contact details, employment history, project write-ups) is specific to
me and isn't open to change. The *code* — the HTML structure, the CSS layout,
the print stylesheet, and the deployment workflow — is fair game, and I
genuinely welcome fixes and improvements to it.

So before you open a pull request, check which category your change falls into.

---

## 📖 Table of Contents

- [Ways to contribute](#ways-to-contribute)
- [Reporting a bug](#reporting-a-bug)
- [Suggesting an improvement](#suggesting-an-improvement)
- [Submitting a pull request](#submitting-a-pull-request)
- [Style guide](#style-guide)
- [Commit messages](#commit-messages)
- [What I'll likely decline](#what-ill-likely-decline)
- [Code of conduct](#code-of-conduct)

---

## Ways to contribute

| ✅ Welcome | ❌ Not applicable |
|---|---|
| Fixing a rendering bug in a specific browser | Editing my work history or personal details |
| Improving the print / PDF output | Rewriting the resume copy |
| Accessibility fixes (contrast, semantics, focus states) | Adding tracking scripts or analytics |
| Performance or markup clean-up | Changing the licence |
| Fixing the CI workflow | "Redesign the whole thing in React" |
| Typo fixes in `README.md` or comments | — |

## Reporting a bug

Open an [issue](../../issues/new) and include:

1. **What you expected** to happen.
2. **What actually happened** — a screenshot helps enormously for a
   visual project like this.
3. **How to reproduce it** — browser name and version, OS, and whether
   it's the on-screen view or the printed / PDF output.
4. **The printed output specifically:** note whether *Background graphics*
   was enabled in the print dialog, since the dark sidebar depends on it.

Please search existing issues first — someone may have hit it already.

## Suggesting an improvement

Open an issue with the `enhancement` label. Describe the problem before
the solution: *"the sidebar text is hard to read when printed in grayscale"*
is more useful than *"change #bdc3c7 to #999"*, because it lets me weigh
alternatives.

## Submitting a pull request

1. **Open an issue first** for anything beyond a trivial fix. It saves you
   building something I can't merge.
2. Fork the repository and create a branch:
   ```bash
   git checkout -b fix/print-sidebar-background
   ```
3. Make your change. Keep it focused — one concern per PR.
4. **Verify it locally before pushing:**
   ```bash
   # HTML must pass the same linter CI uses
   npx --yes html-validate@8 index.html
   ```
   Then open `index.html` in a browser and check both the screen view and
   the print preview (Ctrl/Cmd + P).
5. Push and open a PR against `main`. Describe what changed and why, and
   include before/after screenshots for anything visual.
6. CI runs the `validate` job automatically. A red check means the PR
   can't be merged — click through to the Actions log to see which rule
   failed.

## Style guide

The codebase is deliberately small, so the rules are simple:

- **Match the existing file.** `index.html` is 4-space indented; keep it
  that way. `.editorconfig` enforces this if your editor supports it.
- **No new dependencies.** The project's main selling point is that it's a
  single file with zero build steps. A PR that adds npm packages, a bundler,
  or a framework will be declined regardless of quality.
- **No inline styles** where a class will do. There's one existing exception
  (`style="margin-bottom: 10px;"` in the projects section) — feel free to
  clean that up.
- **Keep the print stylesheet working.** Any layout change must be checked
  in print preview as well as on screen. The `@media print` block hides the
  print button and preserves the sidebar colour; don't break that.
- **Prefer semantic HTML.** Use real heading levels and lists rather than
  styled `<div>`s, and give interactive elements proper focus states.
- **Don't reformat untouched code.** A PR that reindents the whole file
  makes the actual change impossible to review.

## Commit messages

Short imperative subject line, then a blank line and detail if needed:

```
Fix sidebar background disappearing when printed

The @media print block relied on print-color-adjust, which Firefox
ignores. Added an explicit background-color with !important as a fallback.
```

Prefixes I use: `fix:`, `feat:`, `docs:`, `style:`, `ci:`, `chore:`.

## What I'll likely decline

To save us both time:

- Redesigns or rewrites that change the visual identity of the resume.
- Any change to the personal content.
- Added dependencies, build tooling, or frameworks.
- Analytics, tracking, or third-party embeds.
- Changes to `LICENSE`.

None of this is personal — the project is intentionally minimal.

## Code of conduct

Be decent. Assume good faith, critique the code rather than the person, and
keep discussion on topic. I reserve the right to close issues or PRs that
don't meet that bar.

---

Thanks again for reading this far. If you're unsure whether something is in
scope, just open an issue and ask — I'd rather answer a question than have
you waste an afternoon.
```

---

## 3. README patches

### Patch 1 — Project Structure (replaces the version from my last message)

**Find:**

````markdown
## Project Structure

```
.
├── index.html                  # The entire resume (markup + styles + print script)
├── README.md                   # This file
├── LICENSE                     # MIT — covers the code, not the resume content
├── .gitignore                  # OS / editor / tooling junk
├── .htmlvalidate.json          # Optional — relaxes strict HTML linting rules
└── .github/
    └── workflows/
        └── deploy.yml          # Validates HTML, then publishes to GitHub Pages
```
````

**Replace with:**

````markdown
## Project Structure

```
.
├── index.html                  # The entire resume (markup + styles + print script)
├── README.md                   # This file
├── CONTRIBUTING.md             # Scope, style guide, and PR process
├── LICENSE                     # MIT — covers the code, not the resume content
├── .gitignore                  # OS / editor / tooling junk
├── .editorconfig               # Indentation, line endings, charset
├── .htmlvalidate.json          # Optional — relaxes strict HTML linting rules
└── .github/
    └── workflows/
        └── deploy.yml          # Validates HTML, then publishes to GitHub Pages
```

The resume itself is still a single self-contained file. Everything else is
repository scaffolding for licensing, linting, editor consistency, and
deployment.
````

### Patch 2 — the Contributing section

Your README currently has the generic open-source boilerplate ("Contributions are what make the open-source community amazing… fork, branch, PR"). For a personal resume that's misleading. Swap it for this:

**Find** the whole `## Contributing` section and replace with:

```markdown
## Contributing

This is a personal resume, so the **content** isn't open to change — but the
**code** is. Bug fixes, accessibility improvements, print-layout fixes, and
markup clean-ups are all welcome.

Please read [`CONTRIBUTING.md`](CONTRIBUTING.md) before opening a pull
request. It covers what's in scope, the style guide, and how to run the same
HTML validation that CI runs:

```bash
npx --yes html-validate@8 index.html
```

For anything beyond a trivial fix, open an issue first so we can agree on
the approach before you spend time on it.
```

### Patch 3 — one line in the Setup checklist

In the **Deploying → One-time setup** list, append a fourth item:

```markdown
4. **Optional but recommended:** protect `main` under
   **Settings → Branches → Add rule**, requiring the *validate* job to pass.
   This prevents a lint-breaking commit from reaching the live page.
```

---

## Quick ordering note

Commit these in two steps rather than one, so the history reads sensibly:

```bash
# 1. Tooling and docs
git add .editorconfig CONTRIBUTING.md LICENSE .gitignore .github/workflows/deploy.yml
git commit -m "Add licence, editor config, contributing guide, and Pages deploy workflow"

# 2. README updates
git add README.md
git commit -m "docs: document structure, deployment, and contribution scope"
```

The `deploy.yml` commit will trigger the workflow — and will fail at the `validate` step if `index.html` still doesn't exist, which is the guard doing its job. Rename the HTML file in the same push as the first commit and you'll go straight to green.
