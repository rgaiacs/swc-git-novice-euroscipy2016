---
title: Tracking Changes
teaching: 20
exercises: 0
questions:
- "How do I record changes in Git?"
- "How do I check the status of my version control repository?"
- "How do I record notes about what changes I made and why?"
objectives:
- "Go through the modify-add-commit cycle for one or more files."
- "Explain where information is stored at each stage of Git commit workflow."
keypoints:
- "`git status` shows the status of a repository."
- "Files can be stored in a project's working directory (which users see), the staging area (where the next commit is being built up) and the local repository (where commits are permanently recorded)."
- "`git add` puts files in the staging area."
- "`git commit` saves the staged content as a new commit in the local repository."
- "Always write a log message when committing changes."
---
Let's create a directory called `euroscipy2016`.

~~~
$ mkdir euroscipy2016
~~~
{: .bash}

Let's create a file called `euroscipy2016/__init__.py`.

~~~
$ touch euroscipy2016/__init__.py
~~~
{: .bash}

We can se the file that we created by runnin:

~~~
$ ls -R
~~~
{: .bash}

~~~
.:
euroscipy2016/

./euroscipy2016:
__init__.py
~~~
{: .output}

If we check the status of our project again,
Git tells us that it's noticed the new file:

~~~
$ git status
~~~
{: .bash}

~~~
# On branch master
#
# Initial commit
#
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#
#	euroscipy2016/
nothing added to commit but untracked files present (use "git add" to track)
~~~
{: .output}

The "untracked files" message means that there's a file in the directory
that Git isn't keeping track of.
We can tell Git to track a file using `git add`:

~~~
$ git add euroscipy2016/__init__.py
~~~
{: .bash}

and then check that the right thing happened:

~~~
$ git status
~~~
{: .bash}

~~~
# On branch master
#
# Initial commit
#
# Changes to be committed:
#   (use "git rm --cached <file>..." to unstage)
#
#	new file:   euroscipy2016/__init__.py
#
~~~
{: .output}

Git now knows that it's supposed to keep track of `euroscipy2016/__init.py`,
but it hasn't recorded these changes as a commit yet.
To get it to do that,
we need to run one more command:

~~~
$ git commit -m "Create my first Python package"
~~~
{: .bash}

~~~
[master (root-commit) f22b25e] Create my first Python package
 1 file changed, 1 insertion(+)
 create mode 100644 euroscipy2016/__init__.py
~~~
{: .output}

When we run `git commit`,
Git takes everything we have told it to save by using `git add`
and stores a copy permanently inside the special `.git` directory.
This permanent copy is called a [commit](reference.html#commit)
(or [revision](reference.html#revision)) and its short identifier is `f22b25e`
(Your commit may have another identifier.)

We use the `-m` flag (for "message")
to record a short, descriptive, and specific comment that will help us remember later on what we did and why.
If we just run `git commit` without the `-m` option,
Git will launch `nano` (or whatever other editor we configured as `core.editor`)
so that we can write a longer message.

[Good commit messages][commit-messages] start with a brief (<50 characters) summary of
changes made in the commit.  If you want to go into more detail, add
a blank line between the summary line and your additional notes.

If we run `git status` now:

~~~
$ git status
~~~
{: .bash}

~~~
# On branch master
nothing to commit, working directory clean
~~~
{: .output}

it tells us everything is up to date.
If we want to know what we've done recently,
we can ask Git to show us the project's history using `git log`:

~~~
$ git log
~~~
{: .bash}

~~~
commit f22b25e3233b4645dabd0d81e651fe074bd8e73b
Author: Vlad Dracula <vlad@tran.sylvan.ia>
Date:   Thu Aug 22 09:51:46 2013 -0400

    Create my first Python package
~~~
{: .output}

`git log` lists all commits  made to a repository in reverse chronological order.
The listing for each commit includes
the commit's full identifier
(which starts with the same characters as
the short identifier printed by the `git commit` command earlier),
the commit's author,
when it was created,
and the log message Git was given when the commit was created.

> ## Where Are My Changes?
>
> If we run `ls` at this point, we will still see just one file called `euroscipy2016/__init__.py`.
> That's because Git saves information about files' history
> in the special `.git` directory mentioned earlier
> so that our filesystem doesn't become cluttered
> (and so that we can't accidentally edit or delete an old version).
{: .callout}

Now suppose Dracula adds information to the file.
(We'll use `nano` to edit the file;
you can use whatever editor you like.
In particular, this does not have to be the `core.editor` you set globally earlier.)

~~~
$ nano euroscipy2016/__init__.py
$ cat euroscipy2016/__init__.py
~~~
{: .bash}

~~~
__version__ = '0.0.1'
~~~
{: .output}

When we run `git status` now,
it tells us that a file it already knows about has been modified:

~~~
$ git status
~~~
{: .bash}

~~~
# On branch master
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#	modified:   euroscipy2016/__init__.py
#
no changes added to commit (use "git add" and/or "git commit -a")
~~~
{: .output}

The last line is the key phrase:
"no changes added to commit".
We have changed this file,
but we haven't told Git we will want to save those changes
(which we do with `git add`)
nor have we saved them (which we do with `git commit`).
So let's do that now. It is good practice to always review
our changes before saving them. We do this using `git diff`.
This shows us the differences between the current state
of the file and the most recently saved version:

~~~
$ git diff
~~~
{: .bash}

~~~
diff --git a/euroscipy2016/__init__.py b/euroscipy2016/__init__.py
index df0654a..315bf3a 100644
--- a/euroscipy2016/__init__.py
+++ b/euroscipy2016/__init__.py
@@ -0,0 +1 @@
+__version__ = '0.0.1'
~~~
{: .output}

The output is cryptic because
it is actually a series of commands for tools like editors and `patch`
telling them how to reconstruct one file given the other.
If we break it down into pieces:

1.  The first line tells us that Git is producing output similar to the Unix `diff` command
    comparing the old and new versions of the file.
2.  The second line tells exactly which versions of the file
    Git is comparing;
    `df0654a` and `315bf3a` are unique computer-generated labels for those versions.
3.  The third and fourth lines once again show the name of the file being changed.
4.  The remaining lines are the most interesting, they show us the actual differences
    and the lines on which they occur.
    In particular,
    the `+` marker in the first column shows where we added a line.

After reviewing our change, it's time to commit it:

~~~
$ git commit -m "Add version to my first package"
$ git status
~~~
{: .bash}

~~~
# On branch master
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#	modified:   euroscipy2016/__init__.py
#
no changes added to commit (use "git add" and/or "git commit -a")
~~~
{: .output}

Whoops:
Git won't commit because we didn't use `git add` first.
Let's fix that:

~~~
$ git add euroscipy2016/__init__.py
$ git commit -m "Add version to my first package"
~~~
{: .bash}

~~~
[master 34961b1] Add version to my first package
 1 file changed, 1 insertion(+)
~~~
{: .output}

Git insists that we add files to the set we want to commit
before actually committing anything. This allows us to commit our
changes in stages and capture changes in logical portions rather than
only large batches.
For example,
suppose we're adding a few citations to our supervisor's work
to our thesis.
We might want to commit those additions,
and the corresponding addition to the bibliography,
but *not* commit the work we're doing on the conclusion
(which we haven't finished yet).

To allow for this,
Git has a special *staging area*
where it keeps track of things that have been added to
the current [change set](reference.html#change-set)
but not yet committed.

> ## Staging Area
>
> If you think of Git as taking snapshots of changes over the life of a project,
> `git add` specifies *what* will go in a snapshot
> (putting things in the staging area),
> and `git commit` then *actually takes* the snapshot, and
> makes a permanent record of it (as a commit).
> If you don't have anything staged when you type `git commit`,
> Git will prompt you to use `git commit -a` or `git commit --all`,
> which is kind of like gathering *everyone* for the picture!
> However, it's almost always better to
> explicitly add things to the staging area, because you might
> commit changes you forgot you made. (Going back to snapshots,
> you might get the extra with incomplete makeup walking on
> the stage for the snapshot because you used `-a`!)
> Try to stage things manually,
> or you might find yourself searching for "git undo commit" more
> than you would like!
{: .callout}

![The Git Staging Area](../fig/git-staging-area.svg)

Let's watch as our changes to a file move from our editor
to the staging area
and into long-term storage.
First,
we'll add another line to the file:

~~~
$ nano euroscipy2016/__init__.py
$ cat euroscipy2016/__init__.py
~~~
{: .bash}

~~~
__version__ = '0.0.1'
__author__ = 'Vlad Dracula'
~~~
{: .output}

~~~
$ git diff
~~~
{: .bash}

~~~
diff --git a/euroscipy2016/__init__.py b/euroscipy2016/__init__.py
index 315bf3a..b36abfd 100644
--- a/euroscipy2016/__init__.py
+++ b/euroscipy2016/__init__.py
@@ -1,2 +1,3 @@
 __version__ = '0.0.1'
+__author__ = 'Vlad Dracula'
~~~
{: .output}

So far, so good:
we've added one line to the end of the file
(shown with a `+` in the first column).
Now let's put that change in the staging area
and see what `git diff` reports:

~~~
$ git add euroscipy2016/__init__.py
$ git diff
~~~
{: .bash}

There is no output:
as far as Git can tell,
there's no difference between what it's been asked to save permanently
and what's currently in the directory.
However,
if we do this:

~~~
$ git diff --staged
~~~
{: .bash}

~~~
diff --git a/euroscipy2016/__init__.py b/euroscipy2016/__init__.py
index 315bf3a..b36abfd 100644
--- a/euroscipy2016/__init__.py
+++ b/euroscipy2016/__init__.py
@@ -1,2 +1,3 @@
 __version__ = '0.0.1'
+__author__ = 'Vlad Dracula'
~~~
{: .output}

it shows us the difference between
the last committed change
and what's in the staging area.
Let's save our changes:

~~~
$ git commit -m "Add author's name"
~~~
{: .bash}

~~~
[master 005937f] Add author's name
 1 file changed, 1 insertion(+)
~~~
{: .output}

check our status:

~~~
$ git status
~~~
{: .bash}

~~~
# On branch master
nothing to commit, working directory clean
~~~
{: .output}

and look at the history of what we've done so far:

~~~
$ git log
~~~
{: .bash}

~~~
commit 005937fbe2a98fb83f0ade869025dc2636b4dad5
Author: Vlad Dracula <vlad@tran.sylvan.ia>
Date:   Thu Aug 22 10:14:07 2013 -0400

    Add author's name

commit 34961b159c27df3b475cfe4415d94a6d1fcd064d
Author: Vlad Dracula <vlad@tran.sylvan.ia>
Date:   Thu Aug 22 10:07:21 2013 -0400

    Add version to my first package

commit f22b25e3233b4645dabd0d81e651fe074bd8e73b
Author: Vlad Dracula <vlad@tran.sylvan.ia>
Date:   Thu Aug 22 09:51:46 2013 -0400

    Create my first Python package
~~~
{: .output}

> ## Paging the Log
>
> When the output of `git log` is too long to fit in your screen,
> `git` uses a program to split it into pages of the size of your screen.
> When this "pager" is called, you will notice that the last line in your
> screen is a `:`, instead of your usual prompt.
>
> *   To get out of the pager, press `q`.
> *   To move to the next page, press the space bar.
> *   To search for `some_word` in all pages, type `/some_word`
>     and navigate throught matches pressing `n`.
{: .callout}

To recap, when we want to add changes to our repository,
we first need to add the changed files to the staging area
(`git add`) and then commit the staged changes to the
repository (`git commit`):

![The Git Commit Workflow](../fig/git-committing.svg)

> ## Choosing a Commit Message
>
> Which of the following commit messages would be most appropriate for the
> last commit made to `euroscipy2016/__init__.py`?
>
> 1. "Changes"
> 2. "Added line '__author__ = 'Vlad Dracula'' to euroscipy2016/__init__.py"
> 3. "Add Dracula as author"
>
> > ## Solution
> > Answer 1 is not descriptive enough,
> > and answer 2 is too descriptive and redundant,
> > but answer 3 is good: short but descriptive.
> {: .solution}
{: .challenge}

> ## Committing Changes to Git
>
> Which command(s) below would save the changes of `myfile.txt`
> to my local Git repository?
>
> 1. `$ git commit -m "my recent changes"`
>
> 2. `$ git init myfile.txt`  
>    `$ git commit -m "my recent changes"`
>
> 3. `$ git add myfile.txt`  
>    `$ git commit -m "my recent changes"`
>
> 4. `$ git commit -m myfile.txt "my recent changes"`
>
> > ## Solution
> >
> > 1. Would only create a commit if files have already been staged.
> > 2. Would try to create a new repository.
> > 3. Is correct: first add the file to the staging area, then commit.
> > 4. Would try to commit a file "my recent changes" with the message myfile.txt.
> {: .solution}
{: .challenge}

> ## Committing Multiple Files
>
> The staging area can hold changes from any number of files
> that you want to commit as a single snapshot.
>
> 1. Add some comments to `euroscipy2016/__init__.py`.
> 2. Create a new file `euroscipy2016/talks.py` with a list of talks' title.
> 3. Add changes from both files to the staging area,
> and commit those changes.
{: .challenge}

> ## Author and Committer
>
> For each of the commits you have done, Git stored your name twice.
> You are named as the author and as the committer. You can observe
> that by telling Git to show you more information about your last
> commits:
>
> ~~~
> $ git log --format=full
> ~~~
> {: .bash}
>
> When commiting you can name someone else as the author:
>
> ~~~
> $ git commit --author="Vlad Dracula <vlad@tran.sylvan.ia>"
> ~~~
> {: .bash}
>
> Create a new repository and create two commits: one without the
> `--author` option and one by naming a colleague of yours as the
> author. Run `git log` and `git log --format=full`. Think about ways
> how that can allow you to collaborate with your colleagues.
>
> > ## Solution
> >
> > ~~~
> > $ git add me.txt
> > $ git commit -m "Updated Vlad's bio." --author="Frank N. Stein <franky@monster.com>"
> > ~~~
> > {: .bash}
> > ~~~
> > [master 4162a51] Updated Vlad's bio.
> > Author: Frank N. Stein <franky@monster.com>
> > 1 file changed, 2 insertions(+), 2 deletions(-)
> >
> > $ git log --format=full
> > commit 4162a51b273ba799a9d395dd70c45d96dba4e2ff
> > Author: Frank N. Stein <franky@monster.com>
> > Commit: Vlad Dracula <vlad@tran.sylvan.ia>
> >
> > Updated Vlad's bio.
> >
> > commit aaa3271e5e26f75f11892718e83a3e2743fab8ea
> > Author: Vlad Dracula <vlad@tran.sylvan.ia>
> > Commit: Vlad Dracula <vlad@tran.sylvan.ia>
> >
> > Vlad's initial bio.
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

[commit-messages]: http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html
