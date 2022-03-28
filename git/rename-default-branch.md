# Rename Default Branch



Rename `master` to `main`.

If you have a local clone, update it by running the following commands.

```
$ git branch -m master main
$ git fetch origin
$ git branch -u origin/main main
$ git remote set-head origin -a
```