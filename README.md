# GitTutorialRemote
A quick guide to Git and GitHub.

## GLOSSARY
Repository: A collection of refs together with an object database containing all objects which are reachable from the refs,




Reference: https://tutorialzine.com/2016/06/learn-git-in-30-minutes
## First-time setup
```bash
# Writes to global ~/.gitconfig file rather than .git/config
# the email and name to be recorded in any newly created commits.
# ref: https://git-scm.com/docs/git-config#FILES
git config --global user.name <username>
git config --global user.email <email>

#
```

```
# Writes to global ~/.gitconfig file rather than .git/config
# the email and name to be recorded in any newly created commits.
# ref: https://git-scm.com/docs/git-config#FILES
git config --global user.name <username>
git config --global user.email <email>

#
```


In project folder:
git init     # create empty repo
git status   # identify untracked files, commits

Staging process:
git add <file>
git add -A   # Adds all files in directory
git commit -m "Commit message"

# Note that files exist in ".git" subdirectory

Q. How does GitHub work to store data files offline?
# https://guides.github.com/introduction/git-handbook/

Git is a distributed version control system (DVCS)
that synchronizes all changes transparently and asynchronously.

Github is a Git hosting repo providing additional tools like issues,
pull requests, code review.

# switch branches
git checkout <branch>

# push changes
git push --set-upstream origin <branch>

# push exisiting repo to new remote
# must be something you have access to
git remote add origin https://github.com/pyuxiang/GitTutorialRemote.git
git push -u origin master
