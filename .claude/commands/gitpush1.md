---
description: Scan for secrets, then push code, deploy GitHub Pages, refresh the README, and update repo metadata with the live link
---

Run the following steps in order. Stop and report back if any step fails or finds a problem — do not proceed past a failed security scan.

## 1. Security scan (do this first, before anything is staged or pushed)

- Run `git status` and `git diff` (staged + unstaged) to see exactly what would be committed.
- Scan changed/new files for secrets: API keys, tokens, passwords, private keys, `.env` files, credentials JSON, connection strings, etc. Look both at filenames (e.g. `.env`, `*.pem`, `credentials.json`) and file contents (e.g. `AKIA...`, `sk-...`, `-----BEGIN PRIVATE KEY-----`).
- If anything sensitive is found: do NOT stage or push it. Tell the user exactly what was found and where, and ask how to proceed (e.g. add to `.gitignore`, remove, or use a secret manager) before continuing.
- If clean, proceed.

## 2. Push/update code to GitHub

- Check current branch and remote (`git remote -v`). If no remote exists, ask the user for the target GitHub repo before proceeding.
- Stage relevant changes (avoid `git add -A`; add specific files), commit with a concise message describing what changed and why, and push to the tracked branch.

## 3. Create/update the GitHub Pages deployment via GitHub Actions

- Check for an existing Pages workflow under `.github/workflows/`. If missing, create one using `actions/checkout`, `actions/configure-pages`, `actions/upload-pages-artifact`, and `actions/deploy-pages` to deploy the static site on push to the main branch.
- Ensure GitHub Pages is configured with the Actions build source (`gh api repos/{owner}/{repo}/pages` — POST if not yet configured, PUT to update `build_type: workflow` if it already exists with a different source).
- After pushing, confirm the workflow run succeeds (`gh run list`, `gh run watch <run-id> --exit-status`).

## 4. Create/update a professional README

- Write or refresh `README.md` covering: project name/description, live site link, key features, how to run/preview locally, project structure, and how deployment works (pointing at the Actions workflow).
- Base the content on what's actually in the repo — don't invent features or tech that aren't present.

## 5. Update the GitHub repo "About" section

- Use `gh repo edit <owner>/<repo> --description "<one-line description>" --homepage "<live Pages URL>"` to set the repo description and website link.
- Verify with `gh repo view <owner>/<repo> --json description,homepageUrl`.

## Final report

Summarize what changed: commit(s) pushed, whether the Pages workflow was created or already existed, whether the deploy run succeeded and its live URL, README changes, and the updated About section. Flag anything skipped (e.g. because no remote was configured) and what the user needs to do to unblock it.
