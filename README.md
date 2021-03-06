# GitTutorialRemote
A quick guide to Git and GitHub.

## Git Glossary

#### Git / GitHub:
Git is a distributed version control system (DVCS) that synchronizes all changes transparently and asynchronously, storing the entire codebase and its full history. Github is a Git repository hosting service providing additional tools like issues, pull requests, code review, and predominantly serves as a host for a project's central/official repository.

#### Repository / Remote:
A repository is a collection of references together with an object database containing all objects reachable from the refs. A remote repository is a link to another version of the project hosted elsewhere. Remotes can be referenced by use of aliases using `git remote add <alias> <link>` and then subsequently using them as in `git push <alias> <branch>`.

#### Branch:
A branch is an active line of development, with the most recent commit on the branch referred to as the tip. A Git repository can track multiple branches, while your working tree is associated with just one of them, pointed to by HEAD. The master branch is the default active development branch.

#### Reference / Objects:
An object is a unit of storage in Git, identified by its unique object name/identifier created using the SHA-1 hash function, and which lives in the `.git/objects` object database (stored as hash map). Objects can have types: commit, tree, tag, blob.

A reference is a path usually beginning with `.git/refs` that points to an object or another ref. A head is a named reference to the commit at the tip of a branch, while `HEAD`/`@` is a special symbolic ref referring to the current branch, i.e. the head of the current working tree. In particular, `HEAD`, `HEAD^`, `HEAD~2` and `HEAD~3` represent the last four commits, while `ORIG_HEAD` represents the previous value of `HEAD`.

#### Tag:
A reference under `.git/refs/tags` namespace pointing to an object of arbitrary type. Since it is not updated by the `commit` command unlike head, it is typically used to mark a particular point in the commit ancestry chain. Tag objects contain such a ref, and possibly a message as well as a PGP signature.

#### Tree / Working Tree / Index:
An object containing a list of filenames and modes along with refs to associated blobs/tree objects, equivalent to a directory. Tree-ish is a tree object that can be recursively dereferenced to a tree object, including commit objects and symbolic refs to tree objects.

The working tree refers to the tree of the actual checked out files, including local changes made but not committed. An index is a collection of files with associated information, whose contents are stored as objects, effectively a stored version of the working tree. An index entry refers to one such file.

#### Commit:
An object containing the information about a particular revision (e.g. parent commits, committer, date, message, tree object corresponding to top directory of stored revision) and represents a single point in the project history of interrelated commits. Creating a commit saves the current state of the index and advances HEAD to point at the new commit. Commit-ish is a commit object that can be recursively dereferenced to a commit object.

#### Downstream / Upstream / Origin:
Describes relative positions in repository version control. Upstream refers to the repository containing the project source code. Conversely, any information copied from upstream goes downstream. Changes are pushed upstream to distribute work to other contributors. It is not uncommon for a developer to set two remotes: upstream pointing to the project and origin pointing to their own fork of the project. This allows the developer to pull enhancements directly from the original project to stay in sync.

### Other actions:

| Actions | Description |
|:-------:| ----------- |
| Push    | To get the branch head from a remote (which must necessarily be an ancestor to the local head), and put all objects reachable from the local head and missing from the remote into the remote object database and updating the remote head ref. |
| Fetch   | To get the branch head from a remote, and retrieve all objects missing from the local object database. |
| Merge   | To bring contents from another branch into the current branch, by identifying changes made since the branches diverged and applying those changes automatically. If the branch is a descendant of the current branch, the merge is a fast-forward. |
| Pull    | To fetch and merge. |
| Rebase  | To reapply a series of changes from a branch to a different base and reset the head to the result. |
| Resolve | To fix manually what the failed automatic merge left behind. |
| Rewind  | To throw away part of development by assigning the head to an earlier revision. |

## First-time setup for local git repository
```bash
# UPDATE GIT CONFIGURATION
# Writes to ~/.gitconfig (global) file rather than .git/config (repository)
# the email and name to be recorded in any newly created commits.
git config --global user.name <username>
git config --global user.email <email>

# CREATE LOCAL GIT REPOSITORY
# Create new local repository in project folder, i.e. a .git directory with
# object subdirectories and an initial "HEAD" file referencing the HEAD of the
# master branch. Deleting the .git folder removes the repository.
mkdir ProjectFolder
cd ProjectFolder
git init

# Create new file in project
notepad README.md

# STAGE
# Updates index (snapshot of working tree content) using content found in the
# working tree, and prepares it for the next commit.
# Specifies files to add to index, remove using git-rm instead of git-add
git add README.md

# COMMIT
# Stores current contents of index in new commit along with a log message.
git commit -m "Add README to project"
```

## First-time setup to link remote repository
```bash
# Create new empty remote repository on GitHub
# Retrieve the HTTPS link to the remote,
# e.g. https://github.com/pyuxiang/GitTutorialRemote.git

# LINK REPOSITORY TO NEW REMOTE
# origin is the default user-defined alias to the main remote repository
git remote add origin https://github.com/pyuxiang/GitTutorialRemote.git

# PUSH CHANGES TO REMOTE
# The "-u" flag sets the upstream for subsequent argument-less git-pull.
git push -u origin master

# PULL FROM REMOTE
git pull origin
```

## First-time setup for authentication
SSH (Secure Shell Host) keys are used to connect to remote servers. Note that instructions are provided for both Git Bash for Windows and the native bash in WSL (Windows Subsystem for Linux).

```bash
ssh-keygen -t rsa -b 4096 -C <email>  # gen 4096RSA SSH key with email label
eval $(ssh-agent -s)                  # start ssh-agent
ssh-add ~/.ssh/id_rsa                 # add generated RSA key

# Copy and add SSH public key to GitHub settings (use your browser for latter)
clip < ~/.ssh/id_rsa.pub              # Git Bash...
cat ~/.ssh/id_rsa.pub | clip.exe      # ...or WSL

# Authenticate the connection with the following fingerprint:
# SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8
ssh -T git@github.com
```
GPG (GNU Privacy Guard) keys are used to sign git commits.
```bash
gpg --gen-key                # gen 4096RSA GPG key on Git Bash...
gpg --full-generate-key      # ...or WSL

# Copy and add GPG key to GitHub settings (use your browser for latter)
gpg --list-keys --keyid-format SHORT
gpg --armour --export <keyid> | clip         # copy key in ASCII armour format
git config --global user.signingkey <keyid>  # set signing key in git
git config --global commit.gpgsign true      # enable auto-signing all commits

# Note if 'error: gpg failed to sign the data' occurs and running
# 'echo "test" | gpg --clearsign' yields `signing failed: Inappropriate ioctl for device`,
# welcome to poor UI doc and the fix explained [here](https://github.com/keybase/keybase-issues/issues/1712#issuecomment-141226705).
export GPG_TTY=$(tty)

# To automate verification of GPG signing, download a GPG key manager
# (GPG4Win for Windows) and import the key found in ~/.gnupg
# Set the GPG signing program to that supplied by GPG4Win.
git config --global gpg.program "C:/Program Files (x86)/GnuPG/bin/gpg.exe"
```

## Workflows
Stated here are common workflows when working with Git.

### Making changes
Versioning is performed as a two-step process of staging and committing.
```bash
git add <file>                # Stage file
git add -A                    # Match index to working tree
git reset <file>              # Unstage file
git commit -m <message>       # Records index in history
git rm <file>                 # Delete file and stage deletion
git rm --cached <file>        # Remove file from version control only
git mv <old_file> <new_file>  # Rename file and stage change
```

### Reviewing file differences
```bash
git status         # Show status of working tree
git diff           # Show staged file differences
git diff --staged  # Show unstaged file differences
```

### Grouping changes (branches):
Branching is performed to isolate codebase changes and preserve stability of master branch. For rebasing, check out [git-rebase](https://git-scm.com/docs/git-rebase).
```bash
git branch              # List all local branches in repository
git branch <branch>     # Create new branch
git checkout <branch>   # Switch to branch
git merge <branch>      # Combine histories of branches
git branch -d <branch>  # Delete branch
git rebase <branch>     # Reapply commits on specified branch's base tip
```

### Reviewing commit/branch differences
```bash
git log                      # Show commit history of repository
git log --follow <file>      # Show version history of file
git show <commit>            # Show specific commit
git diff <commit>..<commit>  # Compare commit differences
git diff <branch>..<branch>  # Compare branch differences
git tag <name> <commit>      # Give commit an alias
```

### Reversing changes
Detailed use cases can be found in the documentation for [git-reset](https://git-scm.com/docs/git-reset#Examples). For publicly-visible branch, use `revert` instead of `reset` to avoid forcing needless merges on other developers to clean up the history. This [page](https://blog.github.com/2015-06-08-how-to-undo-almost-anything-with-git/) is AMAZING!
```bash
git reset --soft <commit>     # Only reset head to <commit>
git reset --mixed <commit>    # Reset index but not working tree (default)
git reset --hard <commit>     # Reset index and working tree
git revert <commit>           # Reset index AND record revert as commit
git checkout <commit> <file>  # Revert file to that in previous commit
git commit --amend            # Shortcut to modify latest commit
```

### Synchronizing changes
```bash
git clone <repo>                 # Clone repository
git fetch <repo>                 # Retrieve all history of repository
git merge <repo>/<branch>        # Combine with specific branch of repository
git push <alias> <branch>        # Upload all local branch commits
git pull                         # git-fetch + git-merge
git request-pull <start> <repo>  # Generate pull request
```

### Shelving intermediate work
In the case when workflow is interrupted by an urgent fix request, but files in working tree are not ready to be committed yet.
```bash
git stash       # Temporarily stores working tree
git stash pop   # Restores most recently stashed files
git stash drop  # Discard most recent stashed working tree
git stash list  # List all stashed working trees
```

## Getting Started
1. Read the Git documentation: https://git-scm.com/docs
2. Fiddle around with a Git GUI (GitHub Desktop is a great introduction, followed by SourceTree upon familiarity with git revision control).
3. Set up a `.gitignore` file ([here's a guide](https://www.gitignore.io/)) and `README.md` file ([here's another](https://gist.github.com/PurpleBooth/109311bb0361f32d87a2)) in repository.
4. More resources include [everyday git commands](https://git-scm.com/docs/giteveryday), recommended git [workflows](https://git-scm.com/docs/gitworkflows).
5. Optional reading on git [architecture](https://git-scm.com/docs/gittutorial-2), [nomenclature](https://git-scm.com/docs/gitglossary), and [core low-level commands](https://git-scm.com/docs/gitcore-tutorial).
6. Reading on SSH and GPG keys to sign your git commits: [a RyanLue blogpost](https://ryanlue.com/posts/2017-06-29-gpg-for-ssh-auth).

## References:
* https://tutorialzine.com/2016/06/learn-git-in-30-minutes
* https://git-scm.com/docs
* https://services.github.com/on-demand/downloads/github-git-cheat-sheet.pdf
* Stackoverflow
