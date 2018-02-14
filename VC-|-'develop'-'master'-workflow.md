**Table of Contents:**
* [Overview](#overview)
* [Transitioning to a feature-branch/develop/master workflow](#transition_feature_develop_master)
* [Moving local changes to the 'develop' branch](#move_to_develop)

<a name="overview"/>
# Overview

The Trilinos project uses a long-lived branch called 'develop' to conduct basic development.  All Trilinos developers directly pull from and request pulls to the shared github 'develop' branch.  Then, the 'master' branch is updated from the 'develop' branch when it passes a specific set of builds for a specific set of packages (the set of packages and builds will evolve over time).  This is depicted in the below figure:

![Trilinos Git 'develop'/'master' workflow](https://github.com/trilinos/trilinos_wiki_images/blob/master/GitDevelopMasterWorkflow.png)

The motivation for the usage of a 'develop' branch and the full set of mechanics and implications are described in [Addition of a 'develop' branch](https://docs.google.com/document/d/1uVQYI2cmNx09fDkHDA136yqDTqayhxqfvjFiuUue7wo/edit#heading=h.u2ougk1wk7ph).

(NOTE: The "bug-fix" workflow elements shown in the above figure are not described below but are described in great detail in the [above reference](https://docs.google.com/document/d/1uVQYI2cmNx09fDkHDA136yqDTqayhxqfvjFiuUue7wo/edit#heading=h.u2ougk1wk7ph).  If things go well, then these "bug fix" commits should almost never be needed.  But in the rare cases these "bug-fix" commits or revert commits are needed/desired, then more experienced Trilinos git developers can help make those changes.)

<a name="transition_feature_develop_master"/>
# Transitioning to a feature-branch/develop/master workflow

For developers accustomed to the previous single branch Trilinos development model (the [Simple Centralized Workflow](https://github.com/trilinos/Trilinos/wiki/VC-|-Simple-Centralized-Workflow)), the transition to the feature-branch/develop/master workflow is not complicated. In essence, you make changes to the feature branch, rather than the develop or master branch. A few extra details, and commands for doing this are below. These commands will be needed every time the Trilinos repository is cloned.

If the local repo is clean (i.e. no local modifications or untracked files), then one just needs to get on a local 'develop' tracking branch.  If the local 'develop' tracking branch has not already been created, then create it using:

```
$ cd Trilinos/
[ (master)]$ git fetch origin
[ (master)]$ git checkout --track origin/develop
[(develop)]$ 
```

If the local 'develop' tracking branch was already created (can be seen by running `git branch`), then just check out that branch using:

```
$ cd Trilinos/
[ (master)]$ git checkout develop
[(develop)]$ 
```

Once on the local 'develop' branch which is tracking the remote 'origin/develop' branch, simply create a local branch for your new development. changes from develop or your local feature branch can be freely pushed to your fork on github to share with other developers or to simply preserve the code in case of local failuure. When all is ready to integrate simply issue a pull request on github from your feature branch to trilinos:develop. See [feature branch workflow](https://github.com/trilinos/Trilinos/wiki/VC-%7C-Simple-Centralized-Workflow).

NOTE: The [checkin-test-sems.sh](https://github.com/trilinos/Trilinos/wiki/Policies-%7C-Safe-Checkin-Testing) script can be used on the 'develop' branch to locally test modifications.

<a name="move_to_feature_branch"/>
# Move local changes to a feature branch

The below process applies only to situations where a Trilinos developer has made changes to the local git repo's 'master' or 'develop' branch and needs to transfer the changes to their local feature branch, where they can be pushed to their github fork. This will be common only during the initial transition to the feature-branch workflow.

Before getting started, Trilinos developers can optionally set up their local account to use the git usability scripts git-prompt.sh and git-completion.bash and enable git `rerere` as described [here](https://github.com/trilinos/Trilinos/wiki/VC-%7C-Initial-Git-Setup).  The shell scripts will make it obvious what local branch a developer is on and git `rerere` will help with the rebasing and merging commands required to transition local commits.

The below process assumes:
* Changes to be moved to the feature branch have been committed to the local 'master' branch, but not pushed.
* All of the local commits to 'master' are to be moved to the feature branch.

The process to transition local changes not on a feature branch to the feature branch are:

**1) Checkout and update the local tracking 'develop' branch**

If you are not already on the local 'develop' branch, this step is described [above](#transition_develop_master).

Next, pull to update 'develop'.

```
[(develop)]$ git pull   # from origin/develop
[(develop)]$ git push   # to your_fork/develop
[(develop)]$
```

And set up a feature branch
```
[(develop)]$ git checkout -b new-feature-branch   # from origin/develop
[(new-feature-branch)]$
```

**2) Merge changes from the local 'master' or 'develop' branch to the local feature branch**

```
[(new-feature-branch)]$ git merge master   # resolve any merge conflicts
```

**3) Test and push changes**

At this point, the commits merged from the local 'master' branch should be tested prior to pushing to the remote github 'develop' branch.  This can all be done using the checkin-test.py script. The necessary manual commands are:

```
... Test changes ...
[(new-feature-branch)]$ git pull --rebase                      # update origin/develop
[(new-feature-branch)]$ git rebase origin/develop              # rebase the feature-branch onto origin/develop
[(new-feature-branch)]$ git push origin new-feature-branch     # to your_fork/new-feature-branch
```

NOTE: If git rerere is enabled, then any merge conflicts that have already been resolved will be resolved automatically on the final rebase before the final push.

**4) Reset the local 'develop' branch**

If one will later want to go back to the local 'develop' branch, then it is a good idea to reset it to the 'origin/develop' branch  **after** the locally updated feature branch has been merged into the 'develop' branch as described above.  This reset is performed as:

```
[(develop)]$ git checkout develop
[(develop)]$ git pull
```
