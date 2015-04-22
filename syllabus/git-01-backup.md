---
layout: lesson
root: ../..
title: A Better Kind of Backup
---
<div class="objectives" markdown="1">

#### Objectives
*   Explain which initialization and configuration steps are required once per machine,
    and which are required once per repository.
*   Go through the modify-add-commit cycle for single and multiple files
    and explain where information is stored at each stage.
*   Identify and use Git revision numbers.
*   Compare files with old versions of themselves.
*   Restore old versions of files.
*   Configure Git to ignore specific files,
    and explain why it is sometimes useful to do so.

</div>

We'll start by exploring how version control can be used
to keep track of what one person did and when.
Even if you aren't collaborating with other people,
version control is much better for this than this:

<div>
  <a href="http://www.phdcomics.com"><img src="img/phd101212s.gif" alt="Piled Higher and Deeper by Jorge Cham, http://www.phdcomics.com" /></a>
  <p>"Piled Higher and Deeper" by Jorge Cham, http://www.phdcomics.com</p>
</div>

###Local Repo
Git will keep track of the _changes_ to your files, rather than keep multiple copies of the files. 
It saves the first version, then keeps track of subsequent changes to that version. 
This makes it efficient and speedy. 
It can recreate any version (go back in time) by adding up all the changes 
to get to where you want to be.

### Setting Up

The first time we use Git on a new machine,
we need to configure a few things.
Here's how Dracula sets up his new laptop:

~~~
$ git config --global user.name "Vlad Dracula"
$ git config --global user.email "vlad@tran.sylvan.ia"
$ git config --global color.ui "auto"
$ git config --global core.editor "nano"
~~~
{:class="in"}

(Please use your own name and email address instead of Dracula's,
and please make sure you choose an editor that's actually on your system,
such as `notepad` on Windows.)

Git commands are written `git verb`,
where `verb` is what we actually want it to do.
In this case,
we're telling Git:

*   our name and email address,
*   to colorize output,
*   what our favorite text editor is, and
*   that we want to use these settings globally (i.e., for every project),

The four commands above only need to be run once:
the flag `--global` tells Git to use the settings for every project on this machine.

> #### Proxy
>
> In some networks you need to use a proxy. If this is the case you may also
> need to tell Git about the proxy:
>
> ~~~
> $ git config --global http.proxy proxy-url
> $ git config --global https.proxy proxy-url
> ~~~
> {:class="in"}
>
> To disable the proxy, use
>
> ~~~
> $ git config --global --unset http.proxy
> $ git config --global --unset https.proxy
> ~~~
> {:class="in"}

### Creating a Repository

Once Git is configured,
we can start using it.
Let's create a directory for our work:

~~~
$ mkdir planets
$ cd planets
~~~
{:class="in"}

and tell Git to make it a [repository](../../gloss.html#repository)&mdash;a place where
Git can store old versions of our files:

~~~
$ git init
~~~
{:class="in"}

If we use `ls` to show the directory's contents,
it appears that nothing has changed:

~~~
$ ls
~~~
{:class="in"}

But if we add the `-a` flag to show everything,
we can see that Git has created a hidden directory called `.git`:

~~~
$ ls -a
~~~
{:class="in"}
~~~
.	..	.git
~~~
{:class="out"}

Git stores information about the project in this special sub-directory.
If we ever delete it,
we will lose the project's history.

We can check that everything is set up correctly
by asking Git to tell us the status of our project:

~~~
$ git status
~~~
{:class="in"}
~~~
# On branch master
#
# Initial commit
#
nothing to commit (create/copy files and use "git add" to track)
~~~
{:class="out"}

_On the white board draw a box representing the working area and
 explain that this is where you work and make changes._


### Tracking Changes to Files

Let's create a file called `mars.txt` that contains some notes
about the Red Planet's suitability as a base.
(We'll use `nano` to edit the file;
you can use whatever editor you like.
In particular, this does not have to be the core.editor you set globally earlier.)
_also create a second file named venus.txt_

~~~
$ nano mars.txt
~~~
{:class="in"}

Type the text below into the `mars.txt` file:

~~~
Cold and dry, but everything is my favorite color
~~~
{:class="in"}

`mars.txt` now contains a single line:

~~~
$ ls
~~~
{:class="in"}
~~~
mars.txt
~~~
{:class="out"}
~~~
$ cat mars.txt
~~~
{:class="in"}
~~~
Cold and dry, but everything is my favorite color
~~~
{:class="out"}

If we check the status of our project again,
Git tells us that it's noticed the new file:

~~~
$ git status
~~~
{:class="in"}
~~~
# On branch master
#
# Initial commit
#
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#
#	mars.txt
nothing added to commit but untracked files present (use "git add" to track)
~~~
{:class="out"}

The "untracked files" message means that there's a file in the directory
that Git isn't keeping track of.

_Now, lets add files that are inside:
On the white board draw a box representing the staging area (index) and 
explain that this is where we set up the next snapshot of our project.
Like a photographer in a studio, we're putting together a shot 
before we actually snap the picture.
Connect the working area box and the staging box with 'git add'._

_`git add .` This adds __all__ the files in our repository.
But sometimes we only want to add a single file at a time._

We can tell Git that it should do so using `git add`:

~~~
$ git add mars.txt
~~~
{:class="in"}

and then check that the right thing happened:

~~~
$ git status
~~~
{:class="in"}
~~~
# On branch master
#
# Initial commit
#
# Changes to be committed:
#   (use "git rm --cached <file>..." to unstage)
#
#	new file:   mars.txt
#
~~~
{:class="out"}


_-- highlight the "Untracked files" section and that git tells you how to add a file to the next commit._

Git now knows that it's supposed to keep track of `mars.txt`,
but it hasn't yet recorded any changes for posterity as a commit.

_Tell git _"Hey, we want you to remember the way that the files look right now"_._

_On the white board draw a box representing the project history. 
Once we take a snapshot of the project that snapshot becomes a permanent reference point in the project's history that we can always go back to.
The history is like a photo album of changes, and each snapshot has a time stamp, the name of the photographer, and a description.
Connect the staging area to the history with `git commit -m "message"`.
In order to save a snapshot of the current state (revision) of the repository, we use the commit command. 
This command is always associated with a message describing the changes since the last commit and indicating their purpose. 
Git will ask you to add a commit message. 
This is just to remind you what changes you made. 
Informative commit messages will serve you well someday, so make a habit of never committing changes without at least a full sentence description._

__ADVICE: Commit often__
_In the same way that it is wise to often save a document that you are working on, so too is it wise to save numerous revisions of your code. 
More frequent commits increase the granularity of your undo button._

__ADVICE: Good commit messages__
[because it's important!](http://www.commitlogsfromlastnight.com/)
_There are no hard and fast rules, but good commits are atomic: they are the smallest change that remain meaningful. 
A good commit message usually contains a one-line description followed by a longer explanation if necessary.
For code, it's useful to commit changes that can be reviewed by someone in under an hour. 
Or it can be useful to commit changes that "go together" - for example, one paragraph of a manuscript, or each new function added to your script.
For example, if you work on your code all day long (add 200 lines of code, including 5 new functions and write 7 pages of your new manuscript including deleting an old paragraph), and at 3:00 you make a fatal error or deletion, but you didn't commit once, then you will have a hard time recreating the version you are looking for - because it doesn't exist!_


To get it to do that,
we need to run one more command:

~~~
$ git commit -m "Starting to think about Mars"
~~~
{:class="in"}
~~~
[master (root-commit) f22b25e] Starting to think about Mars
 1 file changed, 1 insertion(+)
 create mode 100644 mars.txt
~~~
{:class="out"}

When we run `git commit`,
Git takes everything we have told it to save by using `git add`
and stores a copy permanently inside the special `.git` directory.
This permanent copy is called a [revision](../../gloss.html#revision)
and its short identifier is `f22b25e`.
(Your revision may have another identifier.)

We use the `-m` flag (for "message")
to record a comment that will help us remember later on what we did and why.
If we just run `git commit` without the `-m` option,
Git will launch `nano` (or whatever other editor we configured at the start)
so that we can write a longer message.

__ADVICE:__ _You must have a commit message. It's good practice and git won't let you commit without one._

_If you only want to add one file, use `git commit filename.txt -m "message"
`git commit -am "message` will add ALL tracked files._

If we run `git status` now:

~~~
$ git status
~~~
{:class="in"}
~~~
# On branch master
nothing to commit, working directory clean
~~~
{:class="out"}

it tells us everything is up to date.
If we want to know what we've done recently,
we can ask Git to show us the project's history using `git log`. 
You can see all the changes you have ever made using this command:

~~~
$ git log
~~~
{:class="in"}
~~~
commit f22b25e3233b4645dabd0d81e651fe074bd8e73b
Author: Vlad Dracula <vlad@tran.sylvan.ia>
Date:   Thu Aug 22 09:51:46 2013 -0400

    Starting to think about Mars
~~~
{:class="out"}

`git log` lists all revisions  made to a repository in reverse chronological order.
The listing for each revision includes
the revision's full identifier
(which starts with the same characters as
the short identifier printed by the `git commit` command earlier),
the revision's author,
when it was created,
and the log message Git was given when the revision was created.

> #### Where Are My Changes?
>
> If we run `ls` at this point, we will still see just one file called `mars.txt`.
> That's because Git saves information about files' history
> in the special `.git` directory mentioned earlier
> so that our filesystem doesn't become cluttered
> (and so that we can't accidentally edit or delete an old version).

### Changing a File

Now suppose Dracula adds more information to the file.
(Again, we'll edit with `nano` and then `cat` the file to show its contents;
you may use a different editor, and don't need to `cat`.)

~~~
$ nano mars.txt
$ cat mars.txt
~~~
{:class="in"}
~~~
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
~~~
{:class="out"}

When we run `git status` now,
it tells us that a file it already knows about has been modified:

~~~
$ git status
~~~
{:class="in"}
~~~
# On branch master
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#	modified:   mars.txt
#
no changes added to commit (use "git add" and/or "git commit -a")
~~~
{:class="out"}

The last line is the key phrase:
"no changes added to commit".
We have changed this file,
but we haven't told Git we will want to save those changes
(which we do with `git add`)
much less actually saved them.
Let's double-check our work using `git diff`,
which shows us the differences between
the current state of the file
and the most recently saved version:

~~~
$ git diff
~~~
{:class="in"}
~~~
diff --git a/mars.txt b/mars.txt
index df0654a..315bf3a 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1 +1,2 @@
 Cold and dry, but everything is my favorite color
+The two moons may be a problem for Wolfman
~~~
{:class="out"}

The output is cryptic because
it is actually a series of commands for tools like editors and `patch`
telling them how to reconstruct one file given the other.
If we can break it down into pieces:

1.  The first line tells us that Git is producing output similar to the Unix `diff` command
    comparing the old and new versions of the file.
2.  The second line tells exactly which [revisions](../../gloss.html#revision) of the file
    Git is comparing;
    `df0654a` and `315bf3a` are unique computer-generated labels for those revisions.
3.  The remaining lines show us the actual differences
    and the lines on which they occur.
    In particular,
    the `+` markers in the first column show where we are adding lines.

Let's commit our change:

~~~
$ git commit -m "Concerns about Mars's moons on my furry friend"
~~~
{:class="in"}
~~~
# On branch master
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#	modified:   mars.txt
#
no changes added to commit (use "git add" and/or "git commit -a")
~~~
{:class="out"}

Whoops:
Git won't commit because we didn't use `git add` first.
Let's fix that:

~~~
$ git add mars.txt
$ git commit -m "Concerns about Mars's moons on my furry friend"
~~~
{:class="in"}
~~~
[master 34961b1] Concerns about Mars's moons on my furry friend
 1 file changed, 1 insertion(+)
~~~
{:class="out"}

Git insists that we add files to the set we want to commit
before actually committing anything
because we may not want to commit everything at once.
For example,
suppose we're adding a few citations to our supervisor's work
to our thesis.
We might want to commit those additions,
and the corresponding addition to the bibliography,
but *not* commit the work we're doing on the conclusion
(which we haven't finished yet).

To allow for this,
Git has a special staging area
where it keeps track of things that have been added to
the current [change set](../../gloss.html#change-set)
but not yet committed.
`git add` puts things in this area,
and `git commit` then copies them to long-term storage (as a commit):

<img src="img/git-staging-area.svg" alt="The Git Staging Area" />

Let's watch as our changes to a file move from our editor
to the staging area
and into long-term storage.
First,
we'll add another line to the file:

~~~
$ nano mars.txt
$ cat mars.txt
~~~
{:class="in"}
~~~
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
~~~
{:class="out"}
~~~
$ git diff
~~~
{:class="in"}
~~~
diff --git a/mars.txt b/mars.txt
index 315bf3a..b36abfd 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1,2 +1,3 @@
 Cold and dry, but everything is my favorite color
 The two moons may be a problem for Wolfman
+But the Mummy will appreciate the lack of humidity
~~~
{:class="out"}

So far, so good:
we've added one line to the end of the file
(shown with a `+` in the first column).
Now let's put that change in the staging area
and see what `git diff` reports:

~~~
$ git add mars.txt
$ git diff
~~~
{:class="in"}

There is no output:
as far as Git can tell,
there's no difference between what it's been asked to save permanently
and what's currently in the directory.
However,
if we do this:

~~~
$ git diff --staged
~~~
{:class="in"}
~~~
diff --git a/mars.txt b/mars.txt
index 315bf3a..b36abfd 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1,2 +1,3 @@
 Cold and dry, but everything is my favorite color
 The two moons may be a problem for Wolfman
+But the Mummy will appreciate the lack of humidity
~~~
{:class="out"}

it shows us the difference between
the last committed change
and what's in the staging area.
Let's save our changes:

~~~
$ git commit -m "Thoughts about the climate"
~~~
{:class="in"}
~~~
[master 005937f] Thoughts about the climate
 1 file changed, 1 insertion(+)
~~~
{:class="out"}

check our status:

~~~
$ git status
~~~
{:class="in"}
~~~
# On branch master
nothing to commit, working directory clean
~~~
{:class="out"}

and look at the history of what we've done so far:

~~~
$ git log
~~~
{:class="in"}
~~~
commit 005937fbe2a98fb83f0ade869025dc2636b4dad5
Author: Vlad Dracula <vlad@tran.sylvan.ia>
Date:   Thu Aug 22 10:14:07 2013 -0400

    Thoughts about the climate

commit 34961b159c27df3b475cfe4415d94a6d1fcd064d
Author: Vlad Dracula <vlad@tran.sylvan.ia>
Date:   Thu Aug 22 10:07:21 2013 -0400

    Concerns about Mars's moons on my furry friend

commit f22b25e3233b4645dabd0d81e651fe074bd8e73b
Author: Vlad Dracula <vlad@tran.sylvan.ia>
Date:   Thu Aug 22 09:51:46 2013 -0400

    Starting to think about Mars
~~~
{:class="out"}

_Useful `git log` flags:_

* -3 (shows only the 3 most recent commits)
* --oneline (condenses each log into a single line, for quicker scanning)
* --stat (gives more details for each commit, ++--)
* --since=X.minutes/hours/days/weeks/months/years or YY-MM-DD-HH:MM (for specific time frames)
* --author=<pattern> (look for specific people)

To recap, when we want to add changes to our repository,
we first need to add the changed files to the staging area
(`git add`) and then commit the staged changes to the
repository (`git commit`):

<img src="img/git-committing.svg" alt="The Git Commit Workflow" />

### Exploring History

If we want to see what we changed when,
we use `git diff` again,
but refer to old versions
using the notation `HEAD~1`, `HEAD~2`, and so on:

~~~
$ git diff HEAD~1 mars.txt
~~~
{:class="in"}
~~~
diff --git a/mars.txt b/mars.txt
index 315bf3a..b36abfd 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1,2 +1,3 @@
 Cold and dry, but everything is my favorite color
 The two moons may be a problem for Wolfman
+But the Mummy will appreciate the lack of humidity
~~~
{:class="out"}
~~~
$ git diff HEAD~2 mars.txt
~~~
{:class="in"}
~~~
diff --git a/mars.txt b/mars.txt
index df0654a..b36abfd 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1 +1,3 @@
 Cold and dry, but everything is my favorite color
+The two moons may be a problem for Wolfman
+But the Mummy will appreciate the lack of humidity
~~~
{:class="out"}

_Useful git diff flags_

* `git diff --stat` gives us a summary of the filename and number of insertions/deletions
* `git diff -- filename` looks at the differences for a specific file

In this way,
we build up a chain of revisions.
The most recent end of the chain is referred to as `HEAD`;
we can refer to previous revisions using the `~` notation,
so `HEAD~1` (pronounced "head minus one")
means "the previous revision",
while `HEAD~123` goes back 123 revisions from where we are now.

We can also refer to revisions using
those long strings of digits and letters
that `git log` displays.
These are unique IDs for the changes,
and "unique" really does mean unique:
every change to any set of files on any machine
has a unique 40-character identifier.
Our first commit was given the ID
f22b25e3233b4645dabd0d81e651fe074bd8e73b,
so let's try this:

~~~
$ git diff f22b25e3233b4645dabd0d81e651fe074bd8e73b mars.txt
~~~
{:class="in"}
~~~
diff --git a/mars.txt b/mars.txt
index df0654a..b36abfd 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1 +1,3 @@
 Cold and dry, but everything is my favorite color
+The two moons may be a problem for Wolfman
+But the Mummy will appreciate the lack of humidity
~~~
{:class="out"}

That's the right answer,
but typing random 40-character strings is annoying,
so Git lets us use just the first few:

~~~
$ git diff f22b25e mars.txt
~~~
{:class="in"}
~~~
diff --git a/mars.txt b/mars.txt
index df0654a..b36abfd 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1 +1,3 @@
 Cold and dry, but everything is my favorite color
+The two moons may be a problem for Wolfman
+But the Mummy will appreciate the lack of humidity
~~~
{:class="out"}

### Recovering Old Versions

_So far, this seems like a lot of work. Why are we keeping track of all these little things??
Let's say you fatally ruin a file during an editing mistake 
(like when I deleted an awesome paragraph from my dissertation instead of cutting and pasting it like I meant to.) 
Maybe you even accidentally delete an important file (This code is old, why should I keep it?). 
If you have version control, you don't need to track down your System Administrator. You can fix your problem easily!_

All right:
we can save changes to files and see what we've changed---how
can we restore older versions of things?
Let's suppose we accidentally overwrite our file:

~~~
$ nano mars.txt
$ cat mars.txt
~~~
{:class="in"}
~~~
We will need to manufacture our own oxygen
~~~
{:class="out"}

`git status` now tells us that the file has been changed,
but those changes haven't been staged:

~~~
$ git status
~~~
{:class="in"}
~~~
# On branch master
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#	modified:   mars.txt
#
no changes added to commit (use "git add" and/or "git commit -a")
~~~
{:class="out"}

We can put things back the way they were
by using `git checkout`:

~~~
$ git checkout HEAD mars.txt
$ cat mars.txt
~~~
{:class="in"}
~~~
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
~~~
{:class="out"}

As you might guess from its name,
`git checkout` checks out (i.e., restores) an old version of a file.
In this case,
we're telling Git that we want to recover the version of the file recorded in `HEAD`,
which is the last saved revision.

_This would work even if we deleted our file, and wanted to get it back!_
_delete mars.txt, and then show that it can be checked back out_

If we want to go back even further,
we can use a revision identifier instead:

~~~
$ git checkout f22b25e mars.txt
~~~
{:class="in"}

It's important to remember that
we must use the revision number that identifies the state of the repository
*before* the change we're trying to undo.
A common mistake is to use the revision number of
the commit in which we made the change we're trying to get rid of.
In the example below, we want retrieve the state from before the most
recent commit (`HEAD~1`), which is revision `f22b25e`:

<img src="img/git-checkout.svg" alt="Git Checkout" />

The following diagram illustrates what the history of a file might look
like (moving back from `HEAD`, the most recently committed version):

<img src="img/git-when-revisions-updated.svg" alt="When Git Updates Revision Numbers" />

> #### Simplifying the Common Case
>
> If you read the output of `git status` carefully,
> you'll see that it includes this hint:
>
> ~~~
> (use "git checkout -- <file>..." to discard changes in working directory)
> ~~~
> {:class="in"}
>
> As it says,
> `git checkout` without a version identifier restores files to the state saved in `HEAD`.
> The double dash `--` is needed to separate the names of the files being recovered
> from the command itself:
> without it,
> Git would try to use the name of the file as the revision identifier.

The fact that files can be reverted one by one
tends to change the way people organize their work.
If everything is in one large document,
it's hard (but not impossible) to undo changes to the introduction
without also undoing changes made later to the conclusion.
If the introduction and conclusion are stored in separate files,
on the other hand,
moving backward and forward in time becomes much easier.
Or for your code, if you store functions in files separate from code that executes them, or makes figures,
you can go back in time to find or retrieve specific chunks.

### Ignoring Things

What if we have files that we do not want Git to track for us,
like backup files created by our editor
or intermediate files created during data analysis.

_while git can keep track of data files, this is often not a great idea.
Share story of .rdata files in collaborative project. Why they needed to be in the .git ignore file_

Let's create a few dummy files:

~~~
$ mkdir results
$ touch a.dat b.dat c.dat results/a.out results/b.out
~~~
{:class="in"}

and see what Git says:

~~~
$ git status
~~~
{:class="in"}
~~~
# On branch master
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#
#	a.dat
#	b.dat
#	c.dat
#	results/
nothing added to commit but untracked files present (use "git add" to track)
~~~
{:class="out"}

_Note: if you already added these files (git add .) you can unstage them by typing git reset HEAD_

Putting these files under version control would be a waste of disk space.
What's worse,
having them all listed could distract us from changes that actually matter,
so let's tell Git to ignore them.

We do this by creating a file in the root directory of our project called `.gitignore`.

~~~
$ nano .gitignore
$ cat .gitignore
~~~
{:class="in"}
~~~
*.dat
results/
~~~
{:class="out"}

These patterns tell Git to ignore any file whose name ends in `.dat`
and everything in the `results` directory.
(If any of these files were already being tracked,
Git would continue to track them.)

Once we have created this file,
the output of `git status` is much cleaner:

~~~
$ git status
~~~
{:class="in"}
~~~
# On branch master
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#
#	.gitignore
nothing added to commit but untracked files present (use "git add" to track)
~~~
{:class="out"}

The only thing Git notices now is the newly-created `.gitignore` file.
You might think we wouldn't want to track it,
but everyone we're sharing our repository with will probably want to ignore
the same things that we're ignoring.
Let's add and commit `.gitignore`:

~~~
$ git add .gitignore
$ git commit -m "Add the ignore file"
$ git status
~~~
{:class="in"}
~~~
# On branch master
nothing to commit, working directory clean
~~~
{:class="out"}

As a bonus,
using `.gitignore` helps us avoid accidentally adding files to the repository that we don't want.

~~~
$ git add a.dat
~~~
{:class="in"}
~~~
The following paths are ignored by one of your .gitignore files:
a.dat
Use -f if you really want to add them.
fatal: no files added
~~~
{:class="out"}

If we really want to override our ignore settings,
we can use `git add -f` to force Git to add something.
We can also always see the status of ignored files if we want:

~~~
$ git status --ignored
~~~
{:class="in"}
~~~
# On branch master
# Ignored files:
#  (use "git add -f <file>..." to include in what will be committed)
#
#        a.dat
#        b.dat
#        c.dat
#        results/

nothing to commit, working directory clean
~~~
{:class="out"}

_To discard all your most recent changes and GO BACK IN TIME, 
first look at your `git log` to decide what version you want to go back to. 
Remember the first 5-7 digits in the commit code of the version that wasn't screwed up._

_use `git reset --hard versioncode`_

_To roll back to a specific file, use 
`git checkout version name --filename`_

_To roll back one version (usually I know that I messed up pretty quickly)
`git checkout master~1 PathToFile`_


_A short exercise to show moving back and forth. If I mess up and I notice right away, 
 might want to go back in time quickly. Commit some changes to `mars.txt`. 
 I can return to the previous version by typing `git checkout master~1 mars.txt`, and then
 committing those changes with a message like "I deleted the thing I just added". 
 This will preserve your entire history, including the short-lived mistake, which will allow
 you to return to it if you should decide it wasn't a mistake at all. 
 If you check out the entire repository using `git checkout master~1` you will be in 
 "detached head state". This can be quickly remedied by typing `git checkout master`, to return 
 you to the correct place. Detached head is when the head is not the same version as the master._


<div class="keypoints" markdown="1">

#### Key Points
*   Use `git config` to configure a user name, email address, editor, and other preferences once per machine.
*   `git init` initializes a repository.
*   `git status` shows the status of a repository.
*   Files can be stored in a project's working directory (which users see),
    the staging area (where the next commit is being built up)
    and the local repository (where snapshots are permanently recorded).
*   `git add` puts files in the staging area.
*   `git commit` creates a snapshot of the staging area in the local repository.
*   Always write a log message when committing changes.
*   `git diff` displays differences between revisions.
*   `git checkout` recovers old versions of files.
*   The `.gitignore` file tells Git what files to ignore.

</div>

<div class="challenge" markdown="1">
Create a new Git repository on your computer called `bio`.
Write a three-line biography for yourself in a file called `me.txt`,
commit your changes,
then modify one line and add a fourth and display the differences
between its updated state and its original state.
</div>

<div class="challenge" markdown="1">
The following sequence of commands creates one Git repository inside another:

~~~
cd           # return to home directory
mkdir alpha  # make a new directory alpha
cd alpha     # go into alpha
git init     # make the alpha directory a Git repository
mkdir beta   # make a sub-directory alpha/beta
cd beta      # go into alpha/beta
git init     # make the beta sub-directory a Git repository
~~~
{:class="in"}

Why is it a bad idea to do this?
</div>