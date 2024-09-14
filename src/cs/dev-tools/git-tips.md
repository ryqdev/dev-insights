# Essential Git Commands and Best Practices

## Introduction to Git

Git is a distributed version control system designed to handle everything from small to very large projects with speed and efficiency. It is widely used for version control, collaboration, and managing changes in codebases.

## Commit Messages and Conventions

### Semantic Commit Messages

Using semantic commit messages helps in understanding the purpose of each commit. Here are the standard prefixes:

- `feat`: A new feature for the user (not a new feature for build scripts).
- `fix`: A bug fix for the user (not a fix to a build script).
- `docs`: Changes to the documentation.
- `style`: Code formatting changes, such as missing semicolons, without any production code change.
- `refactor`: Refactoring production code, such as renaming a variable, without any behavior change.
- `test`: Adding or refactoring tests; no production code change.
- `chore`: Updating build scripts or tasks; no production code change.

**Example:**

```shell
feat: add hat wobble
^--^  ^------------^
|     |
|     +-> Summary in present tense.
|
+-------> Type: chore, docs, feat, fix, refactor, style, or test.
```

## Git Ignore

`.gitignore` is a configuration file that tells Git which files or folders to ignore in a project. It helps avoid committing unnecessary files, such as log files, build artifacts, or temporary files.

**Example `.gitignore` file:**

```shell
*.sh          # Ignore all .sh files
.settings/    # Ignore the .settings directory
!*.md         # Do not ignore .md files
```

## Git Logs

The `git log` command displays the commit history of a repository in reverse chronological order. Customize the log output for better readability:

```shell
git log
# Custom log format with colors, graph, and commit details
git log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%ci) %C(bold blue)<%an>%Creset' --abbrev-commit
```

## Resetting Changes in Git

Git provides several commands to undo changes at different stages. Here is how to reset changes from various stages:

### 1. Reset Changes from Disk to Working Directory

Restore the changes of a file from the working directory to its last committed state:

```shell
git restore <changed_file>
```

### 2. Reset Changes from Staging Area to Working Directory

Keep changes in the working directory but unstage them:

```shell
git reset <changed_file>
# Or
git restore --staged <changed_file>
```

Discard changes in the working directory:

```shell
git checkout HEAD <changed_file>
```

`HEAD` represents the latest commit.

### 3. Reset Last Commit to Staging Area

Move the last commit back to the staging area while keeping changes:

```shell
git reset --soft HEAD~1
```

`HEAD~1` refers to the commit before the latest commit.

### 4. Reset Last Commit to Working Directory

Keep the changes in the working directory:

```shell
git reset HEAD~1
# Or
git reset --mixed HEAD~1
```

Discard the changes in the working directory:

```shell
git reset --hard HEAD~1
```

## Reverting Changes in Git

### Revert on Main Branch

Undo the changes from the latest commit by creating a new commit:

```shell
git revert HEAD
git push
```

### Revert on a Feature Branch

Undo the latest commit and force-push the changes:

```shell
git reset --hard HEAD~1
git push -f
```

Be cautious when using `-f` with `git push` on shared remote branches.

## Git Reset vs. Git Revert

### `git reset`

Moves the HEAD and can affect the staging area and working directory.

```shell
git reset [commit hash]
git reset [commit hash] --soft  # Keep changes in staging area
git reset [commit hash] --hard  # Discard changes completely
```

### `git revert`

Creates a new commit that undoes the changes of a previous commit without rewriting history:

```shell
git revert <commit id>
```

To revert a merge commit:

```shell
git revert <commit id> -m 1  # Reverts the changes from the other branch
git revert <commit id> -m 2  # Reverts the changes from the current branch
```

## Handling Merge Conflicts

### Method 1: Merge

Merging is simple and keeps the history intact but can lead to cluttered logs.

```shell
git merge origin/main
```

### Method 2: Rebase

Rebasing makes the commit history cleaner but is harder to master and may change commit IDs.

```shell
git rebase origin/main
```

#### Best Practice

- Use `git rebase` on feature branches.
- Use `git merge` on the main branch.

## Deleting Branches

Delete a local branch and its corresponding remote branch:

```shell
git checkout main
git branch -d foo
git push origin --delete foo
git branch -a  # Verify deletion
```

## Managing Remote Repositories

### Remote Management Commands

- List remote repositories:

```shell
git remote
git remote -v
```

- Add a new remote repository:

```shell
git remote add origin <url>
```

- Change the remote URL:

```shell
git remote set-url origin <url>
```

## Git Tags

Tags mark specific points in the commit history. Create and push a tag:

```shell
git tag -a v1.1 -m "Release v1.1"
git push --tags
```

## Cleaning Up Untracked Files

Remove untracked files and directories from the working directory:

```shell
git clean -fd
```

## Useful Git Aliases

Set up Git aliases in `~/.gitconfig` to speed up your workflow:

```shell
[alias]
    lg = log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%ci) %C(bold blue)<%an>%Creset' --abbrev-commit
```

## Git Commands in Shell Aliases

Add useful Git shortcuts to `~/.zshrc`:

```shell
alias reignore='git rm -r --cached . && git add .'
alias whyignore='git check-ignore -v'
alias gg='git lg'
alias gb='git branch'
alias gco='git checkout'
alias gac="git add . && git commit"
alias gs="git status"
```

## Understanding HEAD in Git

- `HEAD~`: Points to the parent commit of HEAD.
- `HEAD^`: Equivalent to `HEAD~`.
- `HEAD@{}`: Refers to previous states of HEAD.

Example commands:

```shell
git reset --hard HEAD~  # Go back to the commit before HEAD
git reset --hard HEAD~2 # Go back two commits before HEAD
git reset --hard HEAD^  # Equivalent to HEAD~
```

## References

- [How to Use Git HEAD](https://lightrun.com/how-to-use-git-head/)
- [Git Cheat Sheet by Josh Buchea](https://gist.github.com/joshbuchea/6f47e86d2510bce28f8e7f42ae84c716)

---