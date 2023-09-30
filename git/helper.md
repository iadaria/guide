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
