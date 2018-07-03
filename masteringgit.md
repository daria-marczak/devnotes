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