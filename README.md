# READ ME

Merge and Rebase are two options for incorporating changes.
Both have their goods and bads.
This is read me from feature 1 branch.

Don't voilate the GOlden Rule of Rebasing.

## Rebase Options

Rebase feature branch into master. 

```
git checkout feature
git rebase master
```

## Interactive Rebasing

```
git checkout feature
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