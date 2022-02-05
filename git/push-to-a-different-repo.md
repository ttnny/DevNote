# Detached HEAD



Detached HEAD is a valid state in Git, not a problem.

Branch is a reference pointing to a single commit.

HEAD is a reference pointing to the last commit in the current branch.

- HEAD points to a branch most of the time.

 - When adding a new commit, branch reference is updated and point to it. HEAD reference stays the same pointing to the current-and-updated branch.
 - When changing branches, HEAD is updated pointing to the switching-to branch.



### Getting back to normal state (attached HEAD)

No changes to commit or to discard them all, just check out the branch we were in before.

```
$ git checkout <branch-name>
```

or

```
$git switch <branch-name>
```



With changes to commit, just create and switch to a new branch.

```
$ git branch new-branch-with-changes
$ git checkout new-branch-with-changes
```

HEAD is now pointing to the `new-branch-with-changes` branch which, in turn, pointing to the latest commit that contains the changes.



