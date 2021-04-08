# Git Merge, Rebase, Revert, Log

Merge and Rebase are two options for incorporating changes.
Both have their goods and bads.

## Merge

Merge master into feature-branch

```
git checkout feature-branch
git merge master
```

or, one liner:
```
git merge feature-branch master
```

> Ofcourse, you have to fetch the latest master (remote) first before merging and rebasing.

Merge is non-destructive operation i.e. the existing branches are not changes on any way.
However, it creates a new commit ("merge commit") when two branches are merged. This can pollute the 
history. 

## Rebase

Rebase feature-branch branch into master. 

```
git checkout feature-branch
git rebase master
```

### Interactive Rebasing

```
git checkout feature-branch
git rebase -i master
```

This will open a text editor listing all of the commits that are about to be moved:
```
pick 22asd12 Commit message 1 #1
pick 46asd21 Commit message 1 #2
pick 8sd34s1 Commit message 1 #3
```

By changing the pick command and/or re-ordering the entries, you can make the branch’s history look like whatever you want. For example, if the 2nd commit fixes a small problem in the 1st commit, you can condense them into a single commit with the fixup command:
```
pick 22asd12 Commit message 1 #1
fixup 46asd21 Commit message 1 #2
pick 8sd34s1 Commit message 1 #3
```

### The Golden Rule of Rebasing

> **"NEVER USE REBASE ON PUBLIC BRANCHES"**
>
> In other words, never rebase master onto your branch.

The rebase moves all of the commits in master onto the tip of your feature-branch. 
The commit hash from the master branch is replaced with new ones.
The problem is that it happened only in your repository while other team members are still working with the original master.

### Resolve rebase conflict

When you encounter a conflict, the git will also tell you the steps to resolve it which is:
1. Resolve the conflict
2. Add/Remove changes
3. Continue rebase

```
#git add/rm files or 
git add . 
git rebase --continue
```

If you want to abort the rebase:
```
git rebase --abort
```

After rebasing, when you try to push your feature-branch, you might get error like this:

> git push origin feature-branch
> To https://github.com/NibeshSdev/git-rebase-test.git
>  ! [rejected]        feature-branch -> feature-branch (non-fast-forward)
> error: failed to push some refs to 'https://github.com/NibeshSdev/git-rebase-test.git'
> hint: Updates were rejected because the tip of your current branch is behind
> hint: its remote counterpart. Integrate the remote changes (e.g.
> hint: 'git pull ...') before pushing again.
> hint: See the 'Note about fast-forwards' in 'git push --help' for details. 

In this case, you need to force push your changes with:
```
git push -f origin feature-branch
```

If you don't want to force push, then you can checkout to a new branch and push and merge that new branch. After that,
you can delete your old feature-branch.
```
git checkout -b feature-branch-clone
git push feature-branch-clone
git branch -d feature-branch
```
Sometimes when you pull the branch in the above error scenario (as hinted in the error message), it may cause duplication commits due to merging. Because your local rebased feature-branch get merged with your remote feature-branch (which is not rebased).
Then you will have same commits with different hash and the commit history looks confusing and messy. 
If you face that situation, you can revert back.

## Revert

### Temporarily switch to a different commit

If you want to go to that HEAD and come back with no desire to make changes and commits:
```
git checkout 22asd12
```

If you want to make changes and commits:
```
git checkout -b new-branch-name 22asd12
```

### Undo unpublished commits

If you want to reset everything you have done after that commit, then
```
git reset --hard 22asd12
```
It will wipe out all the uncommited local changes as well.
If you have uncommitted work that you want to keep, don't do it.

### Undo unpublished commits, but keeping the uncommited work
```
git stash
git reset --hard 22asd12
git stash pop
```
> It saves the uncommitted work, reset to the HEAD and then re-applies that patch.
> You might get merge conflicts.

### Undo published commits

You need to revert the commits.
```
# It will create three separate revert commits:
git revert a867b4af 25eee4ca 0766c053

# It will revert the last two commits:
git revert HEAD~2..HEAD

# It will revert a range of commits using commit hashes:
git revert 0d1d7fc..a867b4a
```

### Revert a merge commit

If you want to revert a merge commit:
```
git revert -m 1 22asd12
```

## Git Log
See your project history i.e. commit history:
```
git log
```

### Oneline commit message
To get each commit in oneline, use --oneline flag
```
git log --oneline
git log --decorate
```

### Decorate the commit message
Decorate the commit with branch or tag associated with the commit. It can be used with onelineflag.
```
git log --decorate
git log --oneline --decorate
```

### Display the changes in each commit:
```
git log -p
```

### Display the stats for each commit:
```
git log --stat
```

### Get the last N (eg. five) commits:
```
git log -5
```

### Get commits from specific date:
```
git log --after="2021-4-5" --before="2021-4-8"

git log --after="yesterday"
```
You can also use --since and --until flags, but they are similar to after and before flag.

### By Author
```
git log --author="Nibesh"

git log --author="Nibesh\|Harry"
```

### By Commit Message
```
git log --grep="Bugfix-124"
```

### By File
```
git log -- index.html home.js"
```

### By Content
```
git log -S"Home Page"

# want to use regular expression
git log -G"Home"
```

### Filtering Merge Commit
```
# exclude the merge commits
git log --no-merges

#only want to see the merge commits
git log --merges
```


### Additional Links
[Merging vs Rebasing](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)
[git-scm.com section git-revert](https://git-scm.com/book/en/v2/Git-Tools-Advanced-Merging#_undoing_merges)
[stackoverflow - move head to previous loaction](https://stackoverflow.com/questions/34519665/how-can-i-move-head-back-to-a-previous-location-detached-head-undo-commits/34519716#34519716)
[git-revert mainpage](http://schacon.github.io/git/git-revert.html)