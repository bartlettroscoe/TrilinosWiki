[Overview](#overview)

[Clean Track](#clean-track)

[Nightly Track](#nightly-track)

[Specialized Track](#specialized-track)

[Experimental Track](#experimental-track)

[Pull-request Track](#pull-request-track)

[Continuous Track](#continuous-track)

[Release Tracks](#release-tracks)

## <a name="overview"></a>Overview

The Trilinos Project uses the [CDash](https://www.cdash.org) tool to host its [testing dashboard](https://testing.sandia.gov/cdash/index.php?project=Trilinos). The dashboard is divided into a number of _tracks_. Each track has its own policies associated with adding a build to the track, keeping a build on the track, and what can be expected with regards to the builds on the track. While not all of these policies have been implemented, and not all of the expectations have yet been realized, below we discuss the target policies and expectations for builds on each track.

## <a name="clean-track"></a>Clean Track

The Clean Track provides the highest level of stability of any of the Trilinos dashboard tracks. The Clean Track builds correspond to the builds in the Pull-request Testing track builds. Clean Track builds are of very high interest to a wide variety of stakeholders.

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


### Expectations

## <a name="specialized-track"></a>Specialized Track

### Adding a Build



### Maintaining a Build


### Expectations

## <a name="experimental-track"></a>Experimental Track

### Adding a Build



### Maintaining a Build


### Expectations

## <a name="pull-request-track"></a>Pull-request Track

### Adding a Build



### Maintaining a Build


### Expectations

## <a name="continuous-track"></a>Continuous Track

### Adding a Build



### Maintaining a Build


### Expectations

## <a name="release-tracks"></a>Release Tracks

### Adding a Build


### Maintaining a Build


### Expectations
