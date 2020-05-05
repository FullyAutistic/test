Auther:     Luc van den Enden
Date:       10/04/2020

# The git command line!
First, the basics...

**Is git installed?**
> $ git --version

**Set your configuration!**
*Username and email are required.*
> $ git config --global user.name "Luc van den Enden"
> $ git config --global user.email "luc.vandenenden@gmail.com"
> $ git config --list

**Need help?**
*git help <verb>*
*git <verb> --help*
> $ git help config
> $ git config --help

# Good, now lets go!

First off, go to the folder containing the code you want to add to a repository and execute the git init command:
> $ git init

Executing the init command twice in a row will just reinitialize the git repository.

This will create a .git folder within your directory. To verify that it was created, execute ls -la:
> $ ls -la

## Before we commit...

Before you can commit anything to a repository, you have to check the status first:
> $ git status

The output for this directory will look as follows:
> On branch master
>
> No commits yet
>
> Untracked files:
>  (use "git add <file>..." to include in what will be committed)
>
>	.private
>	test.py
>
> nothing added to commit but untracked files present (use "git add" to track)

As we can see there is a hidden file called private inside this directory. What if we dont want to commit this to our repository? Then we have to ignore this file. To do so we need to add all the file names into that gitignore file:
> $ touch .gitignore

Once the ignore file is created, you can add any filename into the ignore file. This also works with special characters. For example, adding \*.py will make it so all files with that file extension are ignored.

To verify that the ignore file was setup correctly, rerun the git status command. The new output should look like this:
> On branch master
>
> No commits yet
>
> Untracked files:
>  (use "git add <file>..." to include in what will be > committed)
>
> 	.gitignore
> 	test.py
>
> nothing added to commit but untracked files present (use "git add" to track)

As you can see, there is no more .private file, only a gitignore file. This means that the files specified inside the gitignore file will NOT be committed to the repository.

## Where are we now?

There are three stages to git. They are, in order, as follows:

1. Working directory
2. Staging area
3. Git repository

### The working directory

The working directory contains the untracked and modified files. This is also what is listed when we use git status. Everything colored red in the output of git status is currently in the working directory.

### The staging area

The staging area is where we want to organize the files that we want to commit to our repository. This makes it so you can make more detailed commits. For example, you don't want to commit a large file with the comment: i made a lot of changes. You always want to make detailed commits.

To add a single file to the staging area, you run the following command:
> $ git add <file>

To add all files to the staging area:
> $ git add -A

To remove a singe file from the staging area, run the git reset command:
> $ git reset <file>

To remove all files from the staging area, you simply run git reset without any arguments:
> $ git reset

Files within the staging area will be colored green when we run git status. After adding all the files within our working directory, the output of git status will look like this:

> On branch master
>
> No commits yet
>
> Changes to be committed:
>   (use "git rm --cached <file>..." to unstage)
>
> 	new file:   .gitignore
> 	new file:   test.py

### The git repository

Now where ready to actually commit files to our repository. To do so, use the following command:
> $ git commit -m "<message>"

The message part is very important. This is where you provide the details about the changes you've made to the files you are committing. Because this is the first time committing to a repository, we will just parse Initial commit" as our message:
> $ git commit -m "Initial commit."

The output will look like this:
> [master (root-commit) 887b0db] Initial commit.
> 2 files changed, 119 insertions(+)
> create mode 100644 .gitignore
> create mode 100644 test.py

Now if we run git status the output will look like this:
> On branch master
> nothing to commit, working tree clean

We can also use git log. This command will show the logs of this git repository:
> $ git log
> commit <hash of the commit, will be unique every time>
> Author: Luc van den Enden <luckert1998@gmail.com>
> Date:   Mon Apr 27 17:27:55 2020 +0200
>
>    Initial commit.

The bottom row represents the message corresponding to that commit.

# Remote Repository's

To clone a remote repository, u can use one of the following commands:
> $ git clone <url> <where to clone to>

You can use relative paths as an argument for where to clone to. So for example, cloning to . will clone to the current working directory.

Lets copy a test repository into a folder called cloned_repo:
> $ git clone https://github.com/FullyAutistic/test.git <current directory>/cloned_repo

Now lets take a look at our newly cloned repository:
> $ cd cloned_repo/
> $ ls -la

The output of ls -la on this particular repository will look like this:
> total 20
> drwxr-xr-x 3 root root 4096 Apr 27 18:49 .
> drwxr-xr-x 4 luc  luc  4096 Apr 27 18:49 ..
> -rw-r--r-- 1 root root   13 Apr 27 18:49 file.txt
> drwxr-xr-x 8 root root 4096 Apr 27 18:49 .git
> -rw-r--r-- 1 root root   25 Apr 27 18:49 README.md

## Viewing information about remote repository's

This one is not so exciting. The following command shows information about the set of tracked repository's:
> $ git remote -v

The output for this demonstrations should looks as follows:
> origin	https://github.com/FullyAutistic/test.git (fetch)
> origin	https://github.com/FullyAutistic/test.git (push)

We can also use the following command to list both remote tracking branches and local branches:
> $ git branch -a

We will get into what branches are shortly.

## Pushing changes

Before we actually push any changes, lets make some changes and use the git diff command to see what kind of changes we've made. The change to make is to append the text "new line" to the end of the file.txt in our cloned repository, and remove the line "Henk is cool":
> $ git diff

The output should look like this:
> diff --git a/file.txt b/file.txt
> index f7d8ba7..5e6f879 100644
> --- a/file.txt
> +++ b/file.txt
> @@ -1 +1,2 @@
> -Henk is cool
> +new line

In this case, we only edited one file named file.txt as we can see in the first row. Git will create an A and B version. The changes are indicated via a + or a - sign at beginning of a line. The colour of the output text will aslo apear either red or green depending on if the line was removed or added.

### Actually pushing changes now...

Remember that we've talked about the dirrent area's files can be in. Right now we are stil inside the working directory, so in order to push a change we need to add a file to the staging area:
> $ git add file.txt

Now lets check the status:
> $ git status

Which should look like this:
> On branch master
> Your branch is up to date with 'origin/master'.
>
> Changes to be committed:
>   (use "git reset HEAD <file>..." to unstage)
>
>	modified:   file.txt

Now we can commit our change using the commit command:
> $ git commit -m "Changed file.txt"

### Okay, now for real...

I know, were getting there. First I need to explain the pull command:
> $ git pull origin master

Since there are probably other people working on files inside the remote repository, we first need to get those changes, and then push our changes using the following command:
> $ git push origin master

Of which the output should look something like this:
> Username for 'https://github.com': <your username>
> Password for 'https://FullyAutistic@github.com': <your password>
> Enumerating objects: 5, done.
> Counting objects: 100% (5/5), done.
> Delta compression using up to 8 threads
> Compressing objects: 100% (2/2), done.
> Writing objects: 100% (3/3), 285 bytes | 285.00 KiB/s, done.
> Total 3 (delta 0), reused 0 (delta 0)
> To https://github.com/FullyAutistic/test.git
>   81d967b..8504508  master -> master

# Common workflow

WERK DIT STUKJE NOG UIT IN DIT DOCUMENT!!!!!!!
https://www.youtube.com/watch?v=HVsySz-h9r4&t=1242
