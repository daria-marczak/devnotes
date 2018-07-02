# Notes from How git works course from Paolo Perrotta

---

The basic idea in git is a simple map: it maps keys to values. It's persistent: stored on the disc. Values are the sequences of bytes that has a calculated key with SHA1hash. 

`echo "Apple Pie" | git hash-object --stdin` command is going to tell git that this is a content, not to make git search for a file "Apple Pie". The output is: `23991897e13e47ed0adb91a0082c31c82fe0cbe5`

SHA1s are unique.

---

## Storing things

For the "Apple pie" to be persistent, we can use `-w` flag. It stands for "write". This piece of content needs a repository.
We can now save this objects, and in the .git/objects we will find it if it's save. The name of the directory will be "23", standing for the first two digits of the SHA1. Inside there will be a file with the remained SHA1 digits. The original string will be stored in this file. This is what git calls a **blob** of data. It's a generic piece of content. We cannot open it through the directory, but with the command `git cat-file`. We should run it this way:

```
git cat-file 23991897e13e47ed0adb91a0082c31c82fe0cbe -t
```

Where -t flag stand for the type.It will tell us that it's a blob. The `-p` command (standing for pretty printing) will unzip the object and tell us what the actual string is in this file. 

Using `git cat` we can read the file inside the terminal.

---
## First commit
To commit a file, we need to put into the staging area (the often used git add __file name__ or `git add .`). It stores the commit in objects directory. It also has a SHA1 of the tree – the root of the directory. It's a tiny piece of text, containing a list of SHA1.

In the tree there are another blobs. A blob is just a content of the file. File name and file permissions are not stored in the blob. 

There is no magic behind the SHA1 – If I have the same content, I will have the same SHA1. It's different in terms of commit – there's different date for example.

---
## Versioning
If we change a file, in git log we will see a tree but also a parent. The commits are linked. The tree of the second commit will not be the same. To cat-file we can also use **first few digits of the SHA1** unless there are multiple SHA1 starting with the same few digits.

If an object does not change, it will still be the same blob moved to the new tree. 

`git count-objects` command will actually count how many files we have and how much space it takes.

## Annonated tags
Those are the ones that come with a message. To create it use `git tagName -a mytag -m "Tag message"`. We can then read it using the cat-file command with -p flag and call the name of the tag, like so:

```
git cat-file -p tagName
```

It also points to the what it is: a commit.

In the git object database we have:
+ Blobs
+ Trees
+ Commits
+ Annonated tags

---

## Branches
In a project we have a branch when we do a first commit, it's our default branch, master. To check out branches, type `git branch`.

Git puts the branches in the directory called refs and subdirectory heads.

If we call `cat .git/refs/heads/master` then we will get its SHA1. After the creation of a branch and calling the `git branch`, we will see that one branch is named with an asterisk. This means that this is a current branch. The information about the current branch is in the HEAD file inside the git directory. 

> HEAD is a reference to a branch.
> And branch is a reference to a commit.

Master is moving with commits and the head goes with it. If we `git checkout secondBranch`, git changes HEAD to point at the secondBranch. After the checkout, the working area changes to the content of the commit of the secondBranch.

> Checkout means move HEAD and update the working area.

If we now modify the file on the secondBranch and commit this. This will now look like this:

![Diagram](https://i.imgur.com/GoXAL6X.png)

## Merging
If we checkout to master, the HEAD will move and we will have the master's version instead of the secondBranch.

If we try to `git merge secondBranch`, we will have a conflict. When we are done fixing the conflict (i.e. searching for the middle ground in the file), we need to `git commit` so that git knows that we have fixed the conflict. After merging, it will have two parents.

![Diagram](https://i.imgur.com/jfyhOkU.png)

## Merging without merging

>> Let us assume that I created a topic branch named speedup from the current master. After working on this branch for a while (three commits, those white circles), I finally decided that I am done and then I pushed it to my own remote. Meanwhile, nothing else happened in the master branch, it remained in the same state right before I branched off. The situation is depicted in the following diagram.

>> Once the project maintainer got notified that my branch is ready to be integrated, she might use the usual steps of git fetch followed by git merge and thus, my work is landed in the source tree. Because master has not been changed since the commit (gray circle) which serves as the base for the said topic branch, Git will perform the merge using fast-forward. The whole series of the commits will be linear. The history will look like the diagram below (left side).

>> Source: [ariya.io](https://ariya.io/2013/09/fast-forward-git-merge)

![Diagram](https://ariya.io/images/2013/09/merging.png)

---
## Loosing the HEAD
One can directly checkout the commit instead of the whole branch

```
git checkout SHA1
git checkout -b <new-branch-name>
```

Then there will be no current branch, the HEAD will be detached. If we then commit something, **only** the HEAD will move with the commit, the master doesn't. If we switch to master, those commits will not be reached. There will be removed after some time from the database.

Before this happens, we can actually get those commit with their SHA1, create a branch for them that will follow up. If we then try to checkout master, the commits are safe, as the HEAD moved to the master.

#### 3 rules
- The current branch tracks new commits
- When you move to another commit, git updates your working directory
- Unreachable objects are garbage collected

## Rebase
We have two branches, each has its commits. We could simply merge them. But we will rebase the second one. ``git checkout secondBranch, git rebase master``. Git will then look in the first commit in the secondBranch, which is also a commit in the master, the parent of both branches. Git will detach the entire branch and move it on the top of the master. But the database objects are **immutable**, git cannot just detach the entire branch. The first blue commit will get a new SHA1, the other commits receive one as well so they actually become new objects thus new commits. They are new files in the directory and the old commits are actually left behind. In this case the old commits will be **almost impossible to reach**. If we don't have the SHA1 for them, git will **garbage collect unreachable objects**.
It will look like this:

![Diagram](https://i.imgur.com/NaNe1re.png)

The secondBranch has both the commits from its branch as well as the master's. There are no conflicts to be resolved.

If we want the commits from secondBranch to be on the master, we can

```
git checkout master // Master is now the current branch
git rebase secondBranch

```
It will work just like a merge. But the branches are just rearranged.

![Diagram](https://i.imgur.com/JRm80Md.png)

We have merging and rebasing because of different trades-off. 

|Merging|Rebase|
|:------|:-----|
|It preserves history exactly as it happened|Rebase history looks simple and neat. Commits are in the same timeline. |
|Less simple when you have a large project|Rebase helps refactor project history. But it actually changes the project's history.|

**When in doubt, just merge**.