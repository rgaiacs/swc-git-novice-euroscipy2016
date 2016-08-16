---
title: Creating a Repository
teaching: 10
exercises: 0
questions:
- "Where does Git store information?"
objectives:
- "Create a local Git repository."
keypoints:
- "`git init` initializes a repository."
---

Once Git is configured,
we can start using it.
Let's create a directory for our work and then move into that directory:

~~~
$ mkdir euroscipy2016
$ cd euroscipy2016
~~~
{: .bash}

Then we tell Git to make `euroscipy2016` a [repository](reference.html#repository)—a place where
Git can store versions of our files:

~~~
$ git init
~~~
{: .bash}

If we use `ls` to show the directory's contents,
it appears that nothing has changed:

~~~
$ ls
~~~
{: .bash}

But if we add the `-a` flag to show everything,
we can see that Git has created a hidden directory within `euroscipy2016` called `.git`:

~~~
$ ls -a
~~~
{: .bash}

~~~
.	..	.git
~~~
{: .output}

Git stores information about the project in this special sub-directory.
If we ever delete it,
we will lose the project's history.

We can check that everything is set up correctly
by asking Git to tell us the status of our project:

~~~
$ git status
~~~
{: .bash}

~~~
# On branch master
#
# Initial commit
#
nothing to commit (create/copy files and use "git add" to track)
~~~
{: .output}

> ## Places to Create Git Repositories
>
> Dracula starts a new project, `moons`, related to his `euroscipy2016` project.
> Despite Wolfman's concerns, he enters the following sequence of commands to
> create one Git repository inside another:
>
> ~~~
> cd             # return to home directory
> mkdir euroscipy2016  # make a new directory euroscipy2016
> cd euroscipy2016     # go into euroscipy2016
> git init       # make the euroscipy2016 directory a Git repository
> mkdir tutorials    # make a sub-directory euroscipy2016/tutorials
> cd tutorials       # go into euroscipy2016/tutorials
> git init       # make the tutorials sub-directory a Git repository
> ~~~
> {: .bash}
>
> Why is it a bad idea to do this?
> How can Dracula undo his last `git init`?
{: .challenge}
