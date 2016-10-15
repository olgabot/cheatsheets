# Git

## When you `git pull` and get merge conflicts

```
$ git pull origin master
From github.com:olgabot/REDACTED
 * branch            master     -> FETCH_HEAD
Updating edd5eec..72537f4
error: Your local changes to the following files would be overwritten by merge:
```

### If you *don't* beyonce about local changes

Force getting all the remote data

```
git fetch --all
git reset --hard origin/master
```
### Git Notes
```
To terminate a hanging command =  ctrl-z OR ctrl-c OR escape OR q

mkdir = make a directory

cd = change direction

ls = listing files

.. = this location

git add __________ (specific file name)
git commit -m "MESSAGE ABOUT CHANGES MADE"

git status = what files are currently being staged

git remote -v = which github cloud is this using (tells what places you are looking from, where code originates)

git push = pushes all code onto github

```
### Books/Sites to READ
[Python for Biologists Book](http://pythonforbiologists.com/)

[Programming for Biologists](http://www.programmingforbiologists.org/)

[Git Cheat Sheet](https://services.github.com/kit/downloads/github-git-cheat-sheet.pdf)

![picture](https://i.imgflip.com/b94g3.jpg)
