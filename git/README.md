# Git commands

`git init`:
- creates new git repository

`git status`:
- displays the state of the working directory and the staging (index) area

`git add <file name>`:
- add file to staging area

`git add .`:
- adds all files into staging area

`git commit`:
- opens commit message in **VIM** 

`git commit -m "commit message here"`:
- committing changes with provided commit message

`git restore <file name>` or `git checkout -- <file name>`: 
- both used in Git to discard changes made to a specific file and restore it to its previous state

`git clean -xdf <file name>`:
- following command will remove created files or directories, if don't provide file name and just execute command like `git clean -xdf` it will remove all new added files

`git restore --staged <file name>` or `git reset -- <file name>`:
- will move our changes from staging (index) area to unstage area (in red color :D), and will NOT remove your changes, just move from staging to unstage area

`git commit --amend -m "commit message"`:
- adding some new changes into last commit, also overwrites last commit message

`git reset HEAD~<number>` or `git reset HEAD^`:
- will remove commit and will move changes into unstaged area, _number_ represent how many commits we need to go back, and also the symbole ^ represents the same, if want to move HEAD back 1 commit ^ in 2 ^^ and so on.

`git reset --soft HEAD~1 or sha`:
- with `--soft` flag we remove the commit(s) and all our changes will be moved into index (staging area)

`git reset --mixed HEAD~1 or sha`:
- with `--mixed` flag we remove the commit(s) and all our changes will be moved into unstaged area

`git reset --hard HEAD~1 or sha`:
- with `--hard` flag we remove the commit(s) and all our changes will be dissapeared

### _Note_
sha can be every commit sha, beside last one, because with reset we moving head into selected sha, but we can't revert it in last because we every time will be in last sha 

`git revert <sha>`:
- adding new commit, and revert all changes to selected commit sha state in remote 

`git checkout <branch name>`:
- checkout into existing branch, which name we are passed

- `git checkout -b <branch name>`:
- checkout into existing branch, if there is no branch with passed name, will create new one and will checkout into it

### Conflict solving
When we are trying to merge branches, there can be conflicts, git by own can't solve that issue, thats why we are getting conflicts, after it in our code we will see following symbols

<<<<<<<<<<< HEAD

some code here

==========

other code

\>>>>>>>>>>> (branch name)

In _**HEAD**_ section we will se our branch code, in which we currently working

and

in (branch name) section we will see the other branch code which exists in same line in other branch which we are trying to merge to our branch

after fixing conflicts manually (just need to remove symbols, then keep both code or maybe remove which part is not necessary), we can run command `git status` and it will say, if you want to cancel everything run `git merge --abort` command or just commit new changes 
