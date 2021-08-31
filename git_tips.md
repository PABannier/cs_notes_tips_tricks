# Git commands

This page is certainly not an exhaustive list of all the things you can do with 
Git. It is merely a personal record of the Git commands I know how to use and might
use on a regular basis.

## Commands

- `git checkout <BRANCH_NAME>`: change to the branch.

- `git checkout -b <BRANCH_NAME>`: create a new branch.  

- `git stash`: saves the changes in a directory. Useful to pull from a branch and then merge with your own changes.

- `git stash pop`: makes the stashed changes reappear.

- `git merge <BRANCH_NAME>`: merge the specified branch with the current branch.

- `git fetch`: fetch the latest branches of a repo.

- upstream: a usual term used to indicate the "mother" repository (non-forked).

- `git remote add <URL>`: add a fork to your repo (useful to work on someone else's PR).

- `git revert <COMMIT_ID>`: to revert changes and switch back to an old specified commit.

- `git log`: shows commit logs.

- `git status`: the current status of what you commit.

- `git diff`: to see the differences between two commits.

- `git remote -v`: to see all current remote repos.

- `git branch -a`: to see all branches (local and remote)

## Misc

- Sometimes, you might want to create new branches without opening a PR (useful for experiments for a paper or a tutorial progress).

- Central idea with Git: have one folder in your local disk where you store branches, forks...
When working on someone else's PR, just add remote its fork and checkout to its branch.
