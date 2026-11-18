# Getting Started (for teammates)

Welcome! This repo is our group project for **ITE 300**. We all edit the same R Markdown report (`Final Project Markdown.Rmd`), so this guide explains how to set up GitHub once and then how to work without stepping on each other.

Repo: <https://github.com/jsmarkertjs/ITEC-300-Final-Project>

> **The rule that keeps us out of trouble:** *always pull the latest changes before you start working, and work on your **own branch** — never directly on `main`.*

------------------------------------------------------------------------

## 1. One-time setup

### 1a. Install the tools

You need three things:

1.  **R** — <https://cran.r-project.org>
2.  **RStudio Desktop** — <https://posit.co/download/rstudio-desktop>
3.  **Git** — <https://git-scm.com/downloads> (accept the default options)

### 1b. Make a GitHub account

Sign up at <https://github.com> if you don't have one, then **send your username** to the project owner so they can add you as a collaborator.

### 1c. Accept the invite

The owner will add you under **Settings → Collaborators**. You'll get an email invite — click **Accept invitation**. (If you don't see the email, check the repo page; a banner will ask you to accept.)

### 1d. Tell Git who you are

Open RStudio and run this once in the **Console** (use your own name/email):

``` r
install.packages("usethis")   # only if you don't already have it
usethis::use_git_config(user.name = "Your Name", user.email = "you@example.com")
```

------------------------------------------------------------------------

## 2. Download the project (clone it)

You only do this once.

1.  Open RStudio.

2.  **File → New Project → Version Control → Git**.

3.  Paste this URL:

    ```         
    https://github.com/jsmarkertjs/ITEC-300-Final-Project
    ```

4.  Pick a folder to save it in (e.g. under Documents) and click **Create Project**.

RStudio creates the folder, downloads the project, and opens it. When Git prompts you to sign in, use your GitHub account (a browser window usually pops up).

------------------------------------------------------------------------

## 3. Get the flight data

The dataset `flights.rds` is **about 222 MB**, which is bigger than GitHub's 100 MB file limit, so it is deliberately **not stored in the repo**. Everyone needs their own copy.

1.  Get `flights.rds` from the **course materials** (wherever the instructor posted the ITE 300 dataset).
2.  Copy it into the **project folder** — the same folder that contains `Final Project.Rproj`, at the top level next to the `.Rmd` file.

```         
ITEC-300-Final-Project/
├── Final Project Markdown.Rmd
├── Final Project.Rproj
├── flights.rds            <-- put your copy here
└── ...
```

**Do not** try to commit or upload `flights.rds`. `.gitignore` is already set up to skip it, so Git will simply ignore it. If you ever see it listed as a change, leave it alone.

------------------------------------------------------------------------

## 4. Your everyday workflow

Each person works on **their own branch** and merges back into `main` through a **Pull Request (PR)**. Think of `main` as the "official" copy that should always work.

### Step A — Start: get the newest `main` and make/switch to your branch

Open the project in RStudio, then in the **Terminal** tab (or R console with a `system()` wrapper) run:

``` bash
git switch main
git pull
git switch <your-branch>      # e.g. git switch jane
```

- The **first time**, your branch doesn't exist yet, so create it:

  ``` bash
  git switch -c <your-branch>   # e.g. git switch -c jane
  ```

- After that, `git switch <your-branch>` is enough.

> Tip: name the branch after yourself or your task (`jane`, `jane-analysis`).

### Step B — Work

Edit the `.Rmd` (or other files), save, and **commit often**. For example every time you finish a small piece:

``` bash
git add -A
git commit -m "Add data cleaning section"
```

You can also do this from RStudio's **Git pane**: tick the boxes next to the files, click **Commit**, type a message, click **Commit** again.

### Step C — Finish: push your branch and open a PR

``` bash
git push -u origin <your-branch>   # first push of a branch needs -u
```

Then on the GitHub repo page, click **Compare & pull request** (or **Pull requests → New pull request**): set **base: main** and **compare: your-branch**, add a short description, and click **Create pull request**.

Ask the owner (or a teammate) to review and **Merge**. After it's merged, the changes are in `main`.

### Step D — Reset for next time

``` bash
git switch main
git pull
git switch <your-branch>
git merge main          # bring the newest main into your branch
```

Now you're ready to keep working.

------------------------------------------------------------------------

## 5. Repo rules: `main` is protected

The `main` branch is **protected** so nobody can accidentally overwrite each other's work. Here's what that means for you:

- **No direct pushes to `main`.** If you run `git push origin main`, GitHub will reject it. That's expected — it's not a sign you did something wrong.
- **Everything goes through a pull request.** All changes (yours and everyone else's) reach `main` only after a PR is created and merged.
- **Force-pushes are blocked.** History on `main` can't be rewritten, so nothing already merged can be wiped out.
- **`main` can't be deleted.**
- The project owner is an **admin** and can bypass these rules in an emergency, but teammates cannot.

In short: **do your work on your own branch, push the branch, and open a PR.** That is the only path into `main`.

------------------------------------------------------------------------

## 6. Avoiding merge conflicts

A **merge conflict** happens when two people changed the *same lines* and Git can't decide which to keep. They are fixable, but the goal is to avoid them.

**Do this:** - 🔁 **Pull `main` every time before you start** (Step A and Step D above). - ⏱️ **Keep your branch short-lived.** Don't sit on changes for weeks — push, open a PR, and merge as soon as your part is done. - 💬 **Talk in the group chat.** Say what section you're editing so two people aren't rewriting the same paragraph at once. - ✂️ **Make small commits** so it's clear what changed. - 🚧 **Never edit directly on `main`.** Always work on your branch.

**Never do this:** - Don't edit files on someone else's branch. - Don't force-push (`git push --force`). - Don't delete the `.gitignore`.

### If you do hit a conflict

Don't panic and don't start deleting things.

1.  RStudio will show the file as conflicted in the **Git pane**; opening it marks the two versions with `<<<<<<<`, `=======`, `>>>>>>>`.

2.  Fix the file so it reads the way you want, delete those marker lines, and save.

3.  Then:

    ``` bash
    git add -A
    git commit -m "Resolve merge conflict"
    ```

4.  If anything looks wrong, **stop and ask in the group chat** — we can always undo it.

------------------------------------------------------------------------

## 7. Command cheat sheet

| What you want                 | Command                                     |
|-------------------------------|---------------------------------------------|
| See what changed              | `git status`                                |
| Get newest `main`             | `git switch main` then `git pull`           |
| Switch to your branch         | `git switch <your-branch>`                  |
| Create your branch (once)     | `git switch -c <your-branch>`               |
| Save your work                | `git add -A` then `git commit -m "message"` |
| Upload your work              | `git push -u origin <your-branch>`          |
| Bring `main` into your branch | `git merge main`                            |

------------------------------------------------------------------------

## 8. Quick checklist

- [ ] R, RStudio, and Git installed
- [ ] GitHub account created, invite accepted
- [ ] Project cloned in RStudio
- [ ] `flights.rds` copied from the course into the project folder (not committed)
- [ ] Created my own branch
- [ ] Pulled `main` before starting work
- [ ] Committed small, pushed my branch, opened a PR
- [ ] Pulled `main` again after my PR merged
