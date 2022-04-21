# Delete Commits from History



To prune the old commits. This makes the old commits and their objects unreachable.

```
$ git fetch --depth=1
```

To expire all old commits and their objects.

```
git reflog expire --expire-unreachable=now --all
```

To remove the old objects

```
git gc --aggressive --prune=all
```

