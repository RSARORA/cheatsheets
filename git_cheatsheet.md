# My git cheatsheet

### Initialize new repo
`git init`

### Add link to remote repo
`git remote add origin <repo-url>`

### Check remote
`git remote -v`

### Replace remote url
`git remote set-url origin <repo-url>`

### Add all changed files to staging area
- #### add all new, modified and deleted files (tracked+untracked) to staging area
  `git add -A`
- #### adds only updated AKA modified+deleted (tracked only) files to staging area
  `git add -u`
- #### acts like git add -A but it will only pick up files from the current directory you are in (check terminal)
  `git add .`

### Move file from staging area to working area
`git reset <file_name>`

### Unstage all files
`git reset`

### Show commit hash, author and commit message along with other details
`git log`

### See file that were changed or added in a commit
`git log --stat`

### Change message in previous commit
`git commit --amend -m "new commit message"`

### Add new file to previous commit
`git add file`
`git commit --amend --no-edit`

### Push to remote
`git push -u origin <branch_name>`

### Reset to a previous commit

- #### remove all commits after commit 852309 and bring all changed code after that in staging area
`git reset --soft 852309`
- #### remove all commits after commit 852309 and bring changed code after that to working area
`git reset --mixed 852309`
`git reset 852309`
- #### remove all commits after commit 852309 and destroy all changed code after that
`git reset --hard 852309`
- #### all untracked files (newly created files) will not be removed
### These commands reset Git history which can be potentially be dangerous

### See what code changes between current state of files and state of files in previous commit
`git diff`

### Check difference in file states between two commits
`git diff hash1 hash2`

### Set state of above file to state of file in HEAD commit (pointer to last commit)
`git checkout -- file_name`

### Checkout all the modified files
`git checkout -- .`

### If you have lots of untracked files or folders in the repository which you want to remove
`git clean -f -d`

### Create new branch
`git checkout -b new_branch`

### Check all local and remote branch
`git branch -a`

### Update your remote-tracking branches
`git fetch`

### If a remote branch already exists with different name that you want to track with current branch
`git branch --set-upstream-to origin/remote_branch`

### Set new remote branch
`git push -u origin new_branch`

### Check if any branches ever merged with current branch
`git branch --merged`

### Merge branch1 into branch2
```
git checkout branch2
git merge branch1
git push
git branch -d branch1 # delete branch
```
### To delete remote new_branch
`git push --delete origin new_branch`

### If we commit to master but did not intend to
1. make new branch
```
git checkout -b dev
```

2. go to master branch and set HEAD to previous commit
```
git checkout master
git reset --hard 1b2ed7
git clean -f -d
```

3. Go to dev branch, push changes to remote dev branch and setup merge request
```
git checkout dev
git add files
git commit -m "commit message"
```

4. Merge to mainline when approved
```
git checkout master
git pull
git merge dev
```

### Move commit from master branch to dev branch
1. Move to dev branch
```
git checkout dev
```
2. Instruct Git to bring 07762b commit without telling from which branch or branches reference it
```
git cherry-pick 07762b
```
3. Reset the HEAD of the master branch to previous commit
```
git checkout master
git reset --hard 1b2ed7b
git clean -f -d
```
4. merge
```
git pull
git merge dev
```
5. Update remote
```
git push
```

### How to correct if you mistakenly reset to a previous commit and made it HEAD.
1. get all operations performed
```
git reflog
```
2. reset to desired HEAD
```
git reset --hard 96452a4
		or
git reset --hard 'HEAD{5}' # or whatever index
```

### How to redo a commit you have pushed
1. create a new commit by reverting all the changes in a commit
```
git revert 7d741e
```
2. verify the correction
```
git diff commit1_hash commit2_hash
```
3. push to remote master
```
git pull
git push
```

### When in a detached HEAD state
- Create new branch out of it
```
git checkout -b new_branch
```

### Switch branches with uncommited work
1. Stash uncommited changes
```
git stash
```
2. Checkout new branch
```
git checkout new_branch
```
3. Get stashed changes and resolve merve conflicts
```
git stash pop (apply and delete)
		or 
git stash apply stash@{0} (apply)
git stash drop stash@{0} (delete)
```

### To clear or clean all entries in stash list
`git stash clean`

### Save stash with a message
`git stash save <message>`

### Abort a merge on git pull
`git merge --abort`

### During merge conflict if you want to reset your work and use remote version
`git reset --merge 2e40a3`

### Rebase will copy all commits from dev branch and put it on top of master branch
`git rebase dev`


### Squash non-consecutive commits. A -> B -> C -> D : squash commit A and D
1. Rebase in interactive mode
```
git rebase -i
```
2. This will open a vim terminal like the following
```
pick aaaaaaa Commit A
pick bbbbbbb Commit B
pick ccccccc Commit C
pick ddddddd Commit D
```
```
# Rebase aaaaaaa..ddddddd onto 1234567 (4 command(s))
#
# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# x, exec = run command (the rest of the line) using shell
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```
3. Edit it to look as
```
pick aaaaaaa Commit A
squash ddddddd Commit D
pick bbbbbbb Commit B
pick ccccccc Commit C
```
