# Team Setup Guide For Postman Native Git

## Purpose

This repository is the shared local and GitHub-backed storage for our Postman workspaces.

We use:

- one GitHub repository for the whole team: [eh_collections](https://github.com/ezem001/eh_collections)
- one local Git repository cloned from that GitHub repository
- three separate Postman workspace roots inside the repository:
  - `workspaces/Demo`
  - `workspaces/Preprod`
  - `workspaces/Stage`

This layout is intentional. It prevents conflicts when different Postman workspaces contain collections with the same names. For example, `Demo` and `Preprod` can both have a collection named `General MIS API`, but because each workspace is stored in a different folder, the files do not overwrite each other.

## Why We Use This Structure

We do not use one flat `postman/collections` directory for everything, because:

- different workspaces often contain collections with identical names
- Postman Native Git becomes confusing when unrelated workspaces are mixed into the same local folder
- it becomes hard to understand what should be committed and pushed
- teammates can accidentally commit changes from the wrong workspace

Instead, we use one shared repository with separate folders per Postman workspace.

The current structure is:

```text
eh_collections/
  workspaces/
    Demo/
      .postman/
      postman/
    Preprod/
      .postman/
      postman/
    Stage/
      .postman/
      postman/
```

Each folder under `workspaces/` is connected to one Postman workspace only.

## What Each Person Needs Before Starting

Each teammate must have:

1. Access to the GitHub repository [eh_collections](https://github.com/ezem001/eh_collections) with write permissions.
2. Access to the Postman team and to these Postman workspaces:
   - `Demo`
   - `Preprod`
   - `Stage`
3. Postman Desktop App installed.
4. Git installed locally.
5. Basic ability to run terminal commands.

Optional but recommended:

- GitHub Desktop, if a teammate prefers GUI for Git
- a GitHub authentication method already configured locally

## Important Rules Before Anyone Starts

Please follow these rules exactly:

1. Never run `git init` inside `workspaces/Demo`, `workspaces/Preprod`, or `workspaces/Stage`.
2. Never create a separate repository inside any workspace folder.
3. Always work from the single repository root.
4. Always connect Postman to the existing cloned repository, not to a random new folder.
5. Always keep each Postman workspace mapped to its own folder:
   - `Demo` -> `workspaces/Demo`
   - `Preprod` -> `workspaces/Preprod`
   - `Stage` -> `workspaces/Stage`
6. Do not store secrets in committed files.
7. Each teammate must create and always use their own personal branch.
8. The personal branch name must match the teammate's name in the agreed format.
9. Do not work directly in `main`.

## Personal Branch Naming Convention

Each teammate first creates one permanent personal branch and keeps using it for all future work.

Examples:

- `Roman_Katamai`
- `Kate_Hubko`
- `Lilia_Besednikova`
- `Katya_Aloshyna`
- `Anastasiia_Afrosimova`

Important:

- this is not a temporary feature branch per task
- this is a long-lived personal branch
- the teammate keeps reusing the same branch every day
- pull requests go from the personal branch into `main`

## Step 1: Clone The Repository

Choose a local folder for the project, then run:

```bash
git clone https://github.com/ezem001/eh_collections.git
cd eh_collections
git checkout main
git pull origin main
```

After that, verify that the expected structure exists:

```bash
find workspaces -maxdepth 2 | sort
```

You should see folders for:

- `workspaces/Demo`
- `workspaces/Preprod`
- `workspaces/Stage`

## Step 2: Verify Git Access

Before connecting Postman, check that Git works:

```bash
git remote -v
git status
```

Expected result:

- `origin` should point to `https://github.com/ezem001/eh_collections.git`
- `git status` should not show repository errors

If push access is not configured yet, fix that before continuing. Otherwise Postman changes may be created locally but later fail during team synchronization.

## Step 3: Create Your Personal Branch

Before opening Postman for real work, each teammate must create their own personal branch.

Example for Roman:

```bash
cd /path/to/eh_collections
git checkout main
git pull origin main
git checkout -b Roman_Katamai
git push -u origin Roman_Katamai
```

Example for Kate:

```bash
cd /path/to/eh_collections
git checkout main
git pull origin main
git checkout -b Kate_Hubko
git push -u origin Kate_Hubko
```

Example for Lilia:

```bash
cd /path/to/eh_collections
git checkout main
git pull origin main
git checkout -b Lilia_Besednikova
git push -u origin Lilia_Besednikova
```

Example for Katya:

```bash
cd /path/to/eh_collections
git checkout main
git pull origin main
git checkout -b Katya_Aloshyna
git push -u origin Katya_Aloshyna
```

Example for Anastasiia:

```bash
cd /path/to/eh_collections
git checkout main
git pull origin main
git checkout -b Anastasiia_Afrosimova
git push -u origin Anastasiia_Afrosimova
```

Do this only once per teammate.

After the personal branch is created, the teammate keeps using that same branch.

## Step 4: Open The Correct Postman Workspace

Open Postman Desktop App and choose the correct workspace:

- `Demo` if you want to work with `workspaces/Demo`
- `Preprod` if you want to work with `workspaces/Preprod`
- `Stage` if you want to work with `workspaces/Stage`

Do not connect the wrong local folder to the wrong Postman workspace.

Correct mapping:

- Postman workspace `Demo` -> local folder `workspaces/Demo`
- Postman workspace `Preprod` -> local folder `workspaces/Preprod`
- Postman workspace `Stage` -> local folder `workspaces/Stage`

## Step 5: Connect Postman To The Existing Local Repository

Inside Postman:

1. Open the target workspace, for example `Demo`.
2. In the left sidebar, open `Files`.
3. If Postman asks to set up the local codebase, choose the option that means opening or connecting an existing local copy.
4. Select the cloned repository root on your machine.

Example:

```text
/Users/<your-user>/.../eh_collections
```

5. When Postman asks for the path inside the repository, choose:

- `/workspaces/Demo` for the `Demo` workspace
- `/workspaces/Preprod` for the `Preprod` workspace
- `/workspaces/Stage` for the `Stage` workspace

6. Confirm the connection.

Important:

- The repository root is the cloned `eh_collections` folder.
- The workspace path is one of the subfolders inside `workspaces/`.
- Do not point Postman to a separate nested Git repository.
- Do not create a second clone just for one workspace.

## Step 6: Confirm That Postman Is Writing To The Correct Folder

After connecting, Postman should create or update files in the matching workspace folder.

Examples:

- `Demo` should write under `workspaces/Demo/...`
- `Preprod` should write under `workspaces/Preprod/...`
- `Stage` should write under `workspaces/Stage/...`

You can verify from terminal:

```bash
git status
```

If you changed something in `Demo`, you should only see file changes inside `workspaces/Demo`.

If you suddenly see `Demo` changes under `workspaces/Preprod` or the other way around, stop and disconnect/reconnect the folder mapping in Postman before continuing.

## Step 7: Understand Local View And Cloud View

Postman Native Git typically uses:

- `Local View` for file-backed local work
- `Cloud View` for the synchronized cloud state

Practical rule:

- work on files in `Local View`
- use `Pull from Postman Cloud` when you need to bring cloud changes into local files
- use `Push to Postman Cloud` when you intentionally want local file changes reflected back in Postman Cloud

Do not assume Git push automatically updates Postman Cloud.
Do not assume Postman Cloud automatically commits to Git.
These are related workflows, but they are not the same action.

## Step 8: Daily Start-Of-Day Workflow

Every day, before making changes, do this:

```bash
cd /path/to/eh_collections
git checkout main
git pull origin main
git checkout Roman_Katamai
git merge main
```

Replace `Roman_Katamai` with the teammate's own branch name.

Examples:

- Roman runs `git checkout Roman_Katamai`
- Kate runs `git checkout Kate_Hubko`
- Lilia runs `git checkout Lilia_Besednikova`
- Katya runs `git checkout Katya_Aloshyna`
- Anastasiia runs `git checkout Anastasiia_Afrosimova`

Why this matters:

- everyone starts from the latest team state
- each person stays in their own branch
- changes stay isolated by person
- pull requests into `main` stay predictable
- conflicts are easier to resolve

Do not do regular work directly in `main`.

## Step 9: Make Changes In Only The Intended Workspace

When working in Postman:

- open only the workspace you intend to change
- keep an eye on which workspace is active
- verify that the files appearing in Git belong to the correct folder

Examples:

- if you work in `Demo`, Git changes should appear under `workspaces/Demo`
- if you work in `Preprod`, Git changes should appear under `workspaces/Preprod`
- if you work in `Stage`, Git changes should appear under `workspaces/Stage`

This is the simplest safety rule for avoiding accidental cross-workspace commits.

## Step 10: Review Your Git Changes Before Committing

Always review the status:

```bash
git status
```

If you want more detail:

```bash
git diff -- workspaces/Demo
git diff -- workspaces/Preprod
git diff -- workspaces/Stage
```

Review before committing because Postman may update:

- collection definitions
- `.postman/resources.yaml`
- request files
- folder metadata
- supporting files in the workspace structure

## Step 11: Commit Only The Workspace You Changed

### If you changed only `Demo`

```bash
git add workspaces/Demo
git status
git commit -m "Update Demo workspace"
git push -u origin Roman_Katamai
```

### If you changed only `Preprod`

```bash
git add workspaces/Preprod
git status
git commit -m "Update Preprod workspace"
git push -u origin Roman_Katamai
```

### If you changed only `Stage`

```bash
git add workspaces/Stage
git status
git commit -m "Update Stage workspace"
git push -u origin Roman_Katamai
```

Replace `Roman_Katamai` with the teammate's personal branch name.

### If you intentionally changed multiple workspaces

```bash
git add workspaces/Demo workspaces/Preprod workspaces/Stage
git status
git commit -m "Update Postman workspaces"
git push -u origin Roman_Katamai
```

Again, replace `Roman_Katamai` with the teammate's personal branch name.

Important:

- prefer adding the specific workspace folder you changed
- avoid `git add .` unless you are absolutely sure everything should be committed

## Step 12: Create A Pull Request

After pushing your personal branch:

1. Open GitHub.
2. Create a pull request into `main`.
3. Ask for review if your team uses reviews.
4. Merge after approval.

This is the safest collaboration flow because everyone sees exactly what changed in each Postman workspace folder.

## Step 13: After Merge

After the pull request is merged:

```bash
git checkout main
git pull origin main
git checkout Roman_Katamai
git merge main
```

Replace `Roman_Katamai` with the teammate's own branch name.

Important:

- do not delete the personal branch after merge
- each teammate keeps their own branch and reuses it for future work

## Common Commands By Workspace

### Demo only

```bash
cd /path/to/eh_collections
git checkout main
git pull origin main
git checkout Roman_Katamai
git merge main
git add workspaces/Demo
git commit -m "Update Demo workspace"
git push -u origin Roman_Katamai
```

### Preprod only

```bash
cd /path/to/eh_collections
git checkout main
git pull origin main
git checkout Roman_Katamai
git merge main
git add workspaces/Preprod
git commit -m "Update Preprod workspace"
git push -u origin Roman_Katamai
```

### Stage only

```bash
cd /path/to/eh_collections
git checkout main
git pull origin main
git checkout Roman_Katamai
git merge main
git add workspaces/Stage
git commit -m "Update Stage workspace"
git push -u origin Roman_Katamai
```

Replace `Roman_Katamai` in the examples above with the teammate's personal branch name.

## How To Pull Latest Team Changes Safely

Recommended flow for every teammate:

```bash
git checkout main
git pull origin main
git checkout Roman_Katamai
git merge main
```

Replace `Roman_Katamai` with the teammate's personal branch name.

If conflicts appear, resolve them carefully and verify the affected Postman workspace folder before continuing.

## How To Recover If Postman Connects To The Wrong Folder

Symptoms:

- files appear in the wrong workspace folder
- `Demo` changes show up under `Preprod`
- Postman asks to initialize a new repository in a subfolder
- Postman points to an unexpected repository or path

Recovery steps:

1. Stop making changes.
2. Do not run `git init`.
3. In Postman, disconnect the current local folder connection.
4. Reconnect to the existing cloned repository root.
5. Choose the correct workspace path:
   - `/workspaces/Demo`
   - `/workspaces/Preprod`
   - `/workspaces/Stage`
6. Run `git status` and verify new changes now appear in the correct folder only.

## How To Recover If Git Says You Do Not Have Permission To Push

Check the remote:

```bash
git remote -v
```

Expected:

```text
origin  https://github.com/ezem001/eh_collections.git (fetch)
origin  https://github.com/ezem001/eh_collections.git (push)
```

If push fails:

1. Confirm your GitHub account has write access to the repository.
2. Confirm your local Git authentication is using the correct GitHub account.
3. Retry from your personal branch:

```bash
git push -u origin Roman_Katamai
```

Replace `Roman_Katamai` with the teammate's personal branch name.

## How To Recover If Postman Pulls Too Many Collections

If you suddenly see collections that belong to another environment:

1. Check which Postman workspace is currently open.
2. Check which local folder path is connected.
3. Confirm the folder path matches the workspace name.
4. If wrong, disconnect and reconnect correctly.
5. Recheck `workspaces/<workspace-name>/.postman/resources.yaml`.

This file is often a useful clue, because it maps the local workspace files to Postman Cloud resources for that specific workspace.

## How To Inspect What Changed

Useful commands:

```bash
git status
git diff -- workspaces/Demo
git diff -- workspaces/Preprod
git diff -- workspaces/Stage
```

To inspect only filenames:

```bash
git diff --name-only
```

To inspect staged changes:

```bash
git diff --cached
```

## Team Conventions We Recommend

Use these team conventions to avoid confusion:

1. One permanent personal branch per teammate.
2. One workspace per commit when possible.
3. Clear commit messages:
   - `Update Demo workspace`
   - `Add Preprod regression collections`
   - `Fix Stage environment requests`
4. Always review `git status` before commit.
5. Never commit changes from a workspace you did not intentionally edit.
6. Prefer pull requests from the personal branch into `main`.

## Frequently Asked Questions

### Do we need a separate GitHub repository for each Postman workspace?

No.

We intentionally use one GitHub repository for the team and separate folders inside it:

- `workspaces/Demo`
- `workspaces/Preprod`
- `workspaces/Stage`

### Do we need a separate local clone for each workspace?

No.

Use one local clone of the repository, then connect each Postman workspace to its own path inside the same clone.

### Should we run `git init` inside a workspace folder?

No.

Never do that. It creates nested repositories and breaks the shared setup.

### Should we use `git add .`?

Usually no.

Prefer:

- `git add workspaces/Demo`
- `git add workspaces/Preprod`
- `git add workspaces/Stage`

This reduces accidental commits.

### Does Git push update Postman Cloud automatically?

No.

Git and Postman Cloud are related but separate flows. Use Postman actions when you need synchronization with Postman Cloud itself.

### Does Pull from Postman Cloud create Git commits automatically?

No.

After Postman changes local files, you still need to commit and push with Git.

## Quick Checklist For New Teammates

1. Clone the repository.
2. Verify GitHub write access works.
3. Create your personal branch, for example `Roman_Katamai`.
4. Open Postman Desktop App.
5. Connect `Demo` to `/workspaces/Demo`.
6. Connect `Preprod` to `/workspaces/Preprod`.
7. Connect `Stage` to `/workspaces/Stage`.
8. Work in `Local View`.
9. Before each work session, update `main`, switch to your personal branch, and merge `main` into it.
10. Commit only the workspace folder you changed.
11. Push your personal branch and create a pull request into `main`.

## Repository Location Reminder

Repository URL:

- [https://github.com/ezem001/eh_collections](https://github.com/ezem001/eh_collections)

Expected local clone example:

```text
/Users/<your-user>/Projects/eh_collections
```

Workspace paths inside the repository:

- `/workspaces/Demo`
- `/workspaces/Preprod`
- `/workspaces/Stage`
