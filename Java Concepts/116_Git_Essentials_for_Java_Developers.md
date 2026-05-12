# Git Essentials for Java Developers

Version control is part of daily Java work: feature branches, code review, and reproducible builds. This note focuses on **Git** commands and habits that pair well with Maven/Gradle projects.

## Core Commands

```bash
git clone <url>
git status
git diff
git add -p
git commit -m "Describe intent in present tense"
git pull --rebase
git push
```

## Branching Model (Typical Team)
- **`main` / `master`**: releasable history.
- **Short-lived feature branches**: `feature/payment-idempotency` from latest `main`.
- Open a **pull request** / merge request for review before merge.

## Useful Quality-of-Life
- **`git stash push -m "wip"`** — park uncommitted work before switching branches.
- **`git bisect`** — binary search for the commit that introduced a regression.
- **`git log --oneline --graph -20`** — visualize recent history.

## Rebase vs Merge
- **`git pull --rebase`**: replay your commits on top of remote `main` for a linear history (team policy permitting).
- **Merge commits**: preserve exact branch topology; fine for long-lived integration branches.

Avoid rewriting **shared** history (`force push`) without team agreement.

## Java-Specific `.gitignore` Ideas
Ignore build outputs and IDE noise, for example:
- **`target/`** (Maven), **`build/`** (Gradle)
- **`.idea/`**, **`*.iml`**, **`.classpath`**, **`.project`**, **`.settings/`**
- **`*.class`**, **`*.jar`** except intentionally versioned wrapper JARs (teams differ on committing wrappers—usually **do** commit Maven/Gradle wrapper files).

## Commit Messages
- **Imperative mood**: “Add refund validation” not “Added”.
- **Why**, not only **what**, when non-obvious.

## Hooks and CI
- **Pre-commit**: formatting (Spotless), import order.
- **CI**: `mvn verify` / `./gradlew check` on every PR.

## Related Topics
- `94_CI_CD_Java.md`
- `65_Code_Quality_Tools.md`
