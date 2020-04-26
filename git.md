# Codeneuro Git is neuroscience! and everything is awesome!

## When you `git pull` and get merge conflicts

```
$ git pull origin master
From github.com:olgabot/REDACTED
 * branch            master     -> FETCH_HEAD
Updating edd5eec..72537f4
error: Your local changes to the following files would be overwritten by merge:
```

### If you *don't* care about local changes

Force getting all the remote data

```
git fetch --all
git reset --hard origin/master
```


## Clone a branch to another folder

E.g. when you want to have different Jupyter notebooks using different versions of your code, or if you want to run the tests for a module while you're working on another feature, and the test suite takes a while to run


```
git clone --shared -b v1.0.0rc1 ~/workspace-git/outrigger ~/workspace-git/outrigger-test
```

Now, if you make any changes in `~/workspace-git/outrigger` and you commit them to the right branch:

```
➜  outrigger git:(v1.0.0rc1) ✗ git add outrigger/tests                                   
➜  outrigger git:(v1.0.0rc1) ✗ git commit -m 'update test files with correct cases and filenames'
[v1.0.0rc1 790c098] update test files with correct cases and filenames
 4 files changed, 48 insertions(+), 364 deletions(-)
 delete mode 100644 outrigger/tests/data/tasic2016/outrigger_output/index/junction_exon_direction_triples.csv
 rename outrigger/tests/data/tasic2016/outrigger_output_bam/index/{junction_exon_direction_triples.csv => exon_direction_junction.csv} (100%)
 create mode 100644 outrigger/tests/data/tasic2016/outrigger_output_bam/psi/outrigger_summary.csv
➜  outrigger git:(v1.0.0rc1) ✗ git add outrigger/index/events.py                   
➜  outrigger git:(v1.0.0rc1) ✗ git commit -m 'fix imports'
[v1.0.0rc1 441142d] fix imports
 1 file changed, 2 insertions(+), 3 deletions(-)
➜  outrigger git:(v1.0.0rc1) ✗ git add logo 
➜  outrigger git:(v1.0.0rc1) ✗ git commit -m "add v2 logo even though I don't like it"
[v1.0.0rc1 00d4c40] add v2 logo even though I don't like it
 3 files changed, 3415 deletions(-)
 delete mode 100644 logo/v2/logo_v2-1x.png
 delete mode 100644 logo/v2/logo_v2-300ppi.png
 delete mode 100644 logo/v2/logo_v2.ai
 ```
 
 Then you can `git pull` them in the other folder:
 
 ```
 ➜  outrigger-test git:(v1.0.0rc1) git pull                 
From /Users/olga/workspace-git/outrigger
   43a93c7..00d4c40  v1.0.0rc1  -> origin/v1.0.0rc1
Updating 43a93c7..00d4c40
Fast-forward
 logo/v2/logo_v2-1x.png                                                                       |  Bin 8998 -> 0 bytes
 logo/v2/logo_v2-300ppi.png                                                                   |  Bin 42177 -> 0 bytes
 logo/v2/logo_v2.ai                                                                           | 3415 --------------------------------------------
 outrigger/index/events.py                                                                    |    5 +-
 outrigger/tests/data/tasic2016/intermediate/psi/se/summary.csv                               |   86 +-
 outrigger/tests/data/tasic2016/outrigger_output/index/junction_exon_direction_triples.csv    |  321 -----
 .../index/{junction_exon_direction_triples.csv => exon_direction_junction.csv}               |    0
 outrigger/tests/data/tasic2016/outrigger_output_bam/psi/outrigger_summary.csv                |    5 +
 8 files changed, 50 insertions(+), 3782 deletions(-)
 delete mode 100644 logo/v2/logo_v2-1x.png
 delete mode 100644 logo/v2/logo_v2-300ppi.png
 delete mode 100644 logo/v2/logo_v2.ai
 delete mode 100644 outrigger/tests/data/tasic2016/outrigger_output/index/junction_exon_direction_triples.csv
 rename outrigger/tests/data/tasic2016/outrigger_output_bam/index/{junction_exon_direction_triples.csv => exon_direction_junction.csv} (100%)
 create mode 100644 outrigger/tests/data/tasic2016/outrigger_output_bam/psi/outrigger_summary.csv
 ```


## Undo `git clone --single-branch`

e.g. if you cloned a single branch from a repo with `--single-branch`:

```
git clone --single-branch --branch dev https://github.com/czbiohub/nf-predictorthologs ripgrep
```

But then you need to merge in the changes from dev, here is how to do it (cribbed from https://stackoverflow.com/a/60846265/1628971):


```
git remote set-branches --add origin dev
git fetch origin dev
git merge dev
```
