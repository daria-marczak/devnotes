# Mastering Git
A git project stores information in 4 areas:
+ Stash
+ Working area
+ Index (place where you put the files before the commit, at the ``git add`` moment)
+ Repository

## Index
It stands between the working area and repository. Data goes through the index to repository. It's actually called staging area. 
The file index in the directory is a binary file. When we have committed all the files from our working area and checked through ``git status``, the message we receive will be "Your branch is up-to-date with origin/master", but this does not mean that the index is empty. It means that index contains exactly the same files as in the working area.

---

When we want to compare the index with the repository, we use ``git diff --cached``. ``git-diff`` will only compare the working area with the index.

If we try to ``git rm fileName`` it will try to remove it both from working area as well as index. If we want to remove only from the index, we should use ``git rm  --cached fileName``.

In order to rename a file in working area we use ``mv previousName.extension wantedName.extenion`` but git will then not know anything about it, it will be unstaged. But the file in the index will stay the same. We can copy the changes:

```
git add wantedName.extension
git status (will show that there is no file of such name in the index)
git add previousName.extension 
```

It will actually remove the file from the index - it's taking the free space of nonexsitent file and replace it. At ``git status`` it will then inform that the name is changed.

When we want to compare the index with the repository, we use ``git diff --cached``. ``git-diff`` will only compare the working area with the index.

If we try to ``git rm fileName`` it will try to remove it both from working area as well as index. If we want to remove only from the index, we should use ``git rm  --cached fileName``.

In order to rename a file in working area we use ``mv previousName.extension wantedName.extenion`` but git will then not know anything about it, it will be unstaged. But the file in the index will stay the same. We can copy the changes:

```
git add wantedName.extension
git status (will show that there is no file of such name in the index)
git add previousName.extension 
```

It will actually remove the file from the index - it's taking the free space of nonexsitent file and replace it. At ``git status`` it will then inform that the name is changed.

## Git reset
It does different things in different contexts. It can move a branch to look at the specific commit. The head is following along the current branch.

``reset --hard`` will copy the data to both the working area and the index. With ``--mixed`` reset moves from the new current commit to the index but not to working area. This is actually a default option. ``--soft`` don't touch any other areas, just move the branch.

If we have commited two files and we actually don't want them to be in the repository, we want to make it as if the last 2 commits never happened. We can use ``git reset --hard SHA1`` where the SHA1 is actually the commit from before those new ones. Git will then move the head with the branch to the new current commit: the one before the changes. It will also change the changed files to both index and working area. The commits that have no branch pointing at them will soon be garbage collected.

If we want to remove a staged file, as git suggests, we can use the command ``git reset HEAD <fileName>`` to unstage. It's actually a ``git reset mixed`` as this is a default reset. 

## Stash
When we are working on a file and then we need to work on another before actually finishing the first job, we can use ``git stash`` so that our staged files do not confuse the other work. ``git stash --include-untracked`` will, as said, include the untracted files in the stash. Then we will be in line with the current commit. We can read the content of stash with ``git stash list``. To move the data from the stash to the working area and the index using ``git stash apply``. Then we can stage these updates and commit it all. After we clear the stash using ``git stash clear``. 

## Solving conflicts
If we modify a file from two different branches, we have a conflict. When we try to merge one from there to the master, git will inform that both branches have modified the same file. It has changed the file in the working area so that we can see if we want to overwrite it. When we go to the file, we will see which branch has what data. We can then manually edit the file and decide what the merged file should look like. Now that we have solve the conflict, we need to inform git that we have solved it. So we ``git add fileName`` because we have edited it. All we then have to do is to commit the data. 

## Working paths
If we have a file that we want to reset to its previous version and try to use ``git reset --hard HEAD fileName``, git will tell us that it cannot do hard reset with paths. The most common way to do this is using checkout: ``git checkout HEAD fileName``. It's one of the most distructive commands in git.