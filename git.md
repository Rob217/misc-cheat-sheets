# Git cheat sheet

Many of these commands were taken from the [Atlassian git tutorials](https://www.atlassian.com/git/tutorials).

## Merging vs rebasing

https://www.atlassian.com/git/tutorials/merging-vs-rebasing

### Merge

```shell
# merge main into feature
git checkout feature
git merge main
```
This merges main into the feature branch. This is clean and keeps track of every step in the history, but can end up with a messy history.

### Rebase

```shell
git checkout feature
git rebase main
```

This rebases feature branch onto main. You are essentially rewriting the history of the commits on feature branch.
You should avoid this if the feature branch is public (e.g., is on a PR, since others might have a different commit history).

#### Interactive rebasing (squash commits)

```shell
git checkout feature
git rebase -i main  # rebase onto main
git rebase -i HEAD~3  # rebase last 3 commits
git rebase -i <commit_id>  # rebase from commit_id
git merge-base feature main  # get commit-id of first commit in feature
```

When you do this, you’ll get a list of commits, e.g., 

```
pick 33d5b7a Message for commit #1
pick 9480b3d Message for commit #2
pick 5c67e61 Message for commit #3
```

To squash commits together, replace pick with fixup:

```
pick 33d5b7a Message for commit #1
fixup 9480b3d Message for commit #2
pick 5c67e61 Message for commit #3
```

The second commit has now been merged into the first.

## Reset, checkout, & revert
https://www.atlassian.com/git/tutorials/resetting-checking-out-and-reverting

| Command      | Scope        | Common use case                                                       |
|--------------|--------------|-----------------------------------------------------------------------|
| git reset    | Commit-level | Discard commits in a private branch or throw away uncommitted changes |
| git reset    | File-level   | Unstage a file                                                        |
| git checkout | Commit-level | Switch between branches or inspect old snapshots                      |
| git checkout | File-level   | Discard changes in the working directory                              |
| git revert   | Commit-level | Undo commits in a public branch                                       |
| git revert   | File-level   | None                                                                  |

Thanks to https://www.tablesgenerator.com/markdown_tables for making the table

### Reset

```shell
git reset <commit-id/HEAD> --soft  # staged snapshot and working directory are unchanged
git reset <commit-id/HEAD> --mixed  # the staged snapshot is updated; working directory is unchanged
git reset <commit-id/HEAD> --hard  # staged snapshot AND working directory are updated
```

Like with rebase, git reset should not be used on public directories since it changes the commit history (you essentially remove the commits you’ve skipped back across).

```shell
git reset HEAD foo.py  # unstage foo.py
git reset HEAD~2 foo.py  # fetch foo.py from 2nd-to-last commit and stage it for commit
```

### Revert

```shell
git revert HEAD~2  # look at changes from 2nd-to-last commit; undo these changes; create a new commit undoing these changes; tack this new commit onto the existing project
```

Like with git merge, this is the safer option to use on public repositories.

### Checkout

```shell
git checkout HEAD~2 foo.py  # change foo.py in working directory to version from 2nd-to-last commit
```

## Clean

```shell
git clean -f  # remove local untracked files
git clean -n  # list which files will be removed
git clean -fd  # remove directories also
```

## Stash

https://www.atlassian.com/git/tutorials/saving-changes/git-stash

```shell
git stash  # save tracked files
git stash -u  # also save untracked files
git stash -a  # also save untracked AND ignore files
git stash save “this is a stash message”  # save a message with the stash
```

```shell
$ git stash list
stash@{0}: WIP on main: this is a stash message
stash@{1}: WIP on main: 5002d47 our new homepage
stash@{2}: WIP on main: 5002d47 our new homepage
```

```shell
git stash pop  # pop first stash in list
git stash pop stash@{2}  # pop specific stash
```

```shell
git stash show stash@{2}  # show stash details
git stash show stash@{2} -p  # show diff as well
```

```shell
git stash drop stash@{2}  # remove stash
git stash clear  # remove all stashes
```

## Log

https://www.atlassian.com/git/tutorials/git-log

```shell
git log --oneline  # single line per commit
git log --decorate  # include branch/tag information
git log -- foo.py bar.py  # commits for specific files
```

## Tags

```shell 
git tag -a tag_name -m "Tag message"  # add a tag
git push --tags  # push tags to remote
git tag -l  # list tags
git tag -d tag_name  # delete local tag
git push --delete origin tag_name  # delete remote tag
```
