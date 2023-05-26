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
