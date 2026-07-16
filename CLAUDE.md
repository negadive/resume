# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

JSON Resume data repo, not app code. Single source of truth: `resume.json`, follows the [JSON Resume schema v1.2.1](https://raw.githubusercontent.com/jsonresume/resume-schema/v1.2.1/schema.json). Everything else (HTML page, PDF) is generated from it via CI.

## Editing

- Edit `resume.json` directly. Keep it schema-valid (basics, work, education, skills, etc.).
- `resources/image.png` is referenced by absolute raw GitHub URL in `resume.json` (`basics.image`), not a relative path — must stay hosted at that path on `main` for the image to resolve.
- `exports/Teguh_Prabowo_Wijangkoro.pdf` is a generated artifact committed by CI (see below). Don't hand-edit it; changes to it should come from `resume.json` edits + the workflow run.

## Build / publish (CI only — no local build step exists)

`.github/workflows/publish_resume.yml` runs on every push to `main` (except changes limited to README, .gitignore, or `exports/**`):

1. `npm install resume-cli` + the custom theme `jsonresume-theme-stackoverflow-ibic` (negadive's fork).
2. `npx resume export --theme stackoverflow-ibic --format html public/index.html` → deployed to GitHub Pages (`gh-pages` branch) via `peaceiris/actions-gh-pages`.
3. `npx resume export --theme stackoverflow-ibic --format pdf exports/Teguh_Prabowo_Wijangkoro.pdf` → committed back to `main` as "Publishing latest PDF" and pushed via `ad-m/github-push-action`.

To preview locally, install the same tools yourself:
```
npm install resume-cli
npm install https://github.com/negadive/jsonresume-theme-stackoverflow-ibic.git
npx resume export --theme stackoverflow-ibic --format html public/index.html
```

Live site: https://negadive.github.io/resume

## Notes

- Since the PDF is auto-committed by the Actions bot on every push to main, expect an extra "Publishing latest PDF" commit to appear after your own push.
