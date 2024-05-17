1 Clear cache

> git rm --cached <filename>
> Example: >git rm --cached .DS_Store

2 Add my settings and revert

> git merge <branch>(with my settings)
> git revert -m 1 <hash>

### delete previous commites

> git reset -hard <commit-hash>
> git push origin --force
> // maybe new branch?

### push all

> vim .git/config
> [remote "all"]
> url = origin-host:path/proj.git
> url = nodester-host:path/proj.git
> git push all master

### searching

exclude the word: "hello NOT word"

### find and delete .DS_Store

> find . -name ".DS_Store" -type f -delete

### can't commit need to continue rebase

If you need current code go to folder .git and delete .git/rebase-merge folder !

> git reflog

> git checkout <branch>

> git rebase --hard HEAD@{1}

> rm -rf .git/rebase-merge

### Deleting the .git folder may cause problems in your git repository. If you want to delete all your commit history but keep the code in its current state, it is very safe to do it as in the following:

Checkout/create orphan branch (this branch won't show in git branch command):

git checkout --orphan latest_branch
Add all the files to the newly created branch:

git add -A
Commit the changes:

git commit -am "commit message"
Delete main (default) branch (this step is permanent):

git branch -D main
Rename the current branch to main:

git branch -m main
Finally, all changes are completed on your local repository, and force update your remote repository:

git push -f origin main
PS: This will not keep your old commit history around. Now you should only see your new commit in the history of your git repository.
