# Packaging octave-stk 2.5.0

## 2018-Feb-24, importing from upstream

Trying from scratch:

```
$ git clone git@salsa.debian.org:pkg-octave-team/octave-stk.git
Cloning into 'octave-stk'...
remote: Counting objects: 1207, done.
remote: Compressing objects: 100% (462/462), done.
remote: Total 1207 (delta 733), reused 1172 (delta 717)
Receiving objects: 100% (1207/1207), 669.25 KiB | 699.00 KiB/s, done.
Resolving deltas: 100% (733/733), done.

$ cd octave-stk 

$ gbp import-orig --uscan --pristine-tar
gbp:error: 
Repository does not have branch 'upstream' for upstream sources. If there is none see
file:///usr/share/doc/git-buildpackage/manual-html/gbp.import.html#GBP.IMPORT.CONVERT
on howto create it otherwise use --upstream-branch to specify it.
```

I don't know why there is no `upstream` branch in my clone of the repo, but this seems to fix it:
```
$ git checkout upstream
$ git checkout master
```

and then:
```
$ gbp import-orig --uscan --pristine-tar
gbp:info: Launching uscan...
uscan: Newest version of octave-stk on remote site is 2.5.0, local version is 2.4.2
uscan:    => Newer package available from
      https://qa.debian.org/watch/sf.php/octave/stk-2.5.0.tar.gz
gbp:info: using ../octave-stk_2.5.0.orig.tar.gz
What is the upstream version? [2.5.0] 
gbp:info: Importing '../octave-stk_2.5.0.orig.tar.gz' to branch 'upstream'...
gbp:info: Source package is octave-stk
gbp:info: Upstream version is 2.5.0
Branch pristine-tar set up to track remote branch pristine-tar from origin.
gbp:info: Merging to 'master'
gbp:info: Successfully imported version 2.5.0 of ../octave-stk_2.5.0.orig.tar.gz
```

The result looks OK in `gitk --all`... let's push:
```
$ git checkout upstream
Switched to branch 'upstream'
Your branch is ahead of 'origin/upstream' by 1 commit.
  (use "git push" to publish your local commits)
  
$ git push --set-upstream --follow-tags origin upstream
Counting objects: 190, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (188/188), done.
Writing objects: 100% (190/190), 119.70 KiB | 0 bytes/s, done.
Total 190 (delta 132), reused 0 (delta 0)
remote: Resolving deltas: 100% (132/132), completed with 91 local objects.
remote: 
remote: To create a merge request for upstream, visit:
remote:   https://salsa.debian.org/pkg-octave-team/octave-stk/merge_requests/new?merge_request%5Bsource_branch%5D=upstream
remote: 
To salsa.debian.org:pkg-octave-team/octave-stk.git
   c6dc46b..f2e4be1  upstream -> upstream
 * [new tag]         upstream/2.5.0 -> upstream/2.5.0
Branch upstream set up to track remote branch upstream from origin.

$ git checkout pristine-tar
Switched to branch 'pristine-tar'
Your branch is ahead of 'origin/pristine-tar' by 1 commit.
  (use "git push" to publish your local commits)

$ git push --set-upstream origin pristine-tar
Counting objects: 4, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (4/4), 11.87 KiB | 0 bytes/s, done.
Total 4 (delta 1), reused 0 (delta 0)
remote: 
remote: To create a merge request for pristine-tar, visit:
remote:   https://salsa.debian.org/pkg-octave-team/octave-stk/merge_requests/new?merge_request%5Bsource_branch%5D=pristine-tar
remote: 
To salsa.debian.org:pkg-octave-team/octave-stk.git
   b63eb77..12dda39  pristine-tar -> pristine-tar
Branch pristine-tar set up to track remote branch pristine-tar from origin.

$ git checkout master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 2 commits.
  (use "git push" to publish your local commits)

$ git push
Counting objects: 2, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (2/2), 355 bytes | 0 bytes/s, done.
Total 2 (delta 1), reused 0 (delta 0)
To salsa.debian.org:pkg-octave-team/octave-stk.git
   dab04d7..5e32dcc  master -> master
```

## 2018-Feb-24, importing from upstream (bis)

Recording for next time: I should have used
```
gbp clone git@salsa.debian.org:pkg-octave-team/octave-stk.git
```
to clone the repo, instead of `git clone`.  This would have set up the `upstream` branch properly.  Also,
```
git push --all --follow-tags
```
would have been a more efficient way to push everything at the end.

Thanks to [Rafael Laboissière](https://lists.debian.org/debian-octave/2018/02/msg00033.html) et [Mike Miller](https://lists.debian.org/debian-octave/2018/02/msg00035.html) for their advices.


## 2018-Feb-24, updating copyright information

The next step was to update the copyright information in `debian/copyright`.

Since I was looking for changes occuring in 2017, I used the following:
```
$ licensecheck -r --copyright -c  '\.m|\.c|\.h' . | grep "Copy.*2017" | sort -u
  [Copyright: 2015-2017 CentraleSupelec]
  [Copyright: 2015, 2017 CentraleSupelec]
  [Copyright: 2015-2017 CentraleSupelec / 2011-2014 SUPELEC]
  [Copyright: 2015-2017 CentraleSupelec / 2012-2013 SUPELEC]
  [Copyright: 2015, 2017 CentraleSupelec / 2012-2014 SUPELEC]
  [Copyright: 2015-2017 CentraleSupelec / 2013-2014 SUPELEC]
  [Copyright: 2015, 2017 CentraleSupelec / 2013-2014 SUPELEC]
  [Copyright: 2015, 2017 CentraleSupelec / 2013 SUPELEC]
  [Copyright: 2015-2017 CentraleSupelec / 2014 SUPELEC]
  [Copyright: 2015, 2017 CentraleSupelec / 2014 SUPELEC]
  [Copyright: 2015-2017 CentraleSupelec / 2014 SUPELEC & A. Ravisankar]
  [Copyright: 2015-2017 CentraleSupelec / 2014 SUPELEC & A. Ravisankar / 2011-2013 SUPELEC]
  [Copyright: 2015-2017 CentraleSupelec / 2014 SUPELEC & A. Ravisankar / 2013 SUPELEC]
  [Copyright: 2016-2017 CentraleSupelec]
  [Copyright: 2016-2017 CentraleSupelec / 2012-2014 SUPELEC]
  [Copyright: 2016-2017 CentraleSupelec / 2013-2014 SUPELEC]
  [Copyright: 2016-2017 CentraleSupelec / 2013 Valentin Resseguier / 2012-2014 SUPELEC]
  [Copyright: 2016-2017 CentraleSupelec / 2014 SUPELEC]
  [Copyright: 2016 EDF R&D / 2015-2017 CentraleSupelec / 2013-2014 SUPELEC]
  [Copyright: 2017 Centrale]
  [Copyright: 2017 CentraleSupelec]
  [Copyright: 2017 CentraleSupelec / 2011-2014 SUPELEC]
  [Copyright: 2017 CentraleSupelec / 2012-2013 SUPELEC]
  [Copyright: 2017 CentraleSupelec / 2012-2014 SUPELEC]
  [Copyright: 2017 CentraleSupelec / 2012 SUPELEC]
  [Copyright: 2017 CentraleSupelec / 2013-2014 SUPELEC]
  [Copyright: 2017 CentraleSupelec / 2013 SUPELEC]
  [Copyright: 2017 CentraleSupelec / 2014 Supelec]
  [Copyright: 2017 CentraleSupelec / 2014 SUPELEC]
  [Copyright: 2017 CentraleSupelec / 2014 SUPELEC / 2013 Alexandra Krauth, Elham Rahali & SUPELEC]
  [Copyright: 2017 CentraleSupelec / 2015 CentraleSupelec & LNE / 2011-2014 SUPELEC]
  [Copyright: 2017 CentraleSupelec / 2016 IRT SystemX]
  [Copyright: 2017 CentraleSupelec & LNE]
  [Copyright: 2017 LNE / 2017 CentraleSupelec]
  [Copyright: bastien Villemot <sebastien@debian.org> Fri, 13 Oct 2017 19:38:10 +0200]
  [Copyright: EDF R&D / 2016-2017 CentraleSupelec]
```

Comparing with the current `debian/control`, I noticed that the copyright information needed to be updated for LNE and EDF R&D, which resulted in the following changeset: [73c3cda0](https://salsa.debian.org/pkg-octave-team/octave-stk/commit/73c3cda0edfdb19003dfaf129cd7ccda9726d920).

## 2018-Feb-24, checking patches

The next step is to check the patches in `debian/patches/`.

There are currently three of them:
```
$ cat debian/patches/series 
0001-Remove-stk_config_testprivatemex.patch
0002-Remove-the-MOLE.patch
0003-Mark-expected-failure.patch
```

### First patch: `0001-Remove-stk_config_testprivatemex.patch`

The first patch is outdated:
```
$ dquilt push
Applying patch 0001-Remove-stk_config_testprivatemex.patch
patching file inst/stk_init.m
Hunk #1 FAILED at 228.
1 out of 1 hunk FAILED -- rejects in file inst/stk_init.m
Patch 0001-Remove-stk_config_testprivatemex.patch can be reverse-applied
```
The reason is that the piece of code that was removed by this patch no longer exists in STK 2.5.0 (see upstream changeset [acb746f5be77](https://sourceforge.net/p/kriging/hg/ci/acb746f5be77d5eef773b67916db1358eba8824a/). The patch is thus obsolete, and can be removed:
```
$ git rm debian/patches/0001-Remove-stk_config_testprivatemex.patch 
$ emacs debian/patches/series  ## -> remove corresponding line
$ git add debian/patches/series 
$ git commit -m "d/patches: Remove patch 0001, no longer needed"
```

### Second patch: `0002-Remove-the-MOLE.patch`

The second patch is outdated too:
```
$ dquilt push
Applying patch 0002-Remove-the-MOLE.patch
patching file inst/misc/mole/linsolve/linsolve.m
patching file inst/misc/parallel/stk_parallel_start.m
Hunk #1 FAILED at 33.
1 out of 1 hunk FAILED -- rejects in file inst/misc/parallel/stk_parallel_start.m
patching file inst/stk_init.m
Hunk #2 FAILED at 124.
Hunk #3 FAILED at 144.
Hunk #4 succeeded at 178 (offset -2 lines).
Hunk #5 FAILED at 286.
3 out of 5 hunks FAILED -- rejects in file inst/stk_init.m
patching file inst/misc/optim/@stk_optim_octavesqp/stk_optim_octavesqp.m
Hunk #1 FAILED at 69.
1 out of 1 hunk FAILED -- rejects in file inst/misc/optim/@stk_optim_octavesqp/stk_optim_octavesqp.m
patching file inst/misc/mole/isrow/isrow.m
patching file inst/misc/mole/quantile/quantile.m
Hunk #1 FAILED at 1.
Not deleting file inst/misc/mole/quantile/quantile.m as content differs from patch
1 out of 1 hunk FAILED -- rejects in file inst/misc/mole/quantile/quantile.m
The next patch would delete the file inst/misc/mole/isoctave/isoctave.m,
which does not exist!  Applying it anyway.
patching file inst/misc/mole/isoctave/isoctave.m
Hunk #1 FAILED at 1.
1 out of 1 hunk FAILED -- rejects in file inst/misc/mole/isoctave/isoctave.m
patching file inst/misc/mole/graphics_toolkit/graphics_toolkit.m
Hunk #1 FAILED at 1.
Not deleting file inst/misc/mole/graphics_toolkit/graphics_toolkit.m as content differs from patch
1 out of 1 hunk FAILED -- rejects in file inst/misc/mole/graphics_toolkit/graphics_toolkit.m
patching file inst/misc/options/stk_options_set.m
Hunk #1 FAILED at 123.
1 out of 1 hunk FAILED -- rejects in file inst/misc/options/stk_options_set.m
patching file post_install.m
Patch 0002-Remove-the-MOLE.patch does not apply (enforce with -f)
```

## 2018-Feb-25, still checking patches...

### Second patch: `0002-Remove-the-MOLE.patch`

The reason why this patch has become outdated is, for the most part, that the function `isoctave` has been removed from the upstream package.  A few adaptations are thus needed...

Here is the resulting commit on salsa: [c75c2aa4](https://salsa.debian.org/pkg-octave-team/octave-stk/commit/c75c2aa41a23f32ce17236751143f9d9dd9c9ee2).

### Third (and last) patch: `0003-Mark-expected-failure.patch`

The last patch also needs to be updated...
```
$ dquilt push
Applying patch 0003-Mark-expected-failure.patch
can't find file to patch at input line 11
Perhaps you used the wrong -p or --strip option?
The text leading up to this was:
--------------------------
|Description: Mark one unit test as expected failure
|Author: Julien Bect <julien.bect@centralesupelec.fr>
|Bug-Debian: https://bugs.debian.org/876777
|Forwarded: no
|Reviewed-By: Sébastien Villemot <sebastien@debian.org>
|Last-Update: 2017-10-13
|---
|This patch header follows DEP-3: http://dep.debian.net/deps/dep3/
|--- a/inst/paramestim/stk_param_init.m
|+++ b/inst/paramestim/stk_param_init.m
--------------------------
No file to patch.  Skipping patch.
1 out of 1 hunk ignored
```

But this one is easy: the target file has just been moved to a different location :wink:

Here is the resulting changset: [9055352f ](https://salsa.debian.org/pkg-octave-team/octave-stk/commit/9055352ff0e888030d4feff34cc6dd516023223f).

### Generating the ChangeLog entry

This part is easy:
```
$ gbp dch -aR
gbp:info: Found tag for topmost changelog version '80784669b495f7d7594d3eacd4e30ae3b49c8b50'
gbp:info: Continuing from commit '80784669b495f7d7594d3eacd4e30ae3b49c8b50'

$ git commit -m "Changelog entry for version 2.5.0-1

Gbp-Dch: Ignore"

$ git push
```

### Testing with cowbuilder

Let us first try to build the package in amd64/unstable:
```
$ sudo -E cowbuilder --create --basepath /var/cache/pbuilder/base-sid-amd64.cow --architecture amd64 --distribution sid
#=== ... blah blah ... very verbose output ...

$ gbp buildpackage --git-export-dir=../build --git-pbuilder --git-arch=amd64 --git-dist=sid
gbp:info: Building with (cowbuilder) for sid:amd64
gbp:info: Exporting 'HEAD' to '/home/bect/repo/gitlab-debian/pkg-octave-team/build/octave-stk-tmp'
gbp:info: Moving '/home/bect/repo/gitlab-debian/pkg-octave-team/build/octave-stk-tmp' to '/home/bect/repo/gitlab-debian/pkg-octave-team/build/octave-stk-2.5.0'
Building with cowbuilder for distribution sid, architecture amd64
+ pdebuild --buildresult ../ --pbuilder cowbuilder --debbuildopts '' -- --architecture amd64 --basepath /var/cache/pbuilder/base-sid-amd64.cow
W: /home/bect/.pbuilderrc does not exist
I: using cowbuilder as pbuilder
dpkg-parsechangelog: warning:     debian/changelog(l13): badly formatted trailer line
LINE:  -- Julien Bect <julien.bect@centralesupelec.fr> Mon, 26 Feb 2018 09:08:11 +0100
#=== stopping here to fix the warning
```

Ooops.  This first test reveals that I have made a formatting mistake (missing white space) when manually editing the trailer line in the changelog.

This is fixed in the following changeset: [fe7021b2](https://salsa.debian.org/pkg-octave-team/octave-stk/commit/fe7021b272a7fa873ff5fa5a07d75a1127372005).

Let's try again:
```
$ gbp buildpackage --git-export-dir=../build --git-pbuilder --git-arch=amd64 --git-dist=sid
gbp:info: Building with (cowbuilder) for sid:amd64
gbp:info: Exporting 'HEAD' to '/home/bect/repo/gitlab-debian/pkg-octave-team/build/octave-stk-tmp'
gbp:info: Moving '/home/bect/repo/gitlab-debian/pkg-octave-team/build/octave-stk-tmp' to '/home/bect/repo/gitlab-debian/pkg-octave-team/build/octave-stk-2.5.0'
Building with cowbuilder for distribution sid, architecture amd64
+ pdebuild --buildresult ../ --pbuilder cowbuilder --debbuildopts '' -- --architecture amd64 --basepath /var/cache/pbuilder/base-sid-amd64.cow
W: /home/bect/.pbuilderrc does not exist
I: using cowbuilder as pbuilder
dpkg-checkbuilddeps: error: Unmet build dependencies: debhelper (>= 11) dh-octave
W: Unmet build-dependency in source
dpkg-source: info: applying 0002-Remove-the-MOLE.patch
dpkg-source: info: applying 0003-Mark-expected-failure.patch
dh clean --buildsystem=octave --with=octave
dh: unable to load addon octave: Can't locate Debian/Debhelper/Sequence/octave.pm in @INC (you may need to install the Debian::Debhelper::Sequence::octave module) (@INC contains: /etc/perl /usr/local/lib/x86_64-linux-gnu/perl/5.24.1 /usr/local/share/perl/5.24.1 /usr/lib/x86_64-linux-gnu/perl5/5.24 /usr/share/perl5 /usr/lib/x86_64-linux-gnu/perl/5.24 /usr/share/perl/5.24 /usr/local/lib/site_perl /usr/lib/x86_64-linux-gnu/perl-base) at (eval 15) line 2.
BEGIN failed--compilation aborted at (eval 15) line 2.

debian/rules:5: recipe for target 'clean' failed
make: *** [clean] Error 2
gbp:error: 'git-pbuilder' failed: it exited with 2
```
WTF ?!?  Apparently the package dh-octave is needed but not found. This seems to be something new related to the recent changes operated by the DoG. What is the proper way to get the missing package inside my pbuilder environment ?

### Testing with cowbuilder (bis)

Let us try with a base pbuilder image:
```
$ sudo cowbuilder --create
#=== ... blah blah ... very verbose output ...

$ gbp buildpackage --git-export-dir=../build --git-pbuilder
gbp:info: Building with (cowbuilder) for sid
gbp:info: Exporting 'HEAD' to '/home/bect/repo/gitlab-debian/pkg-octave-team/build/octave-stk-tmp'
gbp:info: Moving '/home/bect/repo/gitlab-debian/pkg-octave-team/build/octave-stk-tmp' to '/home/bect/repo/gitlab-debian/pkg-octave-team/build/octave-stk-2.5.0'
Building with cowbuilder for distribution sid
+ pdebuild --buildresult ../ --pbuilder cowbuilder --debbuildopts '' -- --basepath /var/cache/pbuilder/base.cow
W: /home/bect/.pbuilderrc does not exist
I: using cowbuilder as pbuilder
dpkg-checkbuilddeps: error: Unmet build dependencies: debhelper (>= 11) dh-octave
W: Unmet build-dependency in source
dpkg-source: info: applying 0002-Remove-the-MOLE.patch
dpkg-source: info: applying 0003-Mark-expected-failure.patch
dh clean --buildsystem=octave --with=octave
dh: unable to load addon octave: Can't locate Debian/Debhelper/Sequence/octave.pm in @INC (you may need to install the Debian::Debhelper::Sequence::octave module) (@INC contains: /etc/perl /usr/local/lib/x86_64-linux-gnu/perl/5.24.1 /usr/local/share/perl/5.24.1 /usr/lib/x86_64-linux-gnu/perl5/5.24 /usr/share/perl5 /usr/lib/x86_64-linux-gnu/perl/5.24 /usr/share/perl/5.24 /usr/local/lib/site_perl /usr/lib/x86_64-linux-gnu/perl-base) at (eval 15) line 2.
BEGIN failed--compilation aborted at (eval 15) line 2.

debian/rules:5: recipe for target 'clean' failed
make: *** [clean] Error 2
gbp:error: 'git-pbuilder' failed: it exited with 2
```

Same problem... Let's try to install manually the missing packages:
```
$ sudo cowbuilder login --save-after-login
>> apt-get install debhelper dh-octave
>> exit

# Checking installed versions
$ sudo cowbuilder login --save-after-login
>> dpkg --list debhelper dh-octave
Desired=Unknown/Install/Remove/Purge/Hold
| Status=Not/Inst/Conf-files/Unpacked/halF-conf/Half-inst/trig-aWait/Trig-pend
|/ Err?=(none)/Reinst-required (Status,Err: uppercase=bad)
||/ Name                                                  Version                         Architecture                    Description
+++-=====================================================-===============================-===============================-================================================================================================================
ii  debhelper                                             11.1.5                          all                             helper programs for debian/rules
ii  dh-octave    

$ gbp buildpackage --git-export-dir=../build --git-pbuilder
gbp:info: Building with (cowbuilder) for sid
gbp:info: Exporting 'HEAD' to '/home/bect/repo/gitlab-debian/pkg-octave-team/build/octave-stk-tmp'
gbp:info: Moving '/home/bect/repo/gitlab-debian/pkg-octave-team/build/octave-stk-tmp' to '/home/bect/repo/gitlab-debian/pkg-octave-team/build/octave-stk-2.5.0'
Building with cowbuilder for distribution sid
+ pdebuild --buildresult ../ --pbuilder cowbuilder --debbuildopts '' -- --basepath /var/cache/pbuilder/base.cow
W: /home/bect/.pbuilderrc does not exist
I: using cowbuilder as pbuilder
dpkg-checkbuilddeps: error: Unmet build dependencies: debhelper (>= 11) dh-octave
W: Unmet build-dependency in source
dpkg-source: info: applying 0002-Remove-the-MOLE.patch
dpkg-source: info: applying 0003-Mark-expected-failure.patch
dh clean --buildsystem=octave --with=octave
dh: unable to load addon octave: Can't locate Debian/Debhelper/Sequence/octave.pm in @INC (you may need to install the Debian::Debhelper::Sequence::octave module) (@INC contains: /etc/perl /usr/local/lib/x86_64-linux-gnu/perl/5.24.1 /usr/local/share/perl/5.24.1 /usr/lib/x86_64-linux-gnu/perl5/5.24 /usr/share/perl5 /usr/lib/x86_64-linux-gnu/perl/5.24 /usr/share/perl/5.24 /usr/local/lib/site_perl /usr/lib/x86_64-linux-gnu/perl-base) at (eval 15) line 2.
BEGIN failed--compilation aborted at (eval 15) line 2.

debian/rules:5: recipe for target 'clean' failed
make: *** [clean] Error 2
gbp:error: 'git-pbuilder' failed: it exited with 2
```

Still the same...

### 27-Feb-2018, Epilogue

I finally gave up using cowbuilder or sbuild, after a few more unsuccessful attempts (see: [mailing list thread](https://lists.debian.org/debian-octave/2018/02/msg00039.html)).

Anyway, octave-stk 2.5.0-1 has now been [accepted into unstable](https://tracker.debian.org/news/936641) :smiley:.

Next time: simply test in a [sid chroot](https://wiki.debian.org/chroot) using `gbp buildpackage` and let my sponsor worry about build dependencies...
