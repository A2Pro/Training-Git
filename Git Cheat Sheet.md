**Team repository:** https://github.com/Tino-FRC-2473

Also checkout the [Official Git Cheat Sheet](https://training.github.com/downloads/github-git-cheat-sheet/). You can refer to this if something in this cheat sheet is unclear.

---

## GIT CHEAT SHEET

### REVIEWING STATUS AND HISTORY

git status # show state of repo: which branch you are on, staged and unstaged changes

git log # show history of your current branch as list of commits; use arrow keys to scroll up or down, press q to quit and exit the preview

git show # show details and contents of most recent commit: author, timestamp, changelist, etc

git show &lt;commit id> # show details and contents of the given commit: author, timestamp, changelist, etc

git diff # shows all unstaged changes

git diff --staged # shows all staged changes

git diff &lt;commit A> &lt;commit B> # shows all changes from commit A to commit B


### STAGING AND UNSTAGING CHANGES

git add . # stage all changes

git add &lt;filename(s)> # stage all changes for given file(s)

git add -p # select patch(es) to stage, use the y/n/s options when prompted. new files are not included in the selection list

git reset # unstage all changes

git reset -p # select patch(es) to stage, use the y/n/s options when prompted

git checkout . # undo all unstaged changes, use with caution


### HANDLING COMMITS

git commit -m “message” # create a commit with the given message

### best practice: your message should describe what the change is for (e.g. “reduce arm starting angle to fit within starting configuration” is more useful than “reduce arm starting angle to 30 degrees”)

git commit --amend # edit the most recent commit to include the staged changes, will be prompted to update the commit message; tip: [EDITING WITH VIM](#editing-with-vim-6)

git revert &lt;commit id> # create a commit that undoes the changes in the given commit, will be prompted to enter a commit message; tip: [EDITING WITH VIM](#editing-with-vim-6)

git reset --soft HEAD^ # undo the last commit but keep them as staged changes

git cherry-pick &lt;commit id> # apply given commit to current branch


### HANDLING BRANCHES

git branch &lt;branch name> # create a branch

git checkout &lt;branch name> # switch over to given branch, you will see HEAD is now pointing to the given branch

git rebase &lt;branch name> # set the reference of your current branch to given branch, think of it as applying all commits on your current branch on top of given branch

git rebase -i &lt;branch name> # interactive rebase: useful to pick/squash/drop commits while rebasing before creating a pull request; tip: see [EDITING WITH VIM](#editing-with-vim-6) and [RESOLVING MERGE CONFLICTS](#resolving-merge-conflicts-5)

git rebase -i &lt;commit_id> # interactive rebase: useful to pick/squash/drop commits on your own branch to clean up; tip: see [EDITING WITH VIM](#editing-with-vim-6)


### SYNCHRONIZING REMOTE AND LOCAL

git fetch # fetch changes for this branch from remote to local, but do not apply these changes

git pull # fetch changes for this branch from remote to local, and update current local branch to match remote branch

git push # push changes for this branch from local to remote


### TAGGING

git tag # show list of tags

git tag &lt;tag name> # create a lightweight tag on the current commit

git push origin &lt;tag name> # push tag to remote


## TIPS


### CREATING A PULL REQUEST

Since we employ the rebase and merge strategy for pull requests, you should make sure your local branch is branched off the top of your target branch. Follow these steps:



1. checkout your target branch and pull in the latest changes
2. checkout your new branch and rebase onto the target branch
3. resolve any merge conflicts
4. once successful, push your new branch to remote
5. go to github.com and open a pull request towards your target branch


### RESOLVING MERGE CONFLICTS

When rebasing, you might run into a lot of merge conflicts, you can try to first squash your commits on your branch then rebase to minimize the number of times you have to resolve conflicts.

When prompted to resolve merge conflicts, do the following:



1. run **git status** to see which files need to be resolved
2. open up each file and resolve the conflicts
3. stage all changes once you are satisfied
4. to continue, run **git rebase --continue**
5. to cancel the entire process, run **git rebase --abort**

Merge conflicts will show up as such:


    <&lt;<&lt;<&lt;< HEAD \
changes on destination_branch \
vs code labels this “current changes” \
======= \
changes on your_branch that the current commit is trying to apply \
vs code labels this “incoming changes” \
>>>>>>> your_branch

You can choose to keep only current changes, only incoming changes, or make a branch new change, which may include changes from both branches. Basically, you want this section of code to match your desired output after the merge.

Make sure to remove the conflict markers <&lt;<&lt;<&lt;<, =======, and >>>>>>> when completing the merge.

See [RESOURCES](#resources-7) for tips on using the VS Code UI for resolving merge conflicts.


### EDITING WITH VIM

Your default editor is probably Vim. To make changes, you can:



1. press **I** to enter insert mode
2. make your edits and navigate with arrow keys
3. press **ESC** to exit insert mode and go back to normal mode
4. enter **:wq** to save and quit; enter **:q!** to force quit (quit without saving)

You can update your VS Code configuration to use your current workspace as the default editor instead, see [RESOURCES](#resources-7).




## RESOURCES


---



* explanation and tutorials for git commands: [https://www.atlassian.com/git/tutorials](https://www.atlassian.com/git/tutorials)
* git intro slides: 
* using the VS Code UI for git:  [https://code.visualstudio.com/docs/sourcecontrol/overview](https://code.visualstudio.com/docs/sourcecontrol/overview)
* updating VS Code default editor: [https://frederic-hemberger.de/notes/visual-studio-code/override-default-editor-in-the-integrated-terminal/](https://frederic-hemberger.de/notes/visual-studio-code/override-default-editor-in-the-integrated-terminal/)


## 


## COMPETITION BEST PRACTICES


---



* a competition branch named **svr/main** shall be created and used to deploy code to the robot
    * other development branches shall be named **svr/subsystem/purpose** 
    * any changes on development branches shall be cherry-picked or merged to **svr/main**
        * see **git cherry-pick**
* all changes must be committed
    * commits should contain targeted changes and useful messages
        * e.g. if changes were made for different subsystems, they should be in different commits
        * see **git add -p**
    * even undoing changes should be recorded with a commit
        * see **git revert**
* the SW representative shall create a tag at the time of each match with the code used on the robot for that match
    * the tag shall be named **svr-qualNN** where NN is the match number (e.g. svr-qual01)
    * if multiple matches used the same code, multiple tags shall be created at the same commit
    * see **git tag**