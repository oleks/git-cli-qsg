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

As you can see, `hello-git.txt` no longer shows up among the untracked
files; it is ignored.

The `.gitignore` file however, should be added to Git, so that your
collaborators too can have each their own, private `hello-git.txt`:

```
$ git add .gitignore
$ git status .
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   .gitignore
```

## Committing Changes

Once you have some changes to be committed, you can commit quickly:

```
$ git commit -m 'Initial commit'
```

Although `-m` reads like "message" (i.e., commit message), you should
really think of it like a subject-line (e.g., e-mail subject line). It
should be very short and to the point. Shorter than a Twitter message,
preferably 50 characters or less.

There is some additional culture to this subject line: It should begin
with a capital letter, and should not end with a dot. It should say
what the commit will do if applied. As you write the subject line,
think "If applied, this commit will ..."

For example, `Fix bug #42`, `Correct a type-o`, `Add a guide on
writing good commit messages`. As an exception to this rule, it is
customary for the first commit to have the subject line `Initial
commit`.

Sometimes, 50 characters doesn't cut it. You should still come up with
a 50-character subject line, but you can provide additional details
below the subject line in a longer commit message. To let you write
the longer commit message, Git will direct you to a text-editor (by
default, `vim`, but see below):

```
$ git commit
(opens editor)
```

This will open up a text-editor where in the top line you can provide
your 50-character subject line, and below, separated by an extra line
break, provide more details about your commit.

The commit will take place as soon as you save and close the text
editor (in `vim`, `:wq` or `:x`). The commit will abort if you exit
without saving (in `vim`, `:q`).

For more on how to write good commit messages see [this
guide](https://chris.beams.io/posts/git-commit/).

### Amending Commits

It is common to make mistakes as you commit. Git is more than willing
to amend your commits. This is best done _before_ you synchronize with
your peers, otherwise you'd have to synchronize already synchronize
commits; not a very pleasant experience.

To amend the previous commit, add the changes you have, or simply
issue the following command if you merely wish to amend the commit
message:

```
$ git commit --amend
```

This will open up a text editor, as with mere `git commit`.

### Changing the Default Text-Editor

Depending on your text-editor of choice, issue one of the following
commands (add your favourite text-editor below).

#### Emacs

```
$ git config --global core.editor emacs
```

#### Vim

```
$ git config --global core.editor vim
```

#### Atom

```
$ git config --global core.editor "atom --wait"
```

#### Sublime

```
$ git config --global core.editor "subl -n -w"
```

### Signing Your Commits

Git and GitHub offer little security out of the box. Your SSH-key must
match to upload and download changes from GitHub, but no guarantee is
provided out of the box that the changes you upload are done by _you_.

It is therefore good practice to sign your commits with your GPG key.

To set this up:

```
$ git config --global commit.gpgsign true
$ git config --global user.signingkey <keyid>
```

If you also submit your public GPG key to GitHub, a nice green
"VERIFIED" icon will appear alongside your commits.

## Making Changes

As you make changes to the files that Git is tracking, `git status`
will let you know when there are files that have changed since last
commit. 

For instance, writing `hello-git.txt` to `.gitignore` might have been
a bit too specific, and we would really like to ignore _all_ `.txt`
files. Using your favourite text editor, you can change
`hello-git.txt` in `.gitignore` to `*.txt`. `.gitignore` supports a
[range of pattern
formats](https://git-scm.com/docs/gitignore#_pattern_format).

Alternatively, if you added `hello-git.txt`, write something to that
file to induce a change.

```
$ git status .
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   .gitignore

no changes added to commit (use "git add" and/or "git commit -a")
```

Although you _can_ add the entire file to a commit, it is a common
misconception that all your changes to a file are best justified by
one commit message. Besides, it can be good to refresh your memory on
what you've done!

It is therefore **strongly recommended** that you selectively add your
changes to a file in preparation for a commit:

```
$ git add -p .gitignore 
diff --git a/.gitignore b/.gitignore
index f07e6d3..2211df6 100644
--- a/.gitignore
+++ b/.gitignore
@@ -1 +1 @@
-hello-git.txt
+*.txt
Stage this hunk [y,n,q,a,d,/,e,?]?
```

`git add -p` will go through its best estimate of "one change at a
time", and for each change ask if you want to "stage this hunk" to
committed.

The options you should memorise are `y` (yes), `n` (no), `q` (quit),
`s` (split), `e` (edit), and `?` (help). `s` lets you ask Git to split
a hunk if it is too big (best split over multiple commits). This does
not always work, and this is when `e` comes in handy. `e` will open
the "hunk" in a text editor, where you can manually change the change.

Once you've added all the changes you want, commit, and iterate until
no changes remain.

### Resetting

If you make a mistake while adding changes (e.g., accidentally pressed
`y` instead of `n`), you can "reset" your additions for a file:

```
$ git reset .gitignore
```

Reset will also reset the tracking status of a file, if the file
wasn't previously tracked.

## Undo/Change Last Commit

Sometimes, you not only add something by mistake, but also commit
something by mistake. To undo the last commit:

```
$ git reset HEAD~
```

This will undo the commit, and reset everything you added before. You
can proceed to `git add -p` as necessary, before you commit again.

To commit with the original message as a starting point in a text-editor:

```
git commit -c ORIG_HEAD
```

## Command Glossary

  * `git status <path>` — check the state of the repo at `<path>`
  * `git add <path>` — add the entire `<path>` to next commit
  * `git add -p <path>` — selectively add changes at `<path>` to next
    commit
  * `git reset <path>` — reset addition of `<path>` to next commit
  * `git commit -m <subject-line>` — commit with a quick short
    message
  * `git commit` — commit with a longer message (opens text-editor)
  * `git commit --amend` — amend the previous commit (opens
    text-editor, in case you also want to amend the message)
