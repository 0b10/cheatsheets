## Cherry Picking
### A Range of Commits

(Git's version of cut and paste using rebase)

Cherry-pick is not ideal for this.

1. Checkout the tip of the commits you want as a detached header
1. Rebase onto the branch you wish to merge to (pushing the commits to the HEAD of that branch)
1. "Save" the commit range as a new branch
1. Merge that branch into the target branch 

```sh
git checkout <last commit hash> # (detached commit) The last desired commit
git rebase --onto somebranch <first commit> # The hash of the first commit
# You now have a new commit hash
git branch newbranchname <new hash> # Save the commits as a branch
git checkout somebranch
git merge newbranchname # Merge them in
```