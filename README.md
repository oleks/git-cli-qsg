# The Git Command-Line Interface—A Quick Start Guide

Git is a system for the distributed revision control of a hierarchy of
files. Git allows people to simultaneously work on local copies of the
file hierarchy, and provides tools to synchronise the copies later.

The philosophy of Git is that a project should, at each end of the
line, evolve in small, incremental steps, each step justified by a
handful of words—a commit message. Sporadic, large, and unjustified
changes, do not look good in Git, and are frowned upon.

Participants should synchronize frequently. A synchronization, or a
"merge", is an interleaved application of multiple sequences of
changes. Choosing this interleaving is itself a considerable change,
which again, should be small, incremental, and justifiable by a
handful of words—a commit message.

Overall, Git enables you to develop your project in such a way that
you can always reliably trace back the changes that led to the current
state of the project, and trace the reasoning behind those changes.
Using Git however, requires some forbearance.

## Most Suitable File Types

For incidental, historical reasons, Git works best with hierarchies of
text-based files, suitable for line-by-line comparison. That is, files
where (1) bytes signify characters, and (2) a line break character
occurs at least every 70-80 characters or so.

Such files (e.g., source code files) are often structured in semantic
blocks, with blocks delimited by designated symbols (e.g., `{}`),
suitably indented, or separated by a systematic number of line breaks.
Git makes no attempt at understanding these semantic blocks, and
sticks to comparing files line-by-line. This is a pity.

Overall, Git is ill-suited for many types of files (e.g., binary
files, PDFs, images, videos). This does not mean that you cannot
manage them using Git, but their revision control will not be
efficient: The power of Git comes from the capability to compute the
difference between file revisions. Many file-types have no inherent
support for doing this. However, even for those that do, Git makes no
attempt at supporting them. Git sticks to good support for text-based
files, suitable for line-by-line comparison. This too is a pity.

## In Comparison to Other Options

Git provides you with many of the features of a popular distributed
revision control service (e.g., Dropbox). Storing ill-supported files
in Git, is not necessarily less efficient than storing them in such
services, in terms of space. The Git workflow however, is more
explicit, where it is your responsibility to initiate synchronization,
and to handle synchronization conflicts when they arise. This has the
added-in benefit, that with due patience, no messy synchronizations
take place.

## GitHub

Git emerged as a distributed source-code revision control system for
managing the development of the Linux kernel. However, Git first grew
into a household name, once granted an intuitive, web-based project
hosting service, called GitHub. Hosting on GitHub is free for
open-source projects.

## The Rest of This Guide

Since Git emerged in the Linux community, it has a good command-line
interface. Professionals today primarily use Git from the
command-line.

## Getting a Repository

In Git parlance, a hierarchy of files, maintained as a project in Git,
is called a "repository". A repository is first and foremost a
directory. If you are joining an existing project, the first thing to
do is to "clone" the repository from some host to your own machine.

To clone a Git repository you are best off with its Git URL (there are
also HTTP URLs, but these are considered clumsy). If you are familiar
with SSH and `scp` (secure copy), you might recognize the Git URL
format. For instance, the Git URL of this repository, as hosted on
GitHub, is `git@github.com:oleks/git-cli-qsg.git`. This resemblance is
not incidental, Git uses good old `scp` behind the scenes.

To clone this repository, go to a suitable parent directory, and
execute the following command:

```
$ git clone git@github.com:oleks/git-cli-qsg.git
```

If instead you would like to start a new repository from scratch:

```
$ git init git-cli-qsg
```

In either case, a directory `git-cli-qsg` will be created, although in
the latter case, you are free to choose another name! A directory
containing a Git repository will always have a `.git` subdirectory.
This subdirectory contains essential metadata that makes the directory
a Git repository.

Before you continue, go on inside the repository directory:

```
$ cd git-cli-qsg
```

## Checking Status

Once you have a local clone of the repository, it is common to ponder
over the state of the local clone. It is good solid advice to always
check the state of your Git repository before making any changes.
Days, weeks, and years might have gone by, and you may have
uncommitted changes lying around. As you go in to make a fresh change,
it is best left uncluttered by old work.

`git status` gives you a complete overview of the state of your clone.

If you have cloned an existing repository, and have made no changes,
your status will look something like this:

```
$ git status
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean
```

If instead you created a fresh repository, your status will look like
this:

```
$ git status
On branch master

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

As you make changes to a large repository, and don't commit
sufficiently often along the way, the status of the entire repository
can become overwhelming. Arguably, you should have committed along the
way, but if you didn't yet know better, it is good to know that `git
status` _takes a path as an optional argument_. This way, you can ask
for the status of a particular directory, not the entire Git
repository, and commit changes one directory at a time:

```
$ git status .
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean
```

## Tracking Files

As you make changes to the hierarchy of files, this will become
visible in subsequent status requests:

```
$ touch hello-git.txt
$ git status .
On branch master
Your branch is up to date with 'origin/master'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	hello-git.txt

nothing added to commit but untracked files present (use "git add" to track)
```

Git does not automatically track all the files in the given directory.
If Git is not tracking the file, you will not be able to track changes
to it, and synchronize it with your peers.

It is important to be vary of what you track in Git, and what you
don't. Forgetting to track certain files in Git, can render your work
incomplete for others. Tracking unnecessary files clutters the
repository, and may unnecessarily expose personal data.

If you want to track a file, you should add it to Git:

```
$ git add hello-git.txt
$ git status
On branch master
...

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   hello-git.txt

```

If instead you don't want to track a file in Git, you should add the
path to it to a special file called `.gitignore`:

```
$ echo hello-git.txt >> .gitignore
$ ls -a
.  ..  .git  .gitignore  hello-git.txt
$ git status .
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	.gitignore

nothing added to commit but untracked files present (use "git add" to track)
```

The `.gitignore` should be added to Git, so that your collaborators
too can have each their own, private `hello-git.txt`:

```
$ git add .gitignore
$ git status .
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   .gitignore
```

distinguish between untracked files, and uncommitted changes.


