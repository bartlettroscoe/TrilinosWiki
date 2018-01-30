[Overview](#overview)

[Clean Track](#clean-track)

[Nightly Track](#nightly-track)

[Broken Nightly Track](#broken-nightly)

[Specialized Track](#specialized-track)

[Experimental Track](#experimental-track)

[Pull-request Track](#pull-request-track)

[Continuous Track](#continuous-track)

[Release Tracks](#release-tracks)

## <a name="overview"></a>Overview

The Trilinos Project uses the [CDash](https://www.cdash.org) tool to host its [testing dashboard](https://testing.sandia.gov/cdash/index.php?project=Trilinos). The dashboard is divided into a number of _tracks_. Each track has its own policies associated with adding a build to the track, keeping a build on the track, and what can be expected with regards to the builds on the track. While not all of these policies have been implemented, and not all of the expectations have yet been realized, below we discuss the target policies and expectations for builds on each track.

## <a name="clean-track"></a>Clean Track

The Clean Track provides the highest level of stability of any of the Trilinos dashboard tracks. The Clean Track builds correspond to the builds in the Pull-request Track. Clean Track builds are of very high interest to a wide variety of stakeholders.

### Adding a Build

The number of Clean Track builds is intentionally kept small. To request the addition or modification of a Clean Track build, contact the Trilinos Framework Product Leader.

### Maintaining a Build

Failures in clean builds are to be corrected immediately by backing out the commit that caused the failure. Any Trilinos developer is authorized to submit a pull request that backs out the offending commit(s) that caused a failure to a clean track build. This is done to restore the failing clean build(s) to a good state, and recognizes the fact that no pull-request tests involving the failing configure, build, or test will pass until the defect is removed. Once Pull-request testing is made mandatory, Clean Track failures should be very rare, as pull request testing will fail in all cases that Clean Track testing fails, except for a few corner cases.

### Expectations

Customers can expect clean track failures will be of very short duration (less than 1/2 day). Developers can expect that if a commit causes a failure to the Clean Track, that it will be backed out and they will have time to correct and resubmit the commit.

## <a name="nightly-track"></a>Nightly Track

The Nightly Track is a set of builds that the Trilinos Project has agreed to keep clean, but not with the same rigor as the Clean Track. Prior to updating the Trilinos master branch from develop, all of the current Nightly Track builds must be clean (no configure, build, or test errors). Nightly Track builds are either of general interest to a wide variety of Trilinos customers, or important to one or more key customers.

### Adding a Build

To request the addition or modification of a Nightly Track build, contact the Trilinos Framework Product Leader. If the build is of broad importance, the Trilinos Framework Team may assist in the addition of the build as resources permit, if the build is of narrower interest, but is accepted for Nightly Track candidacy, the customer(s) requesting the build will be responsible for following the process of adding the build to the Nightly Track.

The process for adding a Nightly Track build involves first creating a Specialized Track build. Once the Specialized Track build has run for two consecutive days without any configure, build, or test errors, it can be moved to the Nightly Track. Once on the Nightly Track, it must be maintained as described below.

### Maintaining a Build

After one or more failures occurs on a Nightly Track build, the commits leading to the failures should be backed out before the next Nightly Track build is run. If there was no attempt to back out the changes that led to the failures and the build fails a second time for any of the same reasons as the first time, the Trilinos Framework team will move the failing Nightly track build to the Broken Nightly track. To document that an attempt was made to back out the change, a GitHub Issue should be filed. If the Trilinos Framework team sees a second consecutive nightly failure for a build, they will first look for a GitHub Issue for the failure to see if an attempt was made. For example, it would be possible (but unlikely) that someone tried to issue a pull request to back out the change, but another failure prevented the build passing the pull-request testing. Also, it is possible that a commit was reverted in an attempt to fix the failure, but the reverted commit did not fix some or all of the failures. Filing a GitHub issue will also make it clear that the failure is being looked at in case someone else is considering backing out the change. Beginning the first day the build runs on the Broken Nightly track, the build will not be considered as part of the develop to master branch promotion process.

For Nightly Track builds of general interest to Trilinos, it is the responsibility of the Trilinos Framework team to back out changes that cause failures. For customer or package-specific builds, it is the responsibility of the interested party. However, any Trilinos developer is authorized to back out a commit that causes a failure on any Nightly Track build.

**Note:** For the purpose of identifying the "first" and "second" Nightly Track failure dates, only Sandia business days falling Monday-Thursday will be considered. In this context, builds run between Sunday night and Monday are considered Monday builds, as that is when developers would see the results. While other days are not counted in the above policy, developers should still try to revert changes causing Nightly Track failures at their earliest opportunity.

### Expectations

In addition to the expectations stated in the above sections on adding and maintaining builds, developers who push changes that break Nightly Track builds whose commits are subsequently backed out can expect to have some way in which they can test their modified changes on the machine(s) that the failures occurred on. This might mean getting help to run the actual test from the Trilinos Framework team or applicable Customer or package team, but it could also mean being given access to the platform in some way with support for basic questions. Therefore, anytime a Nightly Track build is proposed, there is the expectation that if the build is added to the Nightly Track, Trilinos developers will have direct or indirect access to hardware where failures can be reproduced and dealt with.

Finally, if a Nightly Track build decreases in importance, the expectation is that the build maintainer will demote the build to the Specialized Track, or remove it from the dashboard when appropriate.

## <a name="broken-nightly"></a>Broken Nightly Track

The Broken Nightly track is a track on the dashboard where builds that have failed for two consecutive nights on the Nightly Track are moved. (The Broken Nightly Track uses the same policy for determining which days count for failures as the Nightly Track.) Once a build has been moved to the Broken Nightly track, it no longer is considered in the develop to master promotion decision. However, the maintainers of a Broken Nightly track build still have the authority to back out changes that caused the failures to their builds.  

### Adding a Build

Any member of the Trilinos Framework team can move a build to the Broken Nightly track after 2 consecutive failures on the Nightly Track (provided the failures are not completely different - in other words, the first failure was resolved, and another failure injected that day). A comment should be added to the GitHub issue(s) associated with the failures when the build is moved to the Broken Nightly Track.

### Maintaining a Build

A build can fail on the Broken Nightly Track for up to 6 consecutive days, at which point Trilinos Framework staff can move the build to the Specialized Track. If on any day the errors are completely different than those from the previous day, or there are no failures, the six day clock resets. A comment should be added to the GitHub issue(s) associated with the failures when the build is moved to the Specialized Track. If a build on the Broken Nightly Track is clean for two consecutive days (no configure, build, or test failures), it can be moved back to the Nightly Track. The maintainers of the build can do this, or a request can be made to the Trilinos Framework team to do this.

### Expectations

It is expected that few builds will be moved to the Broken Nightly Track, and that builds that are moved will be returned to the Nightly Track quickly. Nightly Track qualification may be reviewed for builds that frequently fall to the Nightly Broken Track.

## <a name="specialized-track"></a>Specialized Track

The Specialized Track contains builds that are either of narrower interest to Trilinos developers and customers, or are in the process of being cleaned up for eventual promotion to the Nightly or Clean Track. Maintainers of Specialized Track builds do not have the authority to back out changes made by other developers to stabilize their build.

### Adding a Build

Any Trilinos developer may submit a build to the Specialized Track. Permission is not required to add a build to this track.

### Maintaining a Build

Although there are no specific stability requirements for the Specialized Track, if a build is failing chronically in very serious ways, the Trilinos Framework team may strongly suggest that the build be moved to the experimental track or discontinued.

### Expectations

The Specialized Track can be used for builds of any purpose, including just keeping an eye on the status of a porting effort and monitoring known failures. These efforts are generally not allowed on the Clean or Nightly Tracks. However, there is still an expectation that the builds on the Specialized Track be removed when they are no longer relevant. 

## <a name="experimental-track"></a>Experimental Track

### Adding a Build

Any Trilinos developer may submit a build to the Experimental Track. Permission is not required to add a build to this track.

### Maintaining a Build

Although there are no specific stability requirements for the Experimental Track, if a build is no longer relevant, it should be removed to avoid wasting computing resources and bloating the database.

### Expectations

There are no expectations for Experimental Track builds other than removing builds that are no longer relevant.

## <a name="pull-request-track"></a>Pull-request Track

The Pull-request Track contains builds that correspond to specific pull requests issued against the develop branch of the Trilinos GitHub repository. The configurations for the builds are identical to the configurations for the builds on the Clean Track (this is a target currently, not a reality). The name of a Pull-request Track build contains the issue number of the pull request being tested.


### Adding a Build

A Pull-request Track build corresponding to each of the Clean Track builds is created when a pull-request is submitted against the develop branch of Trilinos, the initial inspection requirement for the pull-request is satisfied, and the branch containing the pull-request can be merged cleanly onto the current state of the develop branch.

### Maintaining a Build

The Pull-request Track build configurations change only when Clean Track build configurations change.

### Expectations

When all of the Pull-request builds pass and the pull request is merged into the develop branch, the changes that are merged should very rarely cause the Clean Track builds to fail. One example where it is possible a Clean Track build would fail is if two pull-requests are being tested simultaneously against the same version of develop, and both pass when tested individually but, even though no merge conflicts occur when applying the pull-requests, the combination of both pull-requests being applied causes a failure.

## <a name="continuous-track"></a>Continuous Track

Continuous Track builds run once in the morning, then after every change that is applied to the repository. If, while a Continuous Track build is running, more than one change is applied to the repository, all changes applied since the last continuous build will be tested together in the next continuous run. Currently the builds on the Continuous Track closely resemble Clean Track builds. The role of the Continuous Track builds may evolve to include builds using configurations identical to all of the Clean Track builds, or may simply be updated to be identical to the Clean Track builds the Continuous Track builds currently resemble.

### Adding a Build

To request the addition or modification of a Clean Track build, contact the Trilinos Framework Product Leader.

### Maintaining a Build

Failures in Continuous Track builds are to be corrected immediately by backing out the commit that caused the failure. Any Trilinos developer is authorized to submit a pull request that backs out the offending commit(s) that caused a failure to a Continuous Track build. Once the Continuous Track test configurations are made identical to Clean Track (and therefore Pull-request Track) builds, and Pull-request testing is made mandatory, Continuous Track failures should be very rare, as pull request testing will fail in all cases that Continous Track testing fails, except for a few corner cases.

### Expectations

Customers can expect Continuous Track failures will be of very short duration (less than 1/2 day). Developers can expect that if a commit causes a failure to the Continuous Track, that it will be backed out and they will have time to correct and resubmit the commit.

## <a name="release-tracks"></a>Release Tracks



### Adding a Build


### Maintaining a Build


### Expectations