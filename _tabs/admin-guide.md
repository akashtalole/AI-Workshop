---
layout: page
title: Admin Guide
icon: fas fa-cogs
order: 6
---

# AI-Workshop Admin Guide

This guide is for the platform owner and maintainers. It covers initial deployment, configuration, content management, and ongoing operations.

---

## Table of Contents

1. [Initial Deployment](#1-initial-deployment)
2. [GitHub Pages Configuration](#2-github-pages-configuration)
3. [Giscus Comments Setup](#3-giscus-comments-setup)
4. [Analytics Setup](#4-analytics-setup)
5. [Theme Configuration](#5-theme-configuration)
6. [Content Management](#6-content-management)
7. [GitHub Issues & Discussions Management](#7-github-issues--discussions-management)
8. [GitHub Projects — Roadmap Board](#8-github-projects--roadmap-board)
9. [CI/CD Pipeline](#9-cicd-pipeline)
10. [Updating the Theme](#10-updating-the-theme)
11. [Security & Access Control](#11-security--access-control)
12. [Troubleshooting Deployments](#12-troubleshooting-deployments)
13. [Checklist: Launch Readiness](#13-checklist-launch-readiness)

---

## 1. Initial Deployment

### Prerequisites

- Ruby 3.0+ installed locally (for testing builds before push)
- Bundler: `gem install bundler`
- Git access to `akashtalole/AI-Workshop`

### Local Development Setup

```bash
# Clone the repository
git clone https://github.com/akashtalole/AI-Workshop.git
cd AI-Workshop

# Install Ruby dependencies
bundle install

# Start local dev server with live reload
bundle exec jekyll serve --livereload

# Browse to http://localhost:4000/AI-Workshop/
```

### Production Build Test

Before pushing to `main`, verify the production build passes htmlproofer:

```bash
JEKYLL_ENV=production bundle exec jekyll build
bundle exec htmlproofer _site \
  --disable-external \
  --ignore-urls "/^http:\/\/127.0.0.1/,/^http:\/\/0.0.0.0/,/^http:\/\/localhost/"
```

If htmlproofer fails with broken links, fix the links before pushing — the CI pipeline will also fail.

---

## 2. GitHub Pages Configuration

### Step 1: Set Pages Source to GitHub Actions

1. Go to `https://github.com/akashtalole/AI-Workshop/settings/pages`
2. Under **Build and deployment → Source**, select **GitHub Actions**
3. Do NOT select "Deploy from a branch" — the workflow handles deployment

### Step 2: Configure Actions Permissions

1. Go to **Settings → Actions → General**
2. Under **Workflow permissions**:
   - Select **Read and write permissions**
   - Check **Allow GitHub Actions to create and approve pull requests**
3. Save

### Step 3: Trigger the First Deployment

The workflow triggers on push to `main` or `master`. To deploy:

```bash
# Merge or push to main
git checkout main
git merge claude/ai-workshop-platform-AyD5a
git push origin main
```

Then: **Actions tab** → watch the "Build and Deploy" workflow run → after ~2 minutes, the site will be live at `https://akashtalole.github.io/AI-Workshop/`.

### Step 4: Verify the Deployment

Check all critical pages:

| URL | Expected Result |
|-----|----------------|
| `/AI-Workshop/` | Home page, pinned orientation post visible |
| `/AI-Workshop/missions/` | Full mission directory table |
| `/AI-Workshop/categories/` | Recruit, Operative, Commander, Special-Ops categories |
| `/AI-Workshop/posts/recruit-00-orientation/` | Mission page with TOC and Giscus comment box |
| `/AI-Workshop/about/` | About page renders |

### Custom Domain (Optional)

To use a custom domain (e.g., `ai-workshop.dev`):

1. Add a `CNAME` file to the repo root: `echo "ai-workshop.dev" > CNAME`
2. Configure DNS: add a CNAME record pointing to `akashtalole.github.io`
3. In **Settings → Pages**, enter the custom domain and enable **Enforce HTTPS**
4. Update `_config.yml`: set `url: "https://ai-workshop.dev"` and `baseurl: ""`

---

## 3. Giscus Comments Setup

Comments use [Giscus](https://giscus.app) — a comments widget powered by GitHub Discussions. Each mission page automatically creates a Discussion thread on first visit.

### Step 1: Enable GitHub Discussions

1. Go to **Settings → Features**
2. Check **Discussions**
3. Save

### Step 2: Install the Giscus GitHub App

1. Visit `https://github.com/apps/giscus`
2. Click **Install**
3. Select **Only select repositories** → choose `akashtalole/AI-Workshop`
4. Confirm

### Step 3: Create the Comments Discussion Category

1. Go to the **Discussions** tab of the repository
2. Click the pencil icon (Manage categories)
3. Ensure a category named `General` exists with format **Open-ended discussion**
4. If it doesn't exist, create it

### Step 4: Get Your Giscus IDs

1. Visit `https://giscus.app`
2. In **Repository**, enter: `akashtalole/AI-Workshop`
3. Under **Discussion Category**, select: `General`
4. Under **Page ↔ Discussions Mapping**, select: `pathname`
5. Scroll down — a `<script>` block is generated. Extract:
   - `data-repo-id="R_kgDO..."` → your `repo_id`
   - `data-category-id="DIC_kwDO..."` → your `category_id`

### Step 5: Update `_config.yml`

```yaml
comments:
  provider: giscus
  giscus:
    repo: akashtalole/AI-Workshop
    repo_id: R_kgDO...        # paste value from Step 4
    category: General
    category_id: DIC_kwDO...  # paste value from Step 4
    mapping: pathname
    strict: 0
    reactions_enabled: 1
    input_position: bottom
    theme: preferred_color_scheme
    lang: en
```

Push to `main` to deploy the change. Comments will appear on all mission pages.

### Managing Discussion Threads

Each mission page creates one Discussion thread (when first visited). To manage comments:
- Go to the **Discussions** tab
- Use **Mark as answered**, **Lock**, or **Delete** as needed
- Pin important discussions to the Discussions homepage

---

## 4. Analytics Setup

### Google Analytics 4

1. Go to [analytics.google.com](https://analytics.google.com) → create a new property
2. Set up a Web data stream for `https://akashtalole.github.io`
3. Copy the **Measurement ID** (format: `G-XXXXXXXXXX`)
4. Update `_config.yml`:

```yaml
analytics:
  google:
    id: G-XXXXXXXXXX
```

5. Push to `main`. Analytics tracking begins on next page load.

Key reports to monitor:
- **Engagement → Pages and screens** — which missions get the most views
- **Acquisition → Traffic acquisition** — how users find the platform
- **Engagement → Events → scroll** — how far users read

### GoatCounter (Privacy-First Alternative)

For GDPR-compliant analytics without cookies:

1. Create a free account at [goatcounter.com](https://www.goatcounter.com)
2. Set the site code (e.g., `ai-workshop`)
3. Update `_config.yml`:

```yaml
analytics:
  goatcounter:
    id: ai-workshop
```

---

## 5. Theme Configuration

The Chirpy theme is configured entirely through `_config.yml`. Key sections and how to change them:

### Site Identity

```yaml
title: AI-Workshop               # Site name in browser tab and header
tagline: Learn AI by Doing       # Subtitle below site name
description: >-                  # Used for SEO meta description
  A hands-on AI workshop...
avatar: /assets/img/avatar.png   # Sidebar profile image (200×200 px minimum)
```

To update the avatar: replace `assets/img/avatar.png` with a new square PNG.

### Favicon

Chirpy looks for favicons in `assets/img/favicons/`. To generate a complete favicon set:

1. Create a 512×512 PNG logo
2. Upload to [realfavicongenerator.net](https://realfavicongenerator.net)
3. Download and extract the generated files into `assets/img/favicons/`
4. Required files:
   - `favicon.ico`, `favicon.svg`, `favicon-96x96.png`
   - `apple-touch-icon.png`
   - `web-app-manifest-192x192.png`, `web-app-manifest-512x512.png`
   - `site.webmanifest`

### Social Links

```yaml
social:
  name: Akash Talole
  links:
    - https://github.com/akashtalole
    - https://twitter.com/yourtwitterhandle    # optional
    - https://linkedin.com/in/yourprofile      # optional
```

### Dark/Light Mode Default

Leave `theme_mode` blank to allow users to toggle. Set it to force a mode:

```yaml
theme_mode:         # blank = user chooses
# theme_mode: light
# theme_mode: dark
```

### Custom CSS

All CSS customizations go in `assets/css/jekyll-theme-chirpy.scss`. The file already contains mission badge styles. To add more:

```scss
---
---

@import "main
{%- if jekyll.environment == 'production' -%}
  .bundle
{%- endif -%}
";

/* Your custom CSS here */
.my-custom-class {
  color: red;
}
```

---

## 6. Content Management

### Adding a New Mission

1. Create a file in `_posts/` with format `YYYY-MM-DD-slug.md`
2. Required front matter:

```yaml
---
title: "TRACK-NN: Mission Title"
date: 2026-05-01 09:00:00 +0530
categories: [TrackName, SubCategory]
tags: [tag1, tag2]
description: "One sentence for SEO and post card preview."
toc: true
author: akashtalole
---
```

3. Follow the standard mission structure:
   - Mission Brief (track, time, prerequisites)
   - Learning Objectives
   - Background/concepts
   - Hands-On Lab (numbered steps)
   - Mission Complete (checklist)
   - Navigation links

4. Update `_tabs/missions.md` to add the new mission to the directory table
5. Push to `main` — the site auto-deploys

### Category Naming Convention

| Track | Category Array | Example |
|-------|--------------|---------|
| Recruit | `[Recruit, Foundations]` or `[Recruit, Techniques]` | Conceptual missions use Foundations; code-heavy use Hands-On |
| Operative | `[Operative, Agents]` or `[Operative, Advanced]` or `[Operative, Safety]` | |
| Commander | `[Commander, Architecture]` | |
| Special Ops | `[Special-Ops, Tools]` or `[Special-Ops, Security]` | |

### Tagging Guidelines

Tags are lowercase, hyphen-separated. Use existing tags when possible (check the [Tags page](/tags/)):

Common tags: `claude-api`, `prompt-engineering`, `agents`, `tool-use`, `rag`, `embeddings`, `multi-agent`, `ai-safety`, `mcp`, `claude-code`, `streaming`, `python`, `llm`

### Drafts

To write a mission without publishing it:

1. Place the file in `_drafts/` (no date prefix needed)
2. To preview drafts locally: `bundle exec jekyll serve --drafts`
3. When ready to publish: move to `_posts/` and add the date prefix

### Pinning Posts

Set `pin: true` in front matter to pin a post to the top of the home page. Only pin the orientation/entry-point post. Too many pinned posts clutters the home page.

### Updating Existing Missions

Just edit the file and push to `main`. The `last_modified_at` timestamp is automatically computed from git history (requires `fetch-depth: 0` in the workflow, which is already set).

---

## 7. GitHub Issues & Discussions Management

### Issue Triage

New issues arrive via the three issue templates. Recommended workflow:

| Template | Label | Action |
|----------|-------|--------|
| Mission Feedback | `feedback` | Review, apply improvements, close with "fixed in [post]" |
| Bug Report | `bug` | Reproduce, fix, close with commit reference |
| Feature Request | `enhancement` | Add to GitHub Project board → Planned column |

### Labeling Strategy

Default labels to create beyond the templates:

| Label | Color | Use |
|-------|-------|-----|
| `recruit` | `#28a745` | Issues about Recruit track |
| `operative` | `#007bff` | Issues about Operative track |
| `special-ops` | `#fd7e14` | Issues about Special Ops |
| `good first issue` | `#7057ff` | Easy contribution opportunities |
| `help wanted` | `#008672` | Needs community contribution |
| `content` | `#e4e669` | Mission content changes |

### Closing Issues

Always close issues with a reference:
- **Fixed:** "Fixed in [commit abc123](link) — the code example in Step 4 now uses the correct parameter name."
- **Won't fix:** "This is by design — [explanation]."
- **Duplicate:** "Duplicate of #12."

### Discussion Moderation

In the Discussions tab:
- **Mark answer** on Q&A threads once resolved
- **Lock** threads that have been resolved but keep attracting off-topic replies
- **Delete** spam or abusive content
- **Pin** important announcements to the Discussions home (e.g., "Commander Track launching next month")

---

## 8. GitHub Projects — Roadmap Board

### Create the Mission Roadmap

1. Go to `https://github.com/akashtalole/AI-Workshop/projects`
2. Click **New project** → **Board**
3. Name it: **AI-Workshop Mission Roadmap**
4. Create columns:

| Column | Contents |
|--------|---------|
| **Ideas** | Feature requests from issues, community suggestions |
| **Planned** | Missions confirmed for development |
| **In Progress** | Missions currently being written |
| **Review** | Missions ready for review before publishing |
| **Published** | Live missions (archive) |

### Linking Issues to the Board

When a Feature Request issue is opened:
1. Add it to the **Ideas** column
2. When you decide to build it: move to **Planned**, add estimated publish date
3. When you start writing: move to **In Progress**
4. When the PR is open: move to **Review**
5. After merging: move to **Published**

---

## 9. CI/CD Pipeline

The GitHub Actions workflow at `.github/workflows/pages-deploy.yml` handles all deployments.

### Workflow Overview

```
push to main
    └─► build job
            ├─ Checkout (fetch-depth: 0 for git history)
            ├─ Configure Pages (sets base_path)
            ├─ Setup Ruby 3.3 + bundle install
            ├─ Jekyll build (JEKYLL_ENV=production)
            ├─ htmlproofer (internal link check)
            └─ Upload artifact
    └─► deploy job (needs: build)
            └─ actions/deploy-pages
```

### Triggering Manually

If you need to redeploy without a code change:
1. Go to **Actions → Build and Deploy**
2. Click **Run workflow** → **Run workflow** (on the `main` branch)

### Monitoring Deployments

- **Actions tab** → watch live log output for each job
- **Environments tab** → see deployment history and active deployment URL
- If the build job fails, the deploy job never runs — the live site is unaffected

### Adding Secrets

If you add external services requiring API keys:
1. Go to **Settings → Secrets and variables → Actions**
2. Click **New repository secret**
3. Use in workflows as: `${{ secrets.MY_SECRET_NAME }}`

---

## 10. Updating the Theme

Chirpy releases updates regularly. To upgrade:

### Check the Current Version

```bash
bundle show jekyll-theme-chirpy
# or
cat Gemfile.lock | grep jekyll-theme-chirpy
```

### Upgrade Process

1. Update `Gemfile`:
   ```ruby
   gem "jekyll-theme-chirpy", "~> 7.5"   # bump version
   ```

2. Run `bundle update jekyll-theme-chirpy`

3. Check the [Chirpy CHANGELOG](https://github.com/cotes2020/jekyll-theme-chirpy/blob/master/CHANGELOG.md) for breaking changes

4. Test locally: `bundle exec jekyll serve`

5. If the theme changed any layout or include files that you've overridden, you may need to update your local copies in `_includes/` or `_layouts/`

6. Push to `main` — the lockfile is in `.gitignore`, so the CI will also resolve the new version

### Reverting a Bad Upgrade

```bash
git revert HEAD   # if you've already pushed
# or
git checkout HEAD~1 -- Gemfile   # revert just Gemfile
bundle install
```

---

## 11. Security & Access Control

### Repository Permissions

| Role | Access | Who |
|------|--------|-----|
| Owner | Full access | akashtalole |
| Maintainer | Can merge PRs, manage issues | Trusted contributors |
| Triage | Can label and close issues | Active community members |

To add a collaborator: **Settings → Collaborators and teams → Add people**

### Branch Protection (Recommended)

Protect the `main` branch to prevent direct pushes:

1. **Settings → Branches → Add branch protection rule**
2. Branch name pattern: `main`
3. Enable:
   - **Require a pull request before merging**
   - **Require status checks to pass** → select "build" (from the workflow)
   - **Do not allow bypassing the above settings**

This ensures every change to `main` has passed the Jekyll build and htmlproofer before merging.

### Keeping Dependencies Updated

Use [Dependabot](https://docs.github.com/en/code-security/dependabot):

Create `.github/dependabot.yml`:

```yaml
version: 2
updates:
  - package-ecosystem: "bundler"
    directory: "/"
    schedule:
      interval: "weekly"
    labels:
      - "dependencies"
  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "monthly"
    labels:
      - "dependencies"
```

Dependabot will open PRs automatically when gem or action versions are updated.

### Content Security

The site is static HTML — there's no server-side attack surface. However:

- **Giscus / GitHub Discussions:** Monitor for spam. GitHub's built-in moderation tools (block users, lock threads) are sufficient.
- **Issue spam:** Enable **Settings → Moderation → Interaction limits** if spam increases. You can limit interactions to users with prior contributions for a set period.

---

## 12. Troubleshooting Deployments

### Build Fails: htmlproofer

```
linking to internal resource /posts/some-slug/ which does not exist
```

**Fix:** The link in one of your files points to a post slug that doesn't exist or has a typo. Check `_tabs/missions.md` and mission navigation links.

### Build Fails: Jekyll Liquid Error

```
Liquid Exception: undefined method for nil class
```

**Fix:** Usually caused by malformed front matter. Check the recently changed post for YAML syntax errors (missing quotes, wrong indentation).

### Build Fails: Ruby/Bundler

```
Bundler could not find compatible versions for gem "jekyll-theme-chirpy"
```

**Fix:** The `Gemfile.lock` is committed (it shouldn't be — it's in `.gitignore`). Delete it and re-run `bundle install`.

### Site Deploys But Shows 404

**Cause:** `baseurl` mismatch. The site is deployed at `/AI-Workshop/` but `baseurl` in `_config.yml` is wrong.

**Fix:** Ensure `_config.yml` has:
```yaml
url: "https://akashtalole.github.io"
baseurl: "/AI-Workshop"
```

### Site Deploys But CSS Is Missing

**Cause:** Asset paths broken due to `baseurl`.

**Fix:** Ensure you're using `{{ '/assets/...' | relative_url }}` in any custom HTML, not hardcoded `/assets/...` paths.

### Giscus Shows "Discussion Not Found"

**Cause:** `repo_id` or `category_id` in `_config.yml` is incorrect or blank.

**Fix:** Re-run the [giscus.app](https://giscus.app) configuration with your repo, copy the correct IDs, update `_config.yml`, and push.

### Last Modified Date Not Updating

**Cause:** The GitHub Actions workflow is using a shallow clone.

**Fix:** Verify `fetch-depth: 0` is set in the checkout step of `.github/workflows/pages-deploy.yml`. This is already set in the current workflow.

---

## 13. Checklist: Launch Readiness

Complete this checklist before announcing the platform publicly:

### GitHub Setup
- [ ] Repository is public
- [ ] GitHub Pages enabled, source set to **GitHub Actions**
- [ ] First deployment succeeded — site is live at `https://akashtalole.github.io/AI-Workshop/`
- [ ] GitHub Discussions enabled
- [ ] Giscus app installed on the repo
- [ ] `_config.yml` updated with `repo_id` and `category_id` from giscus.app
- [ ] Branch protection on `main` configured
- [ ] `dependabot.yml` added for automated dependency updates

### Content
- [ ] All mission navigation links (prev/next) verified
- [ ] RECRUIT-00 orientation post is pinned (`pin: true`)
- [ ] `_tabs/missions.md` matches all existing posts exactly
- [ ] htmlproofer passes locally with no errors

### Site Quality
- [ ] Site loads at production URL
- [ ] All sidebar tabs render correctly (Categories, Tags, Archives, Missions, About, User Guide, Admin Guide)
- [ ] Search returns results
- [ ] Giscus comment box appears on mission pages
- [ ] Dark/light mode toggle works
- [ ] Site is mobile-responsive (check on phone)

### Analytics & Monitoring
- [ ] Google Analytics or GoatCounter configured
- [ ] Confirmation that page views are being tracked after first visit

### Community
- [ ] GitHub Issues tab enabled
- [ ] All three issue templates visible when opening a new issue
- [ ] GitHub Discussions categories set up (Q&A, Show and Tell, Ideas)
- [ ] GitHub Project "Mission Roadmap" board created with planned missions
