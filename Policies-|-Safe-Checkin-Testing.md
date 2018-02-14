## Overview

In order to maintain the stability of [Primary Tested](http://trac.trilinos.org/wiki/TribitsLifecycleModelOverview#test_categories) packages and code on the 'develop' branch of Trilinos, the checkin-test.py script can be used to test code before any push that changes source code.  A standard environment for running the pre-push CI tests has been set up based on the [SEMS Development Environment](https://github.com/trilinos/Trilinos/wiki/SEMS-Dev-Env) encapsulated in the script [checkin-test-sems.sh](https://github.com/trilinos/Trilinos/blob/develop/cmake/std/sems/checkin-test-sems.sh).  This script makes it easy to run on any machine that has the SEMS Dev Env mounted.  To set up to use this script to test and push changes to Trilinos, do:

```
cd Trilinos/
mkdir CHECKIN/  # Or put this anywhere you want on Linux systems
cd CHECKIN
ln -s ../cmake/std/sems/checkin-test-sems.sh .
``` 

(**WARNING**: The Trilinos git repo must use SSH with the remote URL `git@github.com:trilinos/Trilinos.git` and not HTTPS for access to github.  See [below note](https://github.com/trilinos/Trilinos/wiki/Policies-|-Safe-Checkin-Testing#ssh-vs-https)).

Then when one wants to test changes (and the local git repo is "clean" with no modified or untracked files), one can do:

```
cd Trilinos/CHECKIN/
./checkin-test-sems.sh --do-all
```
DEPRECATED: In the past there was a recommendation to use --push with this script for the develop branch. With the advent of the feature-branch workflow this is deprecated behaviour. The --push flag is still useable for feature branches.

(see default options in [local-checkin-test-defaults.py](https://github.com/trilinos/Trilinos/wiki/Policies-|-Safe-Checkin-Testing#local-checkin-test-defaults.py)).  That will automatically figure out what packages are changed and will enable those packages and all of their downstream packages and [BASIC](https://tribits.org/doc/TribitsDevelopersGuide.html#test-test-category) tests.  Then if everything passes, it will rebase the commits on top of `origin/develop`, amend the top commit message with the test results summary, and push the commits (i.e. it implements the [simple centralized workflow](https://github.com/trilinos/Trilinos/wiki/VC-%7C-Simple-Centralized-Workflow) by default).

## Other workflows

If one is merging and pull-request branch `<some-branch>` from the GitHub fork account `<some-github-id>`, then one can merge the branch locally and run with `--no-rebase` with:

```
git checkout develop
git pull
git remote add <some-github-id> git@github:<some-github-id>/Trilinos.git
git fetch <some-github-id>
git merge --no-ff <some-gitub-id>/<some-branch>
cd CHECKIN/
./checkin-test-sems.sh --do-all --no-rebase --push
```

That will avoid a rebase and leave the merge commit (which will be amended with the test results).  Once the commits are pushed to the 'develop' branch, GitHub will automatically close the associated pull-request issue.

If one is nervous testing and pushing in one shot, then one can break this up into two commands with:

```
./checkin-test-sems.sh --do-all
  <manually inspect what packages got enabled and tested and make sure looks good>
./checkin-test-sems.sh --push
```
(Using the checkin-test-sems.sh script to push is critical in order to mark known-tested commits to allow for [robust usage of git bisect](https://tribits.org/doc/TribitsDevelopersGuide.html#using-git-bisect-with-checkin-test-py-workflows).  Without this, git bisection is very hard to do with Trilinos.  Also, local commits will be rebased by default, keeping a nice linear history on 'develop'.)

If one changes a given Trilinos package and one knows that it only impacts that one package (such as when only modifying tests but no changes to library code), then one can run the build and tests for only the directly modified packages with:

```
./checkin-test-sems.sh --enable-all-packages=off --no-enable-fwd-packages --do-all --push
```

Also, one can test any set of packages `<pkg0>`, `<pkg1>`, etc. (and not downstream packages) with the local git repo in any arbitrary state (i.e. has modified or untracked files, is in a detached-head state, etc.) using:

```
./checkin-test-sems.sh --enable-all-packages=off --enable-packages=<pkg0>,<pkg1>,... \
   --no-enable-fwd-packages --local-do-all
```

One can also run any individual step by replacing `--do-all` and `--local-do-all` with the action commands `--pull`, `--configure`, `--build`, and/or `--test` (the script keep memory of what steps have been performed already).

If the configure, build or any tests fail, then one can easily reproduce them just like for any regular build of Trilinos by sourcing the script [load_ci_sems_dev_env.sh](https://github.com/trilinos/Trilinos/wiki/SEMS-Dev-Env#load_ci_sems_dev_env.sh) and doing the following (in an `sh` compatible shell, **not (t)csh**):

## Reproducing failures manually

```
cd Trilinos/CHECKIN/
source ../cmake/load_ci_sems_dev_env.sh   # NOTE: Does 'module purge' then loads new env!
cd MPI_RELEASE_DEBUG_SHARED_PT/
less configure.out           # View cmake configure output/errors
./do-configure               # Reproduce configure failure
less make.out                # View make build output/errors
make -j<N>                   # Reproduce build failure
less ctest.out               # View ctest output/failures
ctest -j<N> -R <test-name>   # Reproduce failing test(s)
```

(**Warning:** loading the SEMS env with `source ../cmake/load_ci_sems_dev_env.sh` will wipe out your current modules so you might want to open a new bash shell before loading it.)

Many other use cases are also supported.  Some detailed documentation on the checkin-test.py script can be obtained using [checkin-test.py --help](https://tribits.org/doc/TribitsDevelopersGuide.html#checkin-test-py-help).

## NOTES

<a name="ssh-vs-https"/>

* **[ssh-vs-https]** In order for the automated pulls and pushes that the script does to work, the Trilinos git repos must be set up to use SSH to access git instead of HTTPS (i.e. the remote must be `git@github.com:trilinos/Trilinos.git` and **NOT** `https://github.com/trilinos/Trilinos.git`).  See [Connecting to GitHub with SSH](https://help.github.com/articles/connecting-to-github-with-ssh/).

<a name="local-checkin-test-defaults.py"/>

* **[local-checkin-test-defaults.py]** After the first time `checkin-test-sems.sh` is run (e.g. with just `--help`), optionally edit the file `local-checkin-test-defaults.py` to adjust defualt options like `-j<N>` to better match your machine (see comments inside of the script [`checkin-test-sems.sh`](https://github.com/trilinos/Trilinos/blob/develop/cmake/std/sems/checkin-test-sems.sh) itself for how to adjust these default options).

* The `checkin-test-sems.sh` script, by default, runs a single build called `MPI_RELEASE_DEBUG_SHARED_PT` (see CMake options in `MPI_RELEASE_DEBUG_SHARED_PT/do-configure.base`) with the env defined by the source script [load_ci_sems_dev_env.sh](https://github.com/trilinos/Trilinos/blob/develop/cmake/load_ci_sems_dev_env.sh).  This is a build designed to best protect developers and users of Trilinos.  This build allows the enable of any Primary Tested (PT) packages and enables several TPLs provided by the SEMS env (see `Final set of enabled TPLs` in `MPI_RELEASE_DEBUG_SHARED_PT/configure.out`).

* The standard pre-push CI build env is currently chosen to be the Sandia Linux RHEL 6 or 7 COE with the SEMS env.  It is recommended that Trilinos developers test and push from one of these machines, or ask someone else with access to one of these machines to push for you.  (Please contact trilinos-framework at software.sandia.gov if you do not have access and time on one of these systems.  Most Sandia staff can be given lab-support systems for this purpose.).

* One can do development on any machine with any env one wishes and then use a remote SNL Linux RHEL 6 or 7 machine with the SEMS env to do a [remote pull, test, and push](https://github.com/trilinos/Trilinos/wiki/Local-development-with-remote-pull%2C-test%2C-and-push) of the commits.  (Or, one can just manually move the branch from the local repo to an SNL RHEL 6 or 7 machine and run the checkin-test-sems.sh script from there.)

<a name="raw_checkin_test_py"/>

* **[raw_checkin_test_py]** The raw script `Trilinos/checkin-test.py` can still be used for doing local development work and testing or driving any workflow.  It can even be used for pushing (but please prefer to use the `checkin-test-sems.sh` script on a RHEL 6 or 7 machine with the SEMS env when pushing to the main Trilinos 'develop' branch).  However, unless you can define the `MPI_RELEASE_DEBUG_SHARED_PT.config` file to support the full set of PT packages and TPLs, then just disable that build by running with your own pre-defined builds (i.e. creating `<build0>.config`, `<build1>.config`) by passing in `--default-builds= --st-extra-builds=<build0>,<build1>,...`.  (See the checkin-test-sems.sh script for an example of how to do this.)

<a name="disable_already_failing"/>

* **[disable_already_failing]** You can see if the CI build is currently clean by [looking at the CI build on CDash](https://testing.sandia.gov/cdash/index.php?project=Trilinos&filtercount=3&showfilters=1&filtercombine=and&field1=buildname&compare1=66&value1=-MPI_RELEASE_DEBUG_SHARED_PT_CI&field2=groupname&compare2=61&value2=Continuous&field3=buildstarttime&compare3=84&value3=now).  If you see failing tests then you can disable those tests when invoking the checkin-test-sems.sh script by passing in `--extra-cmake-options="-D<failing_test_0>_DISABLE=ON -D<failing_test_1>_DISABLE=ON ..."`.  That will allow you to push by ignoring those failing tests.  If entire packages are failing to build or have catastrophic test failures, then you can disable those using `--disable-packages=<pkg0>,<pkg1>,...`. 

* The checkin-test-sems.sh script is not supported on systems like Mac OSX or Windows.  It only works and has been tested on Linux machines (in particular on RHEL 6 and 7 with the SEMS env at present).

<a name="csh_shell_issues"/>

* **[csh_shell_issues]** Some people have reported module load problems when running the `checkin-test-sems.sh` script directly from a `csh` shell.  Therefore, if there are issues, first open a `bash` shell and then run the script from there.  Also note that one can not manually source the `load_ci_sems_env.sh` script from `csh` but instead must use `bash` (see [above](github.com/trilinos/Trilinos/wiki/Policies-|-Safe-Checkin-Testing#manual_reproduce_bash)).  If one wants to manually load the SEMS modules on a non-`sh` shell (e.g. `(t)csh`) and then reproduce failures , then one will need to manually load the SEMS modules (direct support for `(t)csh` is not provided).