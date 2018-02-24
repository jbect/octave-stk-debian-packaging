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
to clone the repo, instead of `git clone`.  This would have set up the `upstream` branch properly.
