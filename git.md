---
title: Git
category: Tools
tags: [Featured, WIP]
updated: 2020-12-30
weight: -5
---

Intro
-------------------------------------

Staging
-------------------------------------

### Show unstaged differences since last commit

```shell
git diff
```

#### Options

| `--staged` | includes staged changes |

### Remove all changes on file since last commit

```shell
git checkout -- <file name>
```

### Undo last commit

```shell
git reset --soft HEAD^
```

#### Options

|`--soft` | put changes into staging |
|`--hard` | erase all changes |

### Add file to last commit:

```shell
git add <file>
git commit --amend
```

#### Options

| `-m "<message>"` | overwrites the previous commit message |

### Remove untracked files

```shell
git clean -fd
```


Remote
-------------------------------------

### Show all remotes

```shell
git remote -v
```

### Add and remove a remote

```shell
git remote add <remote name> <url>
git remote remove <remote name>
```

### Set a different url to a existing remote

```shell
git remote set-url <remote name> <url>
```

### Lists all remote branches and gives information

```shell
git remote show origin
```

### Delete a remote branch:

```shell
git push <remote name> :<branch name> # Note the colon!
```

### Remove deleted remote branches from local

```shell
git remote prune <remote name>
```

### Reset local branch to remote

```shell
git reset --hard origin/branch-name
```


Tagging
-------------------------------------

### List tags

```shell
git tag
```

### Add tag

```shell
git tag -a <tag name> -m <tag message>
git push --tags
```


Rebase
-------------------------------------

Interactive with the last 3 commits, from oldest to newest:

```shell
git rebase -i HEAD~3
```

#### Commands

| `pick`   | puts commit in temporary area while rebasing. |
| `reword` | change a commit message. |
| `edit`   | this will stop the rebase and put you into staging at the point of that commit. |
| `squash` | that commit into the previous one, when rebasing change the commit message for both of the commits. |

Stashing
-------------------------------------

```shell
git stash list
git stash save <message> # save and message optional
git stash apply <stash name> # stash@{0} is default if stash name is not specified
git stash drop <stash name> # stash@{0} is default if stash name is not specified
git stash pop # does apply + drop
git stash clear # deletes all stashes
```

#### Options

| `--keep-index` | stash only **unstaged** files. |
| `--include-untracked` | stash untracked files too. |

### Create a new branch with the contents of the stash

```shell
git stash branch <new branch name> <stash>
# stash is stash@{0} for example
```


Rewriting history
-------------------------------------
{: .-one-column}

```shell
git filter-branch --tree-filter <command>

# Examples
git ... --tree-filter 'rm -f passwords.txt' -- --all # all commits in all branches
git ... --tree-filter 'find . -name "*.mp4" -exec rm{} \;' HEAD # only current branch
```

To run against each commit without checking it out first (faster):

```shell
git filter-branch --index-filter 'git rm --cached --ignore-unmatch password.txt'
```

Delete all commits without contents:

```shell
git filter-branch -f --prune-empty -- --all
```


Cherry-pick
-------------------------------------

```shell
git log # find out the commit id
git checkout <branch name>
git cherry-pick <commit id>
git cherry-pick <initial commit id>..<final commit id>
```

#### Options

- `--edit` changes commit message.
- -`n, --no-commit hash1 hash2` only stages the changes from those commits, you have to commit manually.
- `-x` adds the source branch to the commit message.


Reflog
-------------------------------------

### Deleted commit

```shell
git reflog # find the commit id
git reset --hard <commit id> # or commit name, eg HEAD@{0}
```

### Deleted branch

```shell
git log --walk-reflogs
git branch <branch name> <commit id>
```


Reverting
-------------------------------------

### Revert a single file

```shell
git checkout [commit ID] -- path/to/file
```

where `commit ID` is the version of the file you want to reverto to.

Commit the change.

