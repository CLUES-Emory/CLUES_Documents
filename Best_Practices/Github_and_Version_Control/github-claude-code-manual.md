# CLUES Lab Guide: Working with GitHub Repositories Using Claude Code

**Version 1.0 | Created: February 12, 2026**

---

**⚠️ IMPORTANT: Claude Code is an AI assistant that can and will make mistakes.** It may introduce bugs, use incorrect logic, misunderstand your intent, choose inappropriate statistical methods, or produce code that runs without errors but generates scientifically incorrect results. This last category is the most dangerous because it can go undetected.

**Every line of code Claude produces must be reviewed, tested, and validated by you before it is committed or included in a pull request.** You are responsible for the correctness of your contributions, not Claude. Treat Claude Code as a fast but fallible collaborator — it drafts, you verify.

Before submitting a PR, always:

- Run the code and confirm it produces expected output.
- Check that parameter choices are scientifically justified.
- Verify results against known standards or QC samples where possible.
- Test edge cases (empty inputs, missing values, unusual sample types).
- Confirm the code follows our lab's coding standards.

If you do not understand what Claude's code is doing, ask it to explain. If the explanation does not make sense, do not commit the code. Consult Doug or another lab member instead.

---

## Purpose

This manual describes the standard workflow for editing, reviewing, and contributing to CLUES lab GitHub repositories using Claude Code. It is written for lab members at all experience levels, including those new to Git, GitHub, and AI-assisted development.

## What You Need Before Starting

### Accounts and Access

- A GitHub account (free at github.com)
- Access to the relevant CLUES lab repository (ask Doug to add you as a collaborator)
- A Claude subscription (Pro, Max, Team, or Enterprise) or an Anthropic API key

### Choosing Your Environment and Setting Up

Claude Code works in three environments. Choose the one that fits your setup and comfort level, then follow the corresponding setup guide to install Git, Node.js, and Claude Code.

| Environment | Best for | Setup guide |
|---|---|---|
| Mac Terminal | Mac users comfortable with the command line | [Setup Guide for Mac](setup-mac.md) |
| Windows PowerShell | Windows users comfortable with the command line | [Setup Guide for Windows](setup-windows.md) |
| VS Code | Anyone who prefers a visual editor; mixed Mac/Windows teams | [Setup Guide for VS Code](setup-vscode.md) |

If you are unsure which to use, start with VS Code. It provides the same Claude Code functionality with a familiar editor interface and works identically on Mac and Windows.

Complete your platform's setup guide before continuing with the workflow below.

## Core Concepts

### Repository

A repository (repo) is a project folder tracked by Git. It contains all the code, data processing scripts, documentation, and the full history of every change ever made. Our lab repos are hosted on GitHub.

### Branch

A branch is a parallel version of the repository where you can make changes without affecting the main codebase. Think of it as making a copy of a lab protocol to try modifications before replacing the original. The default branch is called `main`.

### Commit

A commit is a saved snapshot of your changes with a descriptive message. Each commit records what changed, who changed it, and when.

### Pull Request (PR)

A pull request is a proposal to merge your branch into `main`. It creates a page on GitHub showing exactly what changed, where other lab members and Doug can review the code before it is accepted. PRs are the primary mechanism for quality control in our repositories.

### Fork

A fork is a personal copy of someone else's repository under your own GitHub account. You would fork a repo if you do not have direct write access. After making changes on your fork, you submit a PR back to the original repo.

### Clone

Cloning downloads a copy of a GitHub repository to your local computer so you can work on it.

### Tag

A tag is a permanent label on a specific commit, used to mark milestones like manuscript submissions or stable pipeline versions. Unlike branches, tags do not move — they always point to the exact same snapshot. See the Git Tags section for full details.

## Naming Conventions

Consistent naming across the lab makes code easier to read, review, and maintain. These conventions apply to all CLUES lab repositories.

### File Names

R scripts use numbered prefixes to indicate processing order, followed by a descriptive name in `snake_case` (all lowercase, underscores between words):

| Good | Bad | Why |
|---|---|---|
| `01_read_data.R` | `ReadData.R` | Use snake_case and numbered prefix |
| `02_peak_detection.R` | `peakDetection.r` | Consistent case, `.R` not `.r` |
| `06_filtering_qc.R` | `6-filtering&QC.R` | Zero-pad numbers, no special characters |
| `helpers.R` | `misc_functions_v2_FINAL.R` | No version numbers in filenames (that is what Git is for) |

Other file types follow the same principle:

- Configuration: `xcms_params.yaml`, `rp_c18_positive.yaml`
- Data: `phenodata_pfas_cohort.csv`, `sample_metadata.csv`
- Documentation: `parameter_optimization.md`, `batch_correction_methods.md`
- Output: `feature_table_filtered.csv`, `qc_cv_report.html`

Never use spaces in filenames. Never use version indicators like `_v2`, `_final`, `_FINAL_v3` in filenames — Git tracks versions for you.

### Why No Version Numbers in Filenames?

Before version control, it was common to save different versions of a file by adding suffixes to the filename. Most of us have seen folders that look like this:

```
peak_detection.R
peak_detection_v2.R
peak_detection_v2_revised.R
peak_detection_v3_FINAL.R
peak_detection_v3_FINAL_dougEdits.R
peak_detection_v3_FINAL_FINAL.R
```

This creates real problems: it is unclear which file is actually current, there is no record of what changed between versions, old copies take up space and cause confusion, and if two people create their own `_v2` at the same time, the files diverge with no way to reconcile them.

Git eliminates all of this. There is only one `peak_detection.R` in the repository. Every time you commit a change, Git saves a snapshot of the file at that point. You can view any previous version, compare any two versions side by side, see exactly what changed and who changed it, and restore an old version if needed. The full history is preserved automatically — you never lose a previous version, and you never need to encode that history into the filename.

If you need to mark a significant milestone (for example, "this is the version of the pipeline we used in our submitted manuscript"), use a Git tag:

```
tag this commit as v1.0-manuscript-submission
```

Claude Code will create a tag that permanently marks that exact state of the entire repository. Anyone can check out that tag later to see exactly what the code looked like at that point, across all files, not just one. See the Git Tags section below for full details.

This is also why the version numbers inside our documents (like "Version 1.0" at the top of this guide) serve a different purpose — they are metadata for human readers who might encounter the document outside of Git (printed, emailed, or on a shared drive). They do not replace Git's version tracking and should not be added to filenames.

### R Variables and Functions

All variable and function names use `snake_case`:

| Good | Bad | Convention violated |
|---|---|---|
| `peak_table` | `peakTable` | camelCase; use snake_case |
| `rt_correction` | `RTCorrection` | PascalCase; use snake_case |
| `filter_by_cv` | `filterByCV` | camelCase; use snake_case |
| `n_samples` | `nSamples` | camelCase; use snake_case |
| `qc_threshold` | `QC.threshold` | Dot notation (old R style); use snake_case |
| `max_peakwidth` | `max.peakwidth` | Dot notation; use snake_case |

**Functions** should be named as verbs describing what they do:

- `calculate_feature_cv()` — computes coefficient of variation
- `filter_blank_features()` — removes blank-contaminated features
- `plot_tic_overlay()` — generates a TIC overlay plot
- `load_xcms_params()` — reads parameters from YAML config
- `export_feature_table()` — writes results to CSV and RDS

**Variables** should be named as nouns describing what they contain:

- `raw_data` — the unprocessed MSnExp or XCMSnExp object
- `peak_table` — data frame of detected peaks
- `qc_samples` — logical vector or subset of QC samples
- `rt_deviation` — numeric vector of retention time shifts
- `batch_ids` — factor of batch assignments

**Avoid single-letter names** except in short loops (`i`, `j`) or well-established conventions (`n` for count, `p` for number of features, `m` for number of samples). If you find yourself needing a comment to explain what a variable name means, the name should be more descriptive.

### Pull Request Titles

PR titles follow the same imperative mood as commit messages and should summarize the overall purpose of the branch. Include the type prefix if it helps clarity:

**Good PR titles:**

- "Add blank subtraction step with configurable threshold"
- "Fix RT alignment failure for HILIC negative mode samples"
- "Update XCMS parameters for GC-HRMS pyrolysis method"
- "Refactor batch correction into modular functions"
- "Add QC report generation with CV distributions"

**Bad PR titles:**

- "Updates" — too vague
- "Doug's changes" — names the person, not the work
- "WIP feature" — do not open PRs for incomplete work unless marked as draft
- "fix #42" — reference the issue, but also describe what the fix does

### GitHub Issues

When creating issues to track tasks or bugs, use a clear descriptive title and include enough context for someone unfamiliar with the problem:

**Good issue titles:**

- "Peak detection misses low-intensity features below SNR threshold"
- "Add HILIC negative mode parameter set to xcms_params.yaml"
- "RT alignment crashes when QC samples are missing from batch 3"
- "Evaluate ChromPeakAreaParam vs FillChromPeaksParam for gap-filling"

## Standard Workflow

The following steps describe how to make changes to a CLUES lab repository. Once you have completed your platform setup guide, the workflow is the same regardless of which environment you use.

### Step 1: Clone the Repository

If this is your first time working with the repo, clone it to your computer. Your platform setup guide covers the specific steps, but the short version:

**Terminal (Mac or Windows):**

```
git clone https://github.com/clues-lab/repo-name.git
cd repo-name
```

**VS Code:** Open the Command Palette (Cmd+Shift+P on Mac, Ctrl+Shift+P on Windows), type "Git: Clone", and paste the repository URL.

If you have already cloned the repo, make sure you are up to date before starting new work:

```
git checkout main
git pull origin main
```

### Step 2: Launch Claude Code

**Terminal (Mac or Windows):** Navigate to the project directory and type `claude`.

**VS Code:** Click the Claude icon in the sidebar.

In all environments, Claude Code will read the `CLAUDE.md` file in the project root at the start of each session. You do not need to re-explain the project context.

### Step 3: Create a Branch

Never work directly on `main`. Tell Claude Code to create a branch for your work:

```
create a new branch called feature/add-blank-subtraction
```

This works the same way in all three environments — you type the request in natural language and Claude Code handles the git commands.

Branch names should use the format `type/short-description` with all lowercase letters and hyphens between words (no spaces, no underscores, no capital letters). The type prefix tells everyone at a glance what kind of work the branch contains.

**Branch type prefixes:**

| Prefix | Use for | Examples |
|---|---|---|
| `feature/` | New functionality, processing steps, or capabilities | `feature/add-blank-subtraction`, `feature/hilic-parameter-set`, `feature/add-qc-cv-filter` |
| `fix/` | Bug fixes and corrections | `fix/rt-alignment-drift`, `fix/missing-phenodata-column`, `fix/peakwidth-off-by-one` |
| `analysis/` | Study-specific analysis work | `analysis/pfas-placenta-cohort`, `analysis/breast-cancer-pfas-2026`, `analysis/wastewater-pilot` |
| `docs/` | Documentation, README, or CLAUDE.md updates | `docs/update-readme`, `docs/add-parameter-rationale`, `docs/xcms-workflow-diagram` |
| `refactor/` | Code reorganization without changing behavior | `refactor/split-processing-steps`, `refactor/move-helpers-to-utils`, `refactor/consolidate-params` |
| `test/` | Adding or updating tests | `test/add-filtering-tests`, `test/qc-edge-cases` |

**Good branch names:**

- `feature/add-obiwarp-rt-correction` — clear, specific, describes the feature
- `fix/gap-filling-na-values` — identifies the problem area
- `analysis/liver-cancer-exposome-2026` — includes study name and year
- `docs/add-batch-correction-methods` — says exactly what documentation is changing

**Bad branch names:**

- `my-branch` — no type prefix, not descriptive
- `update` — too vague, could mean anything
- `fix_stuff` — uses underscores instead of hyphens, not specific
- `Feature/Add-Blank-Subtraction` — do not use capital letters
- `feature/add-a-new-blank-subtraction-step-that-flags-features-with-intensity-in-blanks-greater-than-one-third` — too long; keep it under 5–6 words after the prefix

### Step 4: Make Your Changes

Tell Claude Code what you want to do in plain language. Examples:

```
add a blank subtraction step after gap filling that flags features with
mean blank intensity greater than one-third of the mean sample intensity
```

```
optimize the CentWave peakwidth parameter for our HILIC method using
the QC samples in data/raw/qc/
```

```
refactor the batch correction script to use ComBat from the sva package
and add before/after PCA plots
```

Claude Code will read the relevant files, propose changes, and ask for your approval before editing. Review what it proposes before accepting.

### Step 5: Review Your Changes

Before committing, ask Claude Code to summarize what changed:

```
summarize the changes I've made in this session
```

You can also ask it to show the diff:

```
show me the git diff
```

Look for: correct logic, adherence to our coding standards (snake_case, explicit namespacing with `::`, roxygen2 docstrings), and that no raw data or sensitive information is being committed.

### Step 6: Commit Your Changes

Ask Claude Code to commit with a descriptive message:

```
commit my changes with the message "Add blank subtraction step with configurable intensity threshold"
```

Commit messages should follow these conventions:

**Use imperative mood** — write the message as a command, as if you are telling the codebase what to do. This matches the convention Git itself uses (e.g., "Merge branch...").

| Good (imperative) | Bad |
|---|---|
| Add blank subtraction step | Added blank subtraction step |
| Fix RT alignment for HILIC samples | Fixed RT alignment for HILIC samples |
| Update CentWave peakwidth to [5, 20] | Updating CentWave peakwidth |
| Remove deprecated CAMERA annotation | Removed old annotation code |

**Keep the first line short** — aim for 50 characters or fewer. This is the summary that appears in Git logs and on GitHub. If you need to explain more, add a blank line and then a longer description.

**Be specific about what changed and why.** The commit message should tell someone reviewing the history what happened without needing to read the code.

**Good commit messages:**

```
Add ObiwarpParam RT correction with binSize 0.5
```
```
Fix gap-filling failure on samples with zero detected peaks
```
```
Update QC CV threshold from 30% to 25% per reviewer feedback
```
```
Add before/after PCA plots for batch correction QC

Generates PCA score plots colored by batch before and after ComBat
correction. Saves to output/figures/ as PNG and PDF.
```
```
Refactor peak detection into standalone function

Extract CentWave peak detection from monolithic script into
R/utils/detect_peaks.R with roxygen2 documentation and unit tests.
No changes to processing logic or parameters.
```

**Bad commit messages:**

- `update code` — says nothing about what changed
- `fix` — which fix? where?
- `WIP` — do not commit work in progress to a shared branch; finish the thought first
- `asdf` or `temp` — never commit with placeholder messages
- `changes to 02_peak_detection.R` — names the file but not what changed or why

Make small, focused commits. If you made several unrelated changes, commit them separately with distinct messages. For example, if you fixed a bug and also updated documentation, those should be two separate commits — not one commit with the message "Fix bug and update docs."

### Step 7: Push and Open a Pull Request

When your work is ready for review, first update the changelog:

```
update the changelog for my changes in this branch
```

Then push and open the PR:

```
push my branch and create a pull request
```

Claude Code will push your branch to GitHub and open a PR. It generates a description based on your actual code changes.

For PRs that change data processing parameters or methods, include before/after QC plots or summary statistics in the PR description so reviewers can evaluate the impact.

### Step 8: Respond to Review Feedback

After submitting a PR, Doug or another lab member will review it on GitHub. They may leave comments, request changes, or approve it. If changes are requested:

1. Open Claude Code in the same project directory
2. Switch to your branch: `switch to the feature/add-blank-subtraction branch`
3. Make the requested changes
4. Commit and push: `commit and push the review changes`

The PR will automatically update with your new commits.

### Step 9: Merge

Once the PR is approved, it can be merged into `main` on GitHub. After merging, clean up your local environment:

```
switch to main and pull the latest changes
delete the feature/add-blank-subtraction branch
```

## Maintaining a Changelog

Every CLUES lab repository should include a `CHANGELOG.md` file in the project root. This file provides a human-readable record of what changed, when, and why — making it easy for any lab member to understand the history of the project without digging through individual commits.

### Format

Use the following format, with the most recent changes at the top:

```markdown
# Changelog

## [Unreleased]

### Added
- Blank subtraction step with configurable intensity threshold (PR #12)

### Changed
- Updated CentWave peakwidth from [5, 30] to [5, 20] for HILIC method (PR #11)

### Fixed
- Retention time alignment failing on samples with <100 features (PR #10)

## [v1.2.0] - 2026-02-10

### Added
- ComBat batch correction with QC-based reference (PR #9)
- PCA plots before and after batch correction (PR #9)

### Changed
- Switched from CAMERA to MetaboAnnotation for adduct grouping (PR #8)

## [v1.1.0] - 2026-01-15

### Added
- Gap-filling step using ChromPeakAreaParam (PR #5)
- QC coefficient of variation filtering at 30% threshold (PR #6)
```

### Categories

Use these standard categories to organize your entries:

- **Added** — new features, scripts, processing steps, or functionality
- **Changed** — modifications to existing behavior, updated parameters, or refactored code
- **Fixed** — bug fixes
- **Removed** — deleted features or deprecated code
- **Deprecated** — features that will be removed in a future version

### When to Update

Update the changelog as part of every PR. Add your entry under the `[Unreleased]` section. When a new version is tagged for a release or milestone (for example, when a paper is submitted), move the unreleased items into a dated version block.

You can ask Claude Code to help:

```
update the changelog for my changes in this branch
```

```
add a changelog entry for the blank subtraction feature I just added
```

Claude Code will read the diff and draft an appropriate entry. Review it for accuracy before committing — remember that Claude may mischaracterize what changed or omit important details.

### Why This Matters

Changelogs are especially valuable in research code because they document the evolution of your processing pipeline. When a reviewer asks "when did you change the peak detection parameters?" or a lab member needs to reproduce results from six months ago, the changelog provides a clear answer. It also helps during manuscript preparation when you need to describe your methods accurately.

## Git Tags

A Git tag is a permanent label attached to a specific commit. Think of it as putting a bookmark on an exact snapshot of your entire repository at a particular moment in time. Unlike branches, which move forward as you add commits, a tag stays fixed on the exact commit where you placed it. This makes tags the right tool for marking milestones where you need guaranteed reproducibility.

### When to Use Tags

The most common use in research is marking milestones in your analysis pipeline:

- Submitting a manuscript — tag the exact state of the code used to generate your results
- Completing a major processing run — tag the pipeline version used for a specific cohort
- Releasing a stable version of a shared tool or package
- Before making major changes — tag the last known-good state as a safety net

### Types of Tags

There are two types. **Annotated tags** include a message, the tagger's name, and a date — these are preferred because they carry more context. **Lightweight tags** are just a name pointing to a commit with no additional metadata.

### Creating Tags with Claude Code

```
create an annotated tag v1.0 with the message "Pipeline used for breast cancer PFAS manuscript submission"
```

```
tag this commit as v2.0-pfas-placenta-final
```

```
create a tag v1.1 with the message "Updated CentWave parameters per reviewer feedback"
```

### Creating Tags with Git Commands

If you prefer using Git directly:

```bash
# Annotated tag (preferred)
git tag -a v1.0 -m "Pipeline used for breast cancer PFAS manuscript submission"

# Lightweight tag
git tag v1.0

# Tag a previous commit (if you forgot to tag at the time)
git tag -a v1.0 -m "Manuscript submission version" abc1234
```

Tags do not push to GitHub automatically. You must push them explicitly:

```bash
# Push a single tag
git push origin v1.0

# Push all tags at once
git push origin --tags
```

### Viewing and Using Tags

```bash
# List all tags
git tag

# See details of a specific tag
git show v1.0

# Check out a tagged version (creates a read-only detached HEAD)
git checkout v1.0

# See what changed between two tagged versions
git diff v1.0 v2.0

# Return to main after viewing a tagged version
git checkout main
```

In Claude Code, you can do all of this in natural language:

```
show me all the tags in this repo
```

```
what changed between v1.0 and v2.0
```

```
check out the v1.0 tag so I can see what the pipeline looked like at manuscript submission
```

### Tag Naming for Research

For research code, tying tags to meaningful events is often more useful than strict software versioning. Use names that tell a clear story:

| Tag | What it marks |
|---|---|
| `v1.0-submitted` | Code used for initial manuscript submission |
| `v1.1-revision1` | Updated pipeline after first round of reviewer comments |
| `v2.0-accepted` | Final version at manuscript acceptance |
| `v3.0-pfas-placenta-cohort` | Pipeline version used for a specific study |
| `v1.0-exposome-course-2026` | Stable version shared with students |

If you prefer formal semantic versioning, the convention is `vMAJOR.MINOR.PATCH`: bump the major number for breaking changes to the pipeline, the minor number for new features or processing steps, and the patch number for bug fixes. For example, `v1.0.0` to `v1.1.0` when you add a new blank subtraction step, and `v1.1.0` to `v1.1.1` when you fix a bug in that step.

### Tags and GitHub Releases

On GitHub, tags show up under the "Releases" tab of your repository. You can also create a formal GitHub Release from a tag, which lets you attach release notes and downloadable files. This is useful when you want to distribute a stable version of a processing pipeline to collaborators outside the lab, or when you want to archive a snapshot alongside a publication. To create a release, go to the Releases tab on GitHub, click "Draft a new release", select your tag, and add a description of what that version contains.

### Tags and the Changelog

Tags and the changelog work together. When you create a tag to mark a release, move the entries under `[Unreleased]` in your `CHANGELOG.md` into a new dated version block matching the tag name. This keeps the changelog and the Git history aligned, so anyone can look at either one to understand what changed and when.

## Working with Repositories You Do Not Own

If you need to contribute to a repository outside the CLUES lab organization (for example, an open-source R package):

1. Fork the repo on GitHub using the "Fork" button
2. Clone your fork (see Step 1 above, using your fork's URL)
3. Launch Claude Code and tell it: `add the original repo as an upstream remote at https://github.com/original-owner/repo-name.git`
4. Create a branch, make changes, and push to your fork
5. Open a PR from your fork to the original repo on GitHub

To stay in sync with the original:

```
fetch updates from the upstream repo and merge them into my main branch
```

## What Claude Code Can Do for You

Claude Code is not limited to writing code. Here are common tasks you can ask it to handle:

**Code and analysis tasks:**
- Write, edit, or refactor R scripts
- Debug errors by pasting the error message and asking for help
- Optimize XCMS parameters
- Write unit tests for utility functions
- Generate QC plots and reports

**Git and GitHub tasks:**
- Create branches, commit, push, and open PRs
- Summarize changes before committing
- Resolve merge conflicts
- Write descriptive commit messages and PR descriptions

**Documentation tasks:**
- Write or update README files
- Add roxygen2 documentation to functions
- Generate parameter optimization notes
- Draft methods sections describing processing workflows

**Code review:**
- Review a diff for potential issues
- Check adherence to coding standards
- Suggest improvements to existing code

## Things to Watch Out For

### Always Review and Test Before Submitting a PR

Claude Code will propose changes and ask for approval. Read the proposed changes carefully. Do not blindly accept, especially for data processing logic, statistical methods, or parameter choices that affect your results.

Before opening a PR, verify that the code actually works as intended. At a minimum: source the modified scripts and confirm they run without errors; check output files for expected dimensions, column names, and value ranges; compare results against a known-good run if one exists; and look for silent failures where the code runs but produces incorrect output. If the repository has unit tests, run them (`testthat::test_dir("tests/testthat/")`). If your changes affect existing tests, update them as part of the same PR.

### Never Commit Sensitive Data

Our `.gitignore` files are configured to exclude raw data, credentials, and large files. However, always double-check before committing. Never commit: raw `.mzML` or `.mzXML` files, API keys or passwords, participant identifiers or PHI, large intermediate files (`.rds` > 50 MB).

If you accidentally commit sensitive data, tell Doug immediately. Removing it from Git history requires special steps.

### Do Not Work Directly on Main

Always create a branch. This protects the main codebase from incomplete or untested changes and allows for review before merging.

### Communicate with the Team

If you are working on a part of the codebase that someone else is also editing, coordinate to avoid merge conflicts. Use GitHub Issues to track who is working on what.

### Keep Branches Short-Lived

Long-running branches diverge from `main` and become difficult to merge. Aim to open and merge PRs within a few days. If a feature takes longer, break it into smaller PRs.

## The CLAUDE.md File

Every CLUES lab repository should contain a `CLAUDE.md` file in the project root. This file is automatically read by Claude Code at the start of each session and contains project-specific context including coding standards, directory structure, parameter conventions, and QC requirements.

The `CLAUDE.md` is committed to the repository so that every lab member gets the same instructions when using Claude Code. If you need personal settings that differ from the team defaults, use `.claude/settings.local.json` instead — this file is automatically excluded from version control.

If you notice the `CLAUDE.md` is outdated or missing important context, submit a PR to update it.

## Quick Reference

| Task | What to tell Claude Code |
|---|---|
| Clone a repo | See your platform setup guide |
| Start Claude Code | See your platform setup guide |
| Create a branch | `create a branch called feature/my-feature` |
| See current branch | `what branch am I on` |
| Switch branches | `switch to the main branch` |
| Make changes | Describe what you want in plain language |
| View changes | `show me the git diff` or `summarize my changes` |
| Commit | `commit with message "Add RT correction step"` |
| Update changelog | `update the changelog for my changes in this branch` |
| Push and open PR | `push and create a pull request` |
| Tag a milestone | `create an annotated tag v1.0 with the message "Manuscript submission"` |
| List all tags | `show me all the tags in this repo` |
| View a tagged version | `check out the v1.0 tag` |
| Compare two tags | `what changed between v1.0 and v2.0` |
| Pull latest changes | `pull the latest changes from main` |
| Resolve a conflict | `help me resolve this merge conflict` |

## Troubleshooting

For installation and platform-specific issues, see the troubleshooting section in your setup guide (`setup-mac.md`, `setup-windows.md`, or `setup-vscode.md`).

### Merge conflicts

If Git reports a merge conflict, do not panic. Ask Claude Code:

```
help me resolve this merge conflict
```

Claude Code will walk you through the conflicting files and help you choose which changes to keep.

## Getting Help

- **GitHub documentation:** docs.github.com
- **Claude Code documentation:** docs.claude.com/en/docs/claude-code/overview
- **Git basics:** git-scm.com/book (free online book)
- **Lab-specific questions:** Ask Doug or post in the lab Slack channel

If something goes wrong with Git and you are unsure how to fix it, do not force-push or delete things. Ask Claude Code to explain the situation, or ask a lab member for help.
