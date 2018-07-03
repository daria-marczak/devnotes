# Mastering Git
A git project stores information in 4 areas:
+ Stash
+ Working area
+ Index (place where you put the files before the commit, at the ``git add`` moment)
+ Repository

## Index
It stands between the working area and repository. Data goes through the index to repository. It's actually called staging area. 
The file index in the directory is a binary file. When we have committed all the files from our working area and checked through ``git status``, the message we receive will be "Your branch is up-to-date with origin/master", but this does not mean that the index is empty. It means that index contains exactly the same files as in the working area.