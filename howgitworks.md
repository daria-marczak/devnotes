# Notes from How git works course from Paolo Perrotta

===

The basic idea in git is a simple map: it maps keys to values. It's persistent: stored on the disc. Values are the sequences of bytes that has a calculated key with SHA1hash. 

`echo "Apple Pie" | git hash-object --stdin` command is going to tell git that this is a content, not to make git search for a file "Apple Pie". The output is: `23991897e13e47ed0adb91a0082c31c82fe0cbe5`

SHA1s are unique.

===

## Storing things

For the "Apple pie" to be persistent, we can use `-w` flag. It stands for "write". This piece of content needs a repository.
We can now save this objects, and in the .git/objects we will find it if it's save. The name of the directory will be "23", standing for the first two digits of the SHA1. Inside there will be a file with the remained SHA1 digits. The original string will be stored in this file. This is what git calls a **blob** of data. It's a generic piece of content. We cannot open it through the directory, but with the command `git cat-file`. We should run it this way:

```
git cat-file 23991897e13e47ed0adb91a0082c31c82fe0cbe -t
```

Where -t flag stand for the type.It will tell us that it's a blob. The `-p` command (standing for pretty printing) will unzip the object and tell us what the actual string is in this file. 

Using `git cat` we can read the file inside the terminal.

===
## First commit
To commit a file, we need to put into the staging area (the often used git add __file name__ or `git add .`). It stores the commit in objects directory. It also has a SHA1 of the tree – the root of the directory. It's a tiny piece of text, containing a list of SHA1.

In the tree there are another blobs. A blob is just a content of the file. File name and file permissions are not stored in the blob. 

There is no magic behind the SHA1 – If I have the same content, I will have the same SHA1. It's different in terms of commit – there's different date for example.

===
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