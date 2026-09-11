# PastPaper Pro — Netlify + GitHub (No MySQL)

This version is designed for Netlify. It does NOT use PHP or MySQL.

## Two Netlify sites

### Main site
Deploy the repository root using the root `netlify.toml`.
Publish directory: `site`
Functions directory: `netlify/functions`

Connect your custom domain:
`pastpaper.pro`

### Admin site
Create a second Netlify site from the same GitHub repository, with the base directory set to:
`admin`

The admin site's `admin/netlify.toml` publishes the admin folder and its functions.
Connect:
`admin.pastpaper.pro`

There is intentionally no Admin link in the public site.

## GitHub storage

When the admin adds a paper, the admin Netlify Function writes a JSON file to GitHub automatically:

`data/papers/YEAR/CATEGORY/SUBJECT/ID.json`

Example:
`data/papers/2025/Past-Paper/Mathematics/mf8abc123.json`

Therefore redeploying the Netlify website does not erase papers. The paper records live in GitHub.

## Required Netlify environment variables

Set these on BOTH Netlify sites' functions if they need to access the repository:

- `GITHUB_OWNER` = your GitHub username/org
- `GITHUB_REPO` = repository name
- `GITHUB_BRANCH` = `main` (or your branch)
- `GITHUB_TOKEN` = a GitHub token with permission to read/write repository contents

Set these on the ADMIN Netlify site:

- `ADMIN_USERNAME` = `admin`
- `ADMIN_PASSWORD` = `PED2009`
- `ADMIN_SESSION_SECRET` = a long random secret

The code has `admin` / `PED2009` as a fallback, but you should still set the environment variables before production, especially for a public repository.

## Important

The GitHub token must NEVER be put into browser JavaScript. It is only read by Netlify Functions through environment variables.

## Custom subdomain

Your DNS/domain provider should point:
- `pastpaper.pro` → the main Netlify site
- `admin.pastpaper.pro` → the admin Netlify site

Use the DNS records Netlify gives you for your exact domain setup.

## Download behavior

The public Download button calls the site's Netlify Function, which looks up the saved paper URL and redirects the browser to the actual file URL. This means users do not need to open Telegram or visit the admin site.
