# Github Commands

## clone a repository

```
git clone [insert url here]
```

## adding to a repository

Adds a file as it is now to your next commit
```
git add [file]
```

Unstage a file keeping changes in working directory
```
git reset [file]
```

Commit your staged content as a commit snapshot
```
git commit -m "[message]"
```

## Branches and merging

List branches
```
git branch
```

Create a new branch at the current commit
```
git branch [branch-name]
```

Switch to another branch and check it out into your directory
```
git checkout
```

Merge the specified branch's history into current branch
```
git merge [branch]
```

Show all commits in current branch's history
```
git log
```

## Share and Update

Add a git URL as an alias
```
git remote add [alias] [url]
```

Merge a remote branch into your current branch to bring it up to date
```
git merge [alias]/[branch]
```

Transmit local branch commits to remote repository branch
```
git push [alias] [branch]
```

Fetch and merge any commits from the tracking remote branch
```
git pull
```

## Temporary Commits

Save modified and staged changes
```
git stash
```

List of stack-ordered stashed file changes
```
git stash list
```

Write working from top of stash stack
```
git stash pop
```

Discard changes from top of stash stack
```
git stash drop
```
