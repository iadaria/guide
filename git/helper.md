1 Clear cache
> git rm --cached <filename>
Example: >git rm --cached .DS_Store

2 Add my settings and revert
> git merge <branch>(with my settings)
> git revert -m 1 <hash>