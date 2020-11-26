## Advanced git
1. Brances live in `refs/headers` folder and they are just plain text files with last commit id
2. `git add -p` - interactivly add chunks from file
3. `git reset -p` - unstage non commited files but don't delete changes interactivly
4. `git reset --hard hash` - removes unstaged changes , moves pointer to given commit
5. Merge conflict
    - from <== HEAD to === - our local changes
	- from === to  >>>branch_name - changes from branch we want to merge with

## Git rebase
Master has new commits which are not in feature branch.
We checkout feature and rebase with master. Now git history looks likes this
```
f2
f1
m1
m2
m3
together(when feature branch was created)
```
### Interactive rebase
Let's say I made three commits. Fix,Sorry one more,Sorry forgot to add some files.<br>
All of them can be combined into one commit with this command
`git rebase -i HEAD~3`. 3 here is the amount of commits

