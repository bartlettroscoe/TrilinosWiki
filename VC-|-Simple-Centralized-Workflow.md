No matter what git-related workflow that is being used, developers need to understand how to maintain a simple linear shared remote tracking branch.  This shared remote branch may be the ['develop' branch](https://github.com/trilinos/Trilinos/wiki/VC-|-'develop'-'master'-workflow), a shared topic branch, or a Trilinos release branch.  This workflow should be used when a single local commit (or a very small number of local commits) are pushed to the shared remote tracking branch.  In these cases, one should rebase the local commits on top of the updated remote tracking branch before the push in order to maintain a nice linear history on that branch.  The motivation of this simple workflow is explained in more detail at the [Simple Centralized CI Workflow](https://docs.google.com/document/d/1uVQYI2cmNx09fDkHDA136yqDTqayhxqfvjFiuUue7wo/edit#heading=h.7z34akh7lsvp).

## The simple centralized workflow

The simple centralized workflow (i.e. the git equivalent of SVN) is outlined on the Atlassian page [Centralized Git Workflows](https://www.atlassian.com/pt/git/workflows#!workflow-centralized) as the most basic valid git workflow.  On this Atlassian page, it is stated:

* “Your team can develop projects in the exact same way as they do with Subversion.”
* “Before the developer can publish their feature, they need to fetch the updated central commits and rebase their changes on top of them. This is like saying, “I want to add my changes to what everyone else has already done.” The result is a perfectly linear history, just like in traditional SVN workflows”

## Trilinos is moving towards a feature branch workflow which builds on top of the centralized workflow as outlined at [Feature Branch Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow) whith develop as the central branch instead of master.  As stated on that page:

* "Aside from isolating feature development, branches make it possible to discuss changes via pull requests. Once someone completes a feature, they don’t immediately merge it into develop. Instead, they push the feature branch to the central server and file a pull request asking to merge their additions into develop. This gives other developers an opportunity to review the changes before they become a part of the main codebase."

In our case this also facilitates automated testing for each pull request on a number of scenarios that have been deemed important for various reasons.

| Step                                          | Git Commands |
| ---                                           | --- |
| **Get latest updates from the central repo:** | `git pull --rebase` (i.e. from`origin/develop`) |
| **Start a topic branch:**                     | `git checkout -b cool-new-feature-name develop |
| **Make local changes and create commits:**    | `emacs <files>`   (or other editor) |
|                                               | `git commit -a` |
| **Build and test:**                           | `cd <build-dir>; make ; ctest` |
| ... Iterate above ...                         | |
| **Examine state of local branch:**            | `git status` |
|                                               | `git log --name-status @{u}..HEAD` |
| **Final cleanup (optional)**                  | `git rebase -i @{u}` |
| **Test changes locally:**                     | `git pull` (from `origin/<branch>`) |
|                                               | ... Test changes (i.e. using checkin-test-sems.sh) ... |
| **Push changes to your fork:**                | `git checkout develop`
|                                               | `git pull --rebase` (from `origin/develop`) |
|                                               | `git checkout cool-new-feature-name |
|                                               | `git rebase master`
|                                               | `git push cool-new-feature-name`  (to your push-url) |
| **Issue a pull request on the github site:**  | from your_fork:cool-new-feature-name to trilinos:develop |
| **Request reviews and/or assign someone:**    | |
| **After Review and testing:**                 | Rebase and Merge the request |

<a name="rebase"/>

The key to keeping a nice linear history is the `--rebase` argument in the `git pull --rebase` commands.  Such a rebase is always safe if these commits are created locally and were not shared with other repos or developers (which is the typical SVN workflow depicted above).  Similarly, pull requests to origin/develop can usually be rebased to maintain a linear history in that branch. The initial `git pull --rebase` is a good idea to avoid lots of merge commits from `origin/develop` that clutter up local history.  The optional `git rebase -i @{u}` command is very useful for cleaning up commits (i.e. amending, squashing, rearranging) before finally publishing them (see [Interactive Rebase](https://robots.thoughtbot.com/git-interactive-rebase-squash-amend-rewriting-history)).

<a name="git_rerere"/>

To avoid having to address the same merge conflicts more than once on the subsequent rebase, then enable “git rerere” using:

```
$ git config --global rerere.enabled 1
```

If you turn on git rerere before you address merge conflicts the first time, then git will remember these conflict resolutions and take care of that automatically in all future rebase commands in that local git repo.  (NOTE: git rerere should have been enabled as part of your [initial git setup](https://github.com/trilinos/Trilinos/wiki/VC-%7C-Initial-Git-Setup).)

If those references don’t make sense, please don’t give up.  Take a look at [ways to avoid merge commits in git](http://kernowsoul.com/blog/2012/06/20/4-ways-to-avoid-merge-commits-in-git/) (but don’t set up for auto rebase, that will come to bite you later!).  Or look at [this StackOverflow Question](http://stackoverflow.com/questions/25614345/rebase-onto-upstream-changes-with-non-trivial-merge-commits-present-locally).

## Justification

Many beginner developers new to git use a simple git workflow (e.g. where `<branch>` is 'develop' or 'master') 'like:

```
[(<branch>]$ cd Trilinos/   # Old version of '<branch>' :-)
             … change files, do testing, etc. …
[(<branch>]$ git commit -a
[(<branch>]$ git push   # Fails because someone pushed to origin/<branch> since your last pull
[(<branch>]$ git pull   # Creates a trivial merge commit!
[(<branch>]$ git push   # Pushes messy trivial merge commit to origin/<branch> :-(
```

The problem with the above process, is that the later `git pull` creates what is often called a trivial merge commit and forces all testing onto the developer.  Such trivial merge commits make the git history difficult to look at and follow and the mess up the nice linear history that you get using a tool like Subversion (SVN).  (As a result, many development projects that use git will not even let developers push these trivial commits.  For example, the push hooks for the SIERRA git repos do not allow them.  Also, PETSc does not allow them.)

Another problem with these trivial merge commits is that GitHub push emails show all of the changed files associated with these trivial merge commits.  This results in getting a bunch of false matches for custom email filters for the GitHub push emails.  For example, many Trilinos developers have an Outlook email filter that only shows emails that contain package names such as "teuchos", "thyra", "kokkos" and "tpetra".  But these people get spammed because of these trivial merge commits involving commits that have nothing to do with these keywords.  Therefore, routinely using trivial merge commits makes these git email filters almost worthless.
