Git usage
=========
<!-- :: \iffalse -->
[![CC BY 4.0 License][SHIELD_CCBY]][CCBY]
<!-- :: -->
[SHIELD_PDF]: https://img.shields.io/badge/download-PDF-brightgreen.svg
[SHIELD_TXT]: https://img.shields.io/badge/download-TXT-brightgreen.svg
[SHIELD_CCBY]: https://img.shields.io/badge/license-CC%20BY%204.0-blue.svg
<!-- :: \fi -->
<!-- Version 0.1a -->
<!-- :: \maketitle -->

This document starts with basic git usage and explains a basic
work-flow so developers may contribute pull requests to an upstream
repository and how upstream owners may merge pull requests from contributors.

The most recent version of this document is available at
<https://github.com/p-id/gitr>.

Contents
--------
* [Introduction](#introduction)
* [Configure tooling](#configure-tooling)
* [Create repositories](#create-repositories)
* [Make changes](#make-changes)
* [Group changes](group-changes)
* [Refactor file names](#refactor-file-names)
* [Suppress tracking](#suppress-tracking)
* [Save fragments](#save-fragments)
* [Review history](#review-history)
* [Redo commits](#redo-commits)
* [Synchronize changes](#synchronize-changes)
* [Create Pull Request](#create-pull-request)
* [Adding a New Subproject](#adding-a-new-subproject)
* [Viewing a Diff of the Subproject](#viewing-a-diff-of-the-subproject)
* [Cloning a Repository with a Subproject](#cloning-a-repository-with-a-subproject)
* [Pulling in Subproject Updates](#pulling-in-subproject-updates)
* [Making Changes to a Subproject](#making-changes-to-a-subproject)
* [Pushing Changes to the Subproject Repository](#pushing-changes-to-the-subproject-repository)
* [Helpful Configs for Submodules](#helpful-configs-for-submodules)
* [License](#license)
* [Support](#support)

Introduction
------------
Every project has a main development branch where the developers push
commits on a day-to-day basis. Usually, the main development branch is
`master` but some projects choose to have `develop` or `trunk` or
another branch for day-to-day development activities. We refer to this
main development branch as *main development branch* throughout this
document to keep the text general. However in the command examples and
ASCII-diagrams, we use `master` as an example of the main development
branch.

We use the following placeholders in the command examples and
ASCII-diagrams in this document:

  - `GITHUB`: [`github.com`](https://github.com/) or domain
    name/hostname of your private GitHub Enterprise system.
  - `USER` or `CONTRIBUTOR`: The user that forks an upstream repository,
    creates pull requests, and sends them to the upstream repository.
  - `UPSTREAM-OWNER`: Owner of the upstream repository. This is the name
    of the user or organization that merges pull requests into the
    upstream repository.
  - `REPO`: Repository name.
  - `FILES`: One or more filenames to be staged for a commit.
  - `TOPIC-BRANCH`: Feature-specific or bug-specific branch where a
    contributor develops her or his contribution. This is referred to as
    *topic branch* in the text.

Configure tooling
-----------------
Configure user information for all local repositories

    $ git config --global user.name "[name]"

Sets the name you want attached to your commit transactions

    $ git config --global user.email "[email address]"

Sets the email you want attached to your commit transactions


Create repositories
-------------------
Start a new repository or obtain one from an existing URL

    $ git init [project-name]

Creates a new local repository with the specified name

    $ git clone [url]

Downloads a project and its entire version history

Make changes
------------
Review edits and craft a commit transaction

$ git status

Lists all new or modified files to be committed

$ git diff

Shows file differences not yet staged

$ git add [file]

Snapshots the file in preparation for versioning

$ git diff --staged

Shows file differences between staging and the last file version

$ git reset [file]

Unstages the file, but preserves its contents

$ git commit -m"[descriptive message]"

Records file snapshots permanently in version history

Group changes
-------------

Name a series of commits and combine completed efforts

$ git branch

Lists all local branches in the current repository

$ git branch [branch-name]

Creates a new branch

$ git checkout [branch-name]

Switches to the specified branch and updates working directory

$ git merge [branch-name]

Combines the specified branch’s history into the current branch

$ git branch -d [branch-name]

Deletes the specified branch

Refactor file names
-------------------
Relocate and remove versioned files

$ git rm [file]

Deletes the file from the working directory and stages the deletion

$ git rm --cached [file]

Removes the file from version control but preserves the file locally

$ git mv [file-original] [file-renamed]

Changes the file name and prepare it for commit

Suppress tracking
-----------------
Exclude temporary files and paths

*.log
build/
temp-*

A text file named .gitignore suppresses accidental versioning of files and paths matching the specified patterns

$ git ls-files --others --ignored --exclude-standard

Lists all ignored files in this project

Save fragments
--------------
Shelve and restore incomplete changes

$ git stash

Temporarily stores all modified tracked files

$ git stash pop

Restores the most recently stashed files

$ git stash list

Lists all stashed changesets

$ git stash drop

Discards the most recently stashed changeset

Review history
--------------
Browse and inspect the evolution of project files

$ git log

Lists version history for the current branch

$ git log --follow [file]

Lists version history for the file, including renames

$ git diff [first-branch]...[second-branch]

Shows content differences between two branches

$ git show [commit]

Outputs metadata and content changes of the specified commit

Redo commits
------------
Erase mistakes and craft replacement history

$ git reset [commit]

Undoes all commits after [commit], preserving changes locally

$ git reset --hard [commit]

Discards all history and changes back to the specified commit

Synchronize changes
-------------------
Register a remote (URL) and exchange repository history

$ git fetch [remote]

Downloads all history from the remote repository

$ git merge [remote]/[branch]

Combines the remote branch into the current local branch

$ git push [remote] [branch]

Uploads all local branch commits to GitHub

$ git pull

Downloads bookmark history and incorporates changes


Create Pull Request
-------------------
This section is meant for developers who contribute new commits to the
upstream repository from their personal fork.


### Fork and Clone
On GitHub, fork the upstream repository to your personal user account.

Then clone your fork from your personal GitHub user account to your
local system and set the upstream repository URL as a remote named
`upstream`.

    git clone https://GITHUB/USER/REPO.git
    cd REPO
    git remote add upstream https://GITHUB/UPSTREAM-OWNER/REPO.git
    git remote -v

Now the remote named `upstream` points to the upstream repository and
the remote named `origin` points to your fork.

In the fork and pull request workflow, a contributor must never push
commits to `upstream`. A contributor must only push commits to `origin`.


### Work on Pull Request
Work on a new pull request in a new topic branch and commit to your
fork. Remember to use a meaningful name instead of `TOPIC-BRANCH` in the
commands below.

    git checkout -b TOPIC-BRANCH
    git add FILES
    git commit
    git push origin TOPIC-BRANCH

Create pull request via GitHub web interface as per the following steps:

  - Go to your fork on GitHub.
  - Switch to the topic branch.
  - Click *Compare & pull request*.
  - Click *Create pull request*.


  Wait for an upstream developer to review and merge your pull request.

  If there are review comments to be addressed, continue working on your
  branch, commiting, optionally squashing and rebasing them, and pushing
  them to the topic branch of `origin` (your fork). Any changes to the
  topic branch automatically become available in the pull request.

  In the fork and pull request workflow, a contributor should never commit
  anything to the main development branch of personal fork. This makes it
  very easy to keep the main development branch of your fork in sync with
  that of the upstream repository. This is explained in the next
  subsection.


Adding a New Subproject
-----------------------

### Submodule
    git submodule add https://GITHUB/USER/example-submodule

    git commit -m "adding new submodule"

The submodule add command adds a new file called .gitmodules along with a
subdirectory containing the files from example-submodule. Both are added to
your index (staging area) and you simply need to commit them.
The submodule’s history remains independent of the parent project.

### Subtree

    git subtree add --prefix=example-submodule https://GITHUB/USER/example-submodule master --squash

The subtree command adds a subdirectory containing the files from
example-submodule. The most common practice is to use the --squash option to
combine the subproject’s history into a single commit, which is then grafted
onto the existing tree of the parent project. You can omit the --squash option
to maintain all of the history from the designated branch of the subproject.

Viewing a Diff of the Subproject
--------------------------------
### Submodule
To view a diff of the submodule:

    git diff --cached example-submodule
    git diff --cached --submodule example-submodule

### Subtree
No special command required

Cloning a Repository with a Subproject
--------------------------------------
### Submodule
Anyone who clones will need to:

    git clone --recursive URL

Anyone who already has a local copy of the repo will need to:

    git submodule update --init

### Subtree
  No special command required


Pulling in Subproject Updates
-----------------------------
### Submodule
    git submodule update --remote

If you have more than one submodule, you can add the name of the submodule to
the end of the command to specify which subproject to update.

By default, this will update the submodule and check out to the default branch of the submodule remote.

You can change the default branch with:

    git config -f .gitmodules submodule.example-submodule.branch other-branch

### Subtree
    git subtree pull --prefix=example-submodule https://GITHUB/USER/REPO/example-submodule master --squash

You can shorten the command by adding the subtree URL as a remote:

    git remote add sub-remote https://GITHUB/USER/REPO/example-submodule.git

You can add/pull from other refs by replacing master with the desired ref (e.g. stable, v1.0).

Making Changes to a Subproject
------------------------------
In most cases, it is considered best practice to make changes in the subproject
repository and pull them in to the parent project. When this is not practical,
follow these instructions:

### Submodule
Access the submodule directory and create a branch:

    cd example-submodule
    git checkout -b branch-name master

Changes require two commits, one in the subproject repository and one in the parent repository.

###Subtree
No special command required, changes will be committed on the parent project branch.

It is possible to create commits mixing changes to the subproject and the parent project, but this is generally discouraged.

Pushing Changes to the Subproject Repository
--------------------------------------------
### Submodule
While in the submodule directory:

    git push

Or while in the parent directory:

    git push --recurse-submodules=on-demand

### Subtree
    git subtree push --prefix= example-submodule https://GITHUB/USER/REPO/example-submodule master


Helpful Configs for Submodules
------------------------------
Always show the submodule log when you diff:

    git config --global diff.submodule log

Show a short summary of submodule changes in your status message:

    git config status.submoduleSummary true

See the diffs in all of your submodules:

    git config alias.sdiff "git diff; git submodule foreach 'git diff'"


License
-------
Copyright &copy; 2018 Piyush Dewnani

[![CC BY 4.0 Logo](meta/ccby.svg "CC BY 4.0")][CCBY]
<!-- :: -->
This document is licensed under the
[Creative Commons Attribution 4.0 International License][CCBY].

You are free to share the material in any medium or format and/or adapt
the material for any purpose, even commercially, under the terms of the
Creative Commons Attribution 4.0 International (CC BY 4.0) License.

This document is provided **as-is and as-available,**
**without representations or warranties of any kind,** whether
express, implied, statutory, or other. See the
[CC BY 4.0 Legal Code][CCBYLC] for details.

[CCBY]: http://creativecommons.org/licenses/by/4.0/
[CCBYLC]: https://creativecommons.org/licenses/by/4.0/legalcode


Support
-------
To report bugs, suggest improvements, or ask questions,
[create issues][ISSUES].

To contribute improvements to this document,
[fork this project][REPO] and *[create pull request][PR]!* ;-)

[ISSUES]: https://github.com/p-id/gitr/issues
[REPO]: https://github.com/p-id/gitr
[PR]: #create-pull-request
