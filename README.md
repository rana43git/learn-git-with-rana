# 📘 Git Commands Documentation (Enhanced)

This document covers **Git commands** in a learning order, starting from basic setup to advanced operations.  
It also shows **alternative commands** where applicable and explains **differences**.

---

## 1️⃣ Git Configuration

### View Configurations
```bash
git config --list              # Shows all git configurations (local + global + system)
git config --local --list      # Shows configurations specific to the current repository
git config --global --list     # Shows configurations for the current user
git config --system --list     # Shows configurations for the entire system (needs admin rights)
```

### Set User Info
```bash
git config user.name "GitHub username"             # Local: only for current repository
git config user.email "GitHub email"               # Local

git config --global user.name "GitHub username"    # Global: applies to all repositories of current user
git config --global user.email "GitHub email"
```

> 💡 *Best practice:* Always set global user.name and user.email before starting work.

### Remove User Info
```bash
git config --unset user.name                       # Removes local username setting
git config --unset user.email
git config --global --unset user.name              # Removes global username
git config --global --unset user.email
```

---

## 2️⃣ SSH Setup for GitHub

Generate SSH keys for secure authentication with GitHub (preferred over HTTPS for ease):
```bash
cd ~/.ssh                                           # Navigate to SSH folder (Linux/macOS)
# On Windows, usually in C:\Users\<User>\.ssh
ssh-keygen -o -t rsa -C "GitHub email"             # Generate new SSH key pair
```

> 🔑 After generating, add the public key (`id_rsa.pub`) content to your GitHub SSH keys settings.

---

## 3️⃣ Creating & Initializing a Repository
```bash
git init                                            # Initialize a new Git repository locally
```

> You only need `git init` once per project folder.

---

## 4️⃣ Git Areas — Understanding the Workflow

- **Working Directory (Working Area):** Your actual files on disk, can be modified anytime.
- **Staging Area (Index):** Files added with `git add`, ready to be committed.
- **Repository:** Where commits are stored.
- **Remote Repository:** GitHub or other servers where your repo is hosted.

---

## 5️⃣ Checking File Status
```bash
git status                                          # Shows files staged, unstaged, untracked
```

> Helps you understand what changes are ready to commit.

---

## 6️⃣ Adding Files to Staging Area
```bash
git add file.txt                                    # Add a specific file
git add .                                           # Add all changes in current directory (new & modified)
git add -A                                          # Add all changes (including deletions)
git add *.txt                                       # Add all .txt files in current directory
git add **/*.txt                                    # Add all .txt files recursively (needs Bash 4+)
```

> 🔄 `git add .` vs `git add -A`:  
> - `git add .` adds new and modified files in the current directory and below, **but does NOT stage deletions**.  
> - `git add -A` stages **all** changes, including file deletions.

---

## 7️⃣ Removing from Staging or Undoing Changes
```bash
git rm --cached file.txt                            # Unstage file but keep it locally (remove from Git tracking)
git restore file.txt                                # Undo changes in working directory (unstaged changes)
git restore --staged file.txt                       # Remove file from staging area
```

> Use `git restore` for undoing unwanted changes safely (Git 2.23+).

---

## 8️⃣ Viewing Changes (Diff)
```bash
git diff                                            # Show unstaged changes between working dir and staging
git diff --staged                                   # Show staged changes ready to be committed
```

---

## 9️⃣ Committing Changes

```bash
git commit -m "Your message in imperative"          # Commit staged changes with message
git commit -am "Your message"                       # Add tracked files and commit in one step
```

> 💡 *Important:*  
> - `git commit -am` **does not add untracked (new) files**. Use `git add` for new files first.  
> - Commit messages should be in **imperative mood** (e.g., "Fix bug", not "Fixed bug").

### Multi-line commit messages
```bash
git commit -m "Main message" \
    -m " - detail 1" \
    -m " - detail 2" \
    -m " - detail 3"
```

---

## 🔟 Viewing Commit History

```bash
git log                                             # Full commit log with details
git log --oneline                                   # One line per commit (short hash + message)
git log --oneline --graph --all                     # Branches and merges visualized
git log --graph --all                               # Graph without shortening commit info
```

---

## 1️⃣1️⃣ Resetting and Undoing Commits

```bash
git reset --soft HEAD^                              # Move HEAD back one commit, keep all changes staged
git reset HEAD^                                     # Move HEAD back one commit, keep changes unstaged
git reset --hard HEAD^                              # Move HEAD back one commit, discard all uncommitted changes
```

> ⚠️ `--hard` is destructive — be careful.

---

## 1️⃣2️⃣ Restoring Files

```bash
git restore file.txt                                # Revert file in working directory to last commit version
git restore --staged file.txt                       # Unstage the file (remove from staging)
```

---

## 1️⃣3️⃣ Showing Commit Details
```bash
git show commit_id                                  # Show full details of a specific commit
git show                                             # Show last commit details
git show HEAD~2                                     # Show commit 2 steps before HEAD
```

---

## 1️⃣4️⃣ Checkout (Switching Commits or Branches)

```bash
git checkout commit_id                              # Switch to a specific commit (detached HEAD)
git checkout branch_name                            # Switch to existing branch
git checkout -b branch_name                         # Create and switch to a new branch
```

> Detached HEAD means you are not on any branch; commits here are not saved to branches unless you create one.

---

## 1️⃣5️⃣ Branch Management

```bash
git branch branch_name                              # Create a new branch
git branch -M main                                  # Rename current branch to main (force)
git branch -d branch_name                           # Delete a branch (safe delete, refuses if unmerged)
git branch -D branch_name                           # Force delete a branch (dangerous if unmerged)
```

---

## 1️⃣6️⃣ Merging Branches

```bash
git merge branch_name                               # Merge specified branch into current branch
```

> ⚠️ Conflicts may occur during merge; resolve manually and commit the merge.

---

## 1️⃣7️⃣ Ignoring Files: `.gitignore`

Create a `.gitignore` file to prevent tracking unwanted files:

```
*.txt              # Ignore all .txt files
text?.txt          # Ignore files like text1.txt, textA.txt (single character wildcard)
!main.txt          # Do NOT ignore main.txt (negate ignore)
Folder/            # Ignore all contents inside Folder/
```

---

## 1️⃣8️⃣ Git Aliases (Shortcuts)

```bash
git config --global alias.st status                 # Set alias: 'git st' for 'git status'
git config --global alias.co checkout               # 'git co' for 'git checkout'
git config --global alias.br branch                 # 'git br' for 'git branch'
git config --global --unset alias.st                # Remove alias
```

> Aliases help speed up your workflow.

---

## 1️⃣9️⃣ Working with Remote Repositories

```bash
git clone <repository link>                         # Clone remote repository to local machine
git remote -v                                       # Show remote URLs and names (origin etc.)
git remote add origin <repository link>             # Add remote named 'origin'
git push -u origin branch_name                      # Push branch and set upstream (track remote)
git pull                                            # Fetch and merge changes from remote branch
```

> 🛠️ *Tip:*  
> Always `git pull` before pushing to avoid conflicts.

---

## 2️⃣0️⃣ Additional Tips & Best Practices

- Use descriptive, imperative commit messages for clarity.  
- Commit often with small logical changes.  
- Keep `.gitignore` updated to avoid committing build files, secrets, or large binaries.  
- Use branches to develop features independently, then merge into main.  
- Frequently push to remote repositories for backup and collaboration.  
- Use `git fetch` to update remote info without merging.

---

## Summary of Differences for Common Alternatives

| Command                  | Description                                      | Notes                                    |
|--------------------------|------------------------------------------------|------------------------------------------|
| `git add .`              | Adds new/modified files in current directory    | Does **not** stage deletions             |
| `git add -A`             | Adds new, modified, and deleted files           | Includes deletions                        |
| `git commit -m`          | Commit staged changes                            | Must stage files first                    |
| `git commit -am`         | Add & commit tracked files                       | Does **not** add new untracked files    |
| `git branch -d`          | Delete branch if fully merged                    | Safe delete, refuses if unmerged         |
| `git branch -D`          | Force delete branch                              | Dangerous if branch not merged            |

---

Happy Git-ing! 🚀
