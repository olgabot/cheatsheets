# Git is stupid !!! Let's create some conflict and try to resolve it. 

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

***
# Chapter  
## Section
### subsection
#### subsubsection

##### subsubsubsection
###### subsubsubsubsection
