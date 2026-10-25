# Hosting this portfolio on GitHub Pages

A complete walkthrough for publishing this site to
**`github.com/Amanda-dev-bit/Amanda-dev-bit.github.io`**.

Everything the site needs is already in this folder. You do not have to edit any
build configuration. Follow the steps in order.

**Your live URL will be:** <https://amanda-dev-bit.github.io/>

---

## Contents

1. [What kind of site this is, and why it matters](#1-what-kind-of-site-this-is-and-why-it-matters)
2. [Before you start](#2-before-you-start)
3. [Step 1: Fix who the commits are from](#step-1-fix-who-the-commits-are-from)
4. [Step 2: Create the repository on GitHub](#step-2-create-the-repository-on-github)
5. [Files that stay on your computer](#files-that-stay-on-your-computer)
6. [Step 3: Push this folder](#step-3-push-this-folder)
7. [Step 4: Turn Pages on (the step everyone misses)](#step-4-turn-pages-on-the-step-everyone-misses)
8. [Step 5: Watch the first deploy](#step-5-watch-the-first-deploy)
9. [Step 6: Check the live site](#step-6-check-the-live-site)
10. [Step 7: Finish the setup](#step-7-finish-the-setup)
11. [Making changes later](#making-changes-later)
12. [How the deploy actually works](#how-the-deploy-actually-works)
13. [Troubleshooting](#troubleshooting)
14. [Limits and rules worth knowing](#limits-and-rules-worth-knowing)
15. [Using your own domain later](#using-your-own-domain-later)
16. [Final checklist](#final-checklist)

---

## 1. What kind of site this is, and why it matters

GitHub Pages has two kinds of site, and the repository name is what decides which
one you get.

|           | User site                           | Project site                                  |
| --------- | ----------------------------------- | --------------------------------------------- |
| Repo name | `Amanda-dev-bit.github.io`          | anything else                                 |
| URL       | `https://amanda-dev-bit.github.io/` | `https://amanda-dev-bit.github.io/repo-name/` |
| How many  | **One per account**                 | Unlimited                                     |

Because your repo is named `Amanda-dev-bit.github.io`, you get a **user site**
served from the root of the domain. This is the better one for a portfolio: the URL
is short and it is the address people expect.

**If you misspell the repository name even slightly**, GitHub quietly makes a project
site instead and serves it at `https://amanda-dev-bit.github.io/whatever-you-named-it/`.
The name has to be exact.

> **A note on capital letters.** GitHub's docs say to lowercase the name if your
> username has capitals, so `amanda-dev-bit.github.io` is the form they recommend.
> In practice GitHub treats it case-insensitively and `Amanda-dev-bit.github.io`
> works. Either way, **the published URL is always lowercase**:
> `https://amanda-dev-bit.github.io/`. Do not be surprised when the capital A
> disappears.

---

## 2. Before you start

You need:

- **Git installed.** Check with `git --version`. If that fails, install from
  <https://git-scm.com/downloads>.
- **A GitHub account** signed in as `Amanda-dev-bit`.
- **Node 20.19+ or 22.12+** if you want to run the site locally. Check with
  `node -v`. Not needed to deploy, since GitHub builds it for you.

Open a terminal in this project folder. Every command below runs from here.

```bash
cd into where you have the Amanda-Portfolio folder that you unzipped.
```

You should see `package.json` and `index.html` when you run `ls` (or `dir`).

---

## Step 1: Fix who the commits are from

**Do this before pushing.** It is cheap now and painful later.

Right now the two existing commits are authored by:

```
GeraldXtra <gerald.dev274@gmail.com>
```

That is not you. On a public repository the author name and email appear on the
commits page, on every blame line, and through the API. Anyone reading the repo
would see your portfolio credited to a different person, and that email becomes
permanently harvestable. Changing it after pushing needs a force-push and the old
version stays reachable on GitHub anyway.

**Set your identity:**

```bash
git config user.name "Amanda-dev-bit"
git config user.email "YOUR-EMAIL@example.com"
```

For the email you have two options:

- Your real address, or
- A GitHub no-reply address, which keeps your real email private. Find it at
  <https://github.com/settings/emails>. It looks like
  `12345678+Amanda-dev-bit@users.noreply.github.com`. **This is the better choice.**

**Then rewrite the existing commits to use it:**

```bash
git rebase --root --exec "git commit --amend --reset-author --no-edit"
```

**Check it worked:**

```bash
git log --format="%an <%ae>"
```

Both lines should now show your name and address.

> The commit messages also contain a `Claude-Session:` line from the tooling used to
> build the site. It is not a password or a key, but it is not useful to anyone
> either. If you would rather it were not public, remove those lines with
> `git rebase -i --root` and editing each message. Entirely optional.

---

## Step 2: Create the repository on GitHub

1. Go to <https://github.com/new>.
2. **Repository name:** `Amanda-dev-bit.github.io`

   Type it exactly. GitHub will show a note saying it recognises this as a user
   site, which is how you know you got it right.

3. **Description** (optional): `My portfolio. Web Developer and UI/UX Designer, Lagos.`
4. **Visibility: Public.**

   This is required. On a free GitHub account, Pages only works from a public
   repository. (Even on paid plans, a user site's _published page_ is always
   readable by anyone, and there is no private option for this kind of site.)

5. **Do not tick anything** under "Initialize this repository with":

   - no README
   - no .gitignore
   - no license

   This folder already has all of those. Adding them on GitHub creates a commit that
   your first push will conflict with.

6. Click **Create repository**.

You will land on an empty repo page with setup instructions. Ignore them and use the
commands in the next step instead.

---

## Files that stay on your computer

Three files in this folder are **deliberately kept out of the repository**. They are
yours to read locally, and nobody visiting your GitHub page should see them:

| File | Why it stays private |
| --- | --- |
| `HOSTING.md` | This guide. It quotes an email address and describes your setup. |
| `DOCS.md` | Your working notes on editing the site. |
| `resume-session.txt` | A local scratch file, nothing to do with the site. |

They are listed in `.gitignore`, so git ignores them completely. You do not have to
remember anything or avoid any command. `git add .` will simply skip them.

**To confirm they are being skipped**, run this before you push:

```bash
git status --porcelain
```

None of the three should appear. You can also ask git directly:

```bash
git check-ignore -v HOSTING.md DOCS.md resume-session.txt
```

That should print a line for each one showing which `.gitignore` rule catches it. If
a file prints nothing, it is **not** being ignored and would get committed.

To see the full list of what git is actually tracking:

```bash
git ls-files
```

`HOSTING.md`, `DOCS.md` and `resume-session.txt` must not be in that list.

> **Keep your own copy.** Because these files are not in the repository, they do not
> travel with it. If you ever clone this project onto another computer, they will not
> be there. Keep a copy somewhere (email them to yourself, or drop them in your
> cloud drive) if you want them on hand later.

**If you ever want one of them public**, delete its line from `.gitignore`, then:

```bash
git add HOSTING.md
git commit -m "Add the hosting notes"
git push
```

---

## Step 3: Push this folder

```bash
git remote add origin https://github.com/Amanda-dev-bit/Amanda-dev-bit.github.io.git
git branch -M main
git push -u origin main
```

**What each line does:**

- `git remote add origin ...` tells your local repo where GitHub is.
- `git branch -M main` makes sure the branch is called `main`, which is what the
  deploy workflow watches.
- `git push -u origin main` uploads everything and remembers the connection, so
  future pushes are just `git push`.

**If it asks for a password:** GitHub stopped accepting account passwords over HTTPS.
Use a Personal Access Token instead:

1. Go to <https://github.com/settings/tokens> → **Generate new token (classic)**.
2. Tick the **`repo`** scope.
3. Copy the token and paste it as the password. The username is `Amanda-dev-bit`.

Windows will usually save it in Credential Manager so you only do this once.

**If it says the remote already exists**, you have run this before. Update it instead:

```bash
git remote set-url origin https://github.com/Amanda-dev-bit/Amanda-dev-bit.github.io.git
```

Refresh the GitHub page. Your files should be there.

---

## Step 4: Turn Pages on (the step everyone misses)

**Pushing does not publish the site.** You have to point Pages at the workflow.

1. In your repository, click **Settings** (top bar, on the right).
2. In the left sidebar, click **Pages**.
3. Under **Build and deployment**, find **Source**.
4. Change it from _Deploy from a branch_ to **GitHub Actions**.

There is no Save button. The change applies immediately.

> **Why this matters so much.** If Source stays on _Deploy from a branch_, the
> workflow still runs, still goes green, and still says it deployed, but GitHub
> publishes nothing, and you get a 404 with no obvious reason why. This single
> setting is the most common cause of "my Pages site doesn't work".

You do **not** need a `gh-pages` branch. That is an older approach this project does
not use.

---

## Step 5: Watch the first deploy

1. Click the **Actions** tab.
2. You should see a run called **Deploy to GitHub Pages**.
3. Click it. There are two jobs, **Build** then **Deploy**, and they run in order.

A normal run takes about one to two minutes.

**If nothing is listed**, the workflow did not trigger. Either the push did not land
on `main`, or Actions is disabled. Check **Settings → Actions → General** and make
sure _Allow all actions_ is selected.

When both jobs have green ticks, the Deploy job shows the published URL. GitHub says
it can take **up to 10 minutes** for a brand new site to become reachable, so if the
run is green but the page 404s, wait a few minutes before assuming something broke.

---

## Step 6: Check the live site

Open <https://amanda-dev-bit.github.io/>

Go through this list:

- [ ] The hero loads with your photo, not a blank space
- [ ] Scrolling reveals About, Craft, Work, Journey and Contact
- [ ] Your About photo appears
- [ ] Project cards show their preview images
- [ ] **The footer is visible at the very bottom** (your name in large faint type,
      the copyright line, and the "Back to top" button)
- [ ] The dark/light toggle in the header works
- [ ] "Download CV" opens your PDF
- [ ] The contact form appears with all its fields

**Then hard-refresh** with `Ctrl + F5`. Browsers cache aggressively and a stale copy
is a common source of false alarms.

If something looks wrong, open the browser console with `F12` and check the Console
and Network tabs for red errors before changing anything.

---

## Step 7: Finish the setup

**a. Add the URL to the repo header.** On the repo's main page, click the gear icon
next to **About** on the right, and put `https://amanda-dev-bit.github.io/` in the
**Website** field. Add topics too: `portfolio`, `react`, `vite`, `github-pages`.
These show in the sidebar and in GitHub search.

**b. Connect the contact form.** The form currently falls back to opening the
visitor's email app, because no Formspree ID is set. To make it post properly:

1. Sign up at <https://formspree.io> and create a form. Point it at
   `chukwujekwuamanda09@gmail.com`.
2. Formspree gives you an endpoint like `https://formspree.io/f/abcdwxyz`.
3. Open `src/data/site.js` and paste **only the id**, the part after `/f/`:

   ```js
   export const formspreeId = "abcdwxyz";
   ```

4. Commit and push. The deploy runs automatically.
5. Send yourself a test message. Formspree makes you confirm the very first
   submission by email before it starts forwarding.

**c. Check the social preview.** Paste your URL into
<https://www.opengraph.xyz/> to see how the link looks when shared. The card image
comes from `public/og.png`.

---

## Making changes later

The whole loop is:

```bash
# edit your files, then:
git add .
git commit -m "Describe what changed"
git push
```

Pushing to `main` redeploys automatically. Watch it in the **Actions** tab.

**Most edits do not need you to touch a component.** Everything is in `src/data/`:

| To change                                     | Edit                     |
| --------------------------------------------- | ------------------------ |
| Name, role, bio, email, phone, socials, stats | `src/data/site.js`       |
| Projects, descriptions, images, links         | `src/data/projects.js`   |
| Skills and proficiency levels                 | `src/data/skills.js`     |
| Work history, education, services             | `src/data/experience.js` |

**To preview before pushing:**

```bash
npm install     # first time only
npm run dev     # opens a local server, updates as you type
```

**To redeploy without changing anything** (rarely needed): Actions tab → **Deploy to
GitHub Pages** → **Run workflow**.

---

## How the deploy actually works

Two workflow files drive everything.

### `.github/workflows/deploy.yml`

Runs on every push to `main`, and on demand.

1. **Build job:** checks out the code, installs Node 22, runs `npm ci` (a clean
   install from `package-lock.json`), asks GitHub what the site's real URL is, runs
   `npm run build`, and uploads `dist/` as an artifact.
2. **Deploy job:** takes that artifact and publishes it.

The built site is passed between jobs as an artifact. **`dist/` is never committed**,
which is why `.gitignore` excludes it. There is no `gh-pages` branch.

### `.github/workflows/ci.yml`

Runs on pull requests and on branches other than `main`. It builds the site and
checks the output actually contains the files the page needs. Its job is to catch a
broken build on a branch, rather than letting you find out through a failed deploy on
`main`.

### Why your domain is not written into any file

`vite.config.js` sets `base: './'`, so every asset path is relative. The same build
works at a domain root, in a subfolder, or opened straight off disk.

Social crawlers are the exception: Facebook, LinkedIn, X and friends ignore relative
image URLs, so link previews stay blank without absolute ones. The workflow asks
GitHub for the real published address and passes it to the build as `VITE_SITE_URL`;
a small plugin in `vite.config.js` turns that into the `canonical`, `og:image` and
`twitter:image` tags. If you later add a custom domain, this follows it
automatically with no edits.

### `.github/dependabot.yml`

Opens a grouped pull request once a month if the actions or npm packages need
updating. Pinned action versions go stale quietly, and without this the first sign is
usually a deploy that suddenly fails months from now.

---

## Troubleshooting

### The site shows 404

**In order of likelihood:**

1. **Source is not set to GitHub Actions.** Settings → Pages → Source. This is the
   cause most of the time. See [Step 4](#step-4-turn-pages-on-the-step-everyone-misses).
2. **It has not finished publishing.** A first deploy can take up to 10 minutes.
3. **The repo name is wrong.** It must be exactly `Amanda-dev-bit.github.io`. If it
   is anything else, your site is at
   `https://amanda-dev-bit.github.io/that-other-name/` instead.
4. **The repo is private.** Free accounts need it public. Settings → General →
   scroll to the bottom → Change visibility.
5. **The workflow never ran.** Actions tab. If it is empty, check Settings → Actions
   → General → _Allow all actions_.

### The workflow is green but the site never updates

Almost always browser cache. Hard-refresh with `Ctrl + F5`, or open the URL in a
private window. If it is still stale after ten minutes, re-run the workflow from the
Actions tab.

### The Deploy job hangs or fails with a branch error

Open **Settings → Environments → github-pages** and look at **Deployment branches**.
GitHub creates this environment automatically and may restrict it to one branch. If
you are deploying from a branch that is not listed, the deploy job is blocked. Add
your branch, or deploy from `main`.

### The build fails on `npm ci`

`npm ci` needs `package-lock.json` to match `package.json` exactly. If you installed
or removed a package, commit the updated lock file:

```bash
npm install
git add package-lock.json package.json
git commit -m "Update dependencies"
git push
```

### The build fails with a Node version error

The workflows use Node 22 and the project needs 20.19+ or 22.12+. If you changed
`node-version` in a workflow, put it back to `'22'`. Keep `.nvmrc` in step with it.

### Images or the CV give 404 on the live site but work locally

Check the file is actually committed:

```bash
git ls-files public/
```

If a file is missing from that list it was never pushed. Anything matching a rule in
`.gitignore` is skipped silently.

### The contact form does nothing

Expected until you set `formspreeId` in `src/data/site.js`. Until then it opens the
visitor's own email app with the message written out. See
[Step 7b](#step-7-finish-the-setup).

### Push is rejected with "updates were rejected"

The remote has commits you do not have locally, usually because the repo was created
with a README. Since your local history is the real one:

```bash
git push --force-with-lease origin main
```

Only do this on the very first push, while the remote has nothing you care about.

### "Deprecated version of actions" warnings

The actions are pinned to current versions as of writing, and Dependabot will offer
updates monthly. GitHub removes the old Node 20 runtime on **23 September 2026**, so
if you are reading this later and seeing warnings, merge the Dependabot pull request.

---

## Limits and rules worth knowing

| Limit                  | Value                                                                  |
| ---------------------- | ---------------------------------------------------------------------- |
| Published site size    | 1 GB (this site is about 1 MB)                                         |
| Bandwidth              | 100 GB per month, soft limit                                           |
| Builds per hour        | 10, soft, and **not applied** when deploying via Actions, as this does |
| User sites per account | 1                                                                      |

**Your published site is public, always.** Even if you later make the repository
private on a paid plan, the page at the URL stays readable by anyone. There is no
private option for a user site. Do not put anything in this repo you would not want
read: the CV, photos and contact details here are all deliberately published.

Pages is for sites like this one. GitHub's terms do not allow using it to run a
business, a shop, or anything handling payments or passwords.

---

## Using your own domain later

If you buy something like `amandachimdiuso.com`:

1. Create a file called `CNAME` in the `public/` folder containing just the bare
   domain, no `https://` and no slash:

   ```
   amandachimdiuso.com
   ```

2. Commit and push.
3. At your domain registrar, add these DNS records:

   | Type  | Name  | Value                       |
   | ----- | ----- | --------------------------- |
   | A     | `@`   | `185.199.108.153`           |
   | A     | `@`   | `185.199.109.153`           |
   | A     | `@`   | `185.199.110.153`           |
   | A     | `@`   | `185.199.111.153`           |
   | CNAME | `www` | `amanda-dev-bit.github.io.` |

4. Settings → Pages → **Custom domain**, enter the domain, click Save.
5. Wait for the DNS check to pass, then tick **Enforce HTTPS**. DNS can take up to
   24 hours, and the HTTPS option only appears once the certificate is issued.

The social tags follow the new domain on the next deploy with no code changes,
because the workflow asks GitHub for the current URL each time.

---

## Final checklist

Before you call it done:

- [ ] `git log --format="%an <%ae>"` shows **your** name and email, not GeraldXtra
- [ ] `git ls-files` does not list HOSTING.md, DOCS.md or resume-session.txt
- [ ] Repo is named exactly `Amanda-dev-bit.github.io`
- [ ] Repo is **public**
- [ ] Settings → Pages → Source is **GitHub Actions**
- [ ] The Deploy workflow finished green
- [ ] <https://amanda-dev-bit.github.io/> loads
- [ ] The footer is visible at the bottom of the page
- [ ] Both photos load, and the CV downloads
- [ ] The About → Website field points at the live URL
- [ ] `formspreeId` is set in `src/data/site.js` and a test message arrived
- [ ] The link preview looks right on <https://www.opengraph.xyz/>

---

_Questions about the code itself are answered in [README.md](README.md)._
