# GitTutorialRemote
A quick guide to Git and GitHub.

## LINGO
### Git / GitHub:
Git is a distributed version control system (DVCS) that synchronizes all changes transparently and asynchronously. Github is a Git hosting repository providing additional tools like issues, pull requests, code review.
### Repository / Remote:
A repository is a collection of references together with an object database containing all objects reachable from the refs. A remote repository is a link to another version of the project hosted elsewhere. Remotes can be referenced by use of aliases using `git remote add <alias> <link>` and then subsequently using them as in `git push <alias> <branch>`.

### Branch:
A branch is an active line of development, with the most recent commit on the branch referred to as the tip. A Git repository can track multiple branches, while your working tree is associated with just one of them, pointed to by HEAD. The master branch is the default active development branch.

### Reference / Objects:
An object is a unit of storage in Git, identified by its unique object name/identifier created using the SHA-1 hash function, and which lives in the `.git/objects` object database. Objects can have types: commit, tree, tag, blob.

A reference is a path usually beginning with `.git/refs` that points to an object or another ref. A head is a named reference to the commit at the tip of a branch, while HEAD is a special symbolic ref referring to the current branch, i.e. the head of the current working tree.

### Tree:
An object containing a list of filenames and modes along with refs to associated blobs/tree objects, equivalent to a directory. Tree-ish is a tree object that can be recursively dereferenced to a tree object, including commit objects and symbolic refs to tree objects.

### Commit:
An object containing the information about a particular revision (e.g. parent commits, committer, date, message, tree object corresponding to top directory of stored revision) and represents a single point in the project history of interrelated commits. Creating a commit saves the current state of the index and advances HEAD to point at the new commit. Commit-ish is a commit object that can be recursively dereferenced to a commit object.

### Index / Working Tree:
An index is a collection of files with associated information, whose contents are stored as objects, effectively a stored version of the working tree. An index entry refers to one such file. The working tree refers to the tree of the actual checked out files, including local changes made but not committed.

### Downstream / Upstream / Origin:
Describes relative positions in repository version control. Upstream refers to the repository containing the project source code. Conversely, any information copied from upstream goes downstream. Changes are pushed upstream to distribute work to other contributors. It is not uncommon for a developer to set two remotes: upstream pointing to the project and origin pointing to their own fork of the project. This allows the developer to pull enhancements directly from the original project to stay in sync.

### Tag:
A reference under `.git/refs/tags` namespace pointing to an object of arbitrary type. Since it is not updated by the `commit` command unlike head, it is typically used to mark a particular point in the commit ancestry chain. Tag objects contain such a ref, and possibly a message as well as a PGP signature.

### (to) Push:
To get the branch head from a remote (which must necessarily be an ancestor to the local head), and put all objects reachable from the local head and missing from the remote into the remote object database and updating the remote head ref.

### (to) Fetch:
To get the branch head from a remote, and retrieve all objects missing from the local object database.

### (to) Merge:
To bring contents from another branch into the current branch, by identifying changes made since the branches diverged and applying those changes automatically. If the branch is a descendant of the current branch, the merge is a fast-forward.

### (to) Pull:
To fetch and merge.

### (to) Rebase:
To reapply a series of changes from a branch to a different base and reset the head to the result.

### (to) Resolve:
To fix manually what the failed automatic merge left behind.

### (to) Rewind:
To throw away part of development by assigning the head to an earlier revision.

## First-time setup
```bash
# UPDATE GIT CONFIGURATION
# Writes to ~/.gitconfig (global) file rather than .git/config (repository)
# the email and name to be recorded in any newly created commits.
git config --global user.name <username>
git config --global user.email <email>

# CREATE LOCAL GIT REPOSITORY
# Create new local repository in project folder
# Creates .git directory with object subdirectories, creating an initial "HEAD"
# file referencing the HEAD of the master branch.
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

# Alternatively, the flag "-A" modifies index entries to match the working tree.
git add -A

# COMMIT
# Stores current contents of index in new commit along with a log message.
git commit -m "Add README to project"

# Create new empty remote repository on GitHub
# Retrieve the HTTPS link to the remote,
# e.g. https://github.com/pyuxiang/GitTutorialRemote.git

# LINK REPOSITORY TO NEW REMOTE
# origin is the default user-defined alias to the main remote repository
git remote add origin https://github.com/pyuxiang/GitTutorialRemote.git

# PUSH CHANGES TO REMOTE
# The "-u" flag sets the upstream for subsequent argument-less git-pull.
git push -u origin master

# PULL
git pull origin
```



# switch branches
git checkout <branch>

# push changes
git push --set-upstream origin <branch>

# push exisiting repo to new remote
# must be something you have access to
git remote add origin https://github.com/pyuxiang/GitTutorialRemote.git
git push -u origin master

## References:
* https://tutorialzine.com/2016/06/learn-git-in-30-minutes
# https://git-scm.com/docs
* Stackoverflow
