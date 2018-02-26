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

Thanks to [Raphaël Laboissière](https://lists.debian.org/debian-octave/2018/02/msg00033.html) et [Mike Miller](https://lists.debian.org/debian-octave/2018/02/msg00035.html) for their advices.


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
