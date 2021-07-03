# docker

## Install docker desktop

### Unable to connect to 127.0.0.1:3306
http://github.com/docker-library/mysql/issues/275
```
docker exec -it [container_id] bash
mysql -u root -p
  Password: root

mysql> SELECT host, user FROM mysql.user;
mysql> CREATE USER 'username'@'%' IDENTIFIED BY 'password';
mysql> GRANT ALL PRIVILEGES ON *.* TO 'username'@'%';
mysql> exit
```

```
docker exec -it [container_id] bash
vim /etc/my.cnf
  [mysqld]
  secure-file-priv=NULL
```

### Move files to docker container

https://stackoverflow.com/questions/22907231/copying-files-from-host-to-docker-container
```
docker cp file.csv [container_id]:/var/lib/mysql/SCHEMA_NAME/
```

```sql
CREATE TABLE `ANIMAL_INS` (
  col1 type1,
  ...
  colN typeN
);

LOAD DATA INFILE 'aac_intakes.csv' 
-- LOAD DATA LOCAL INFILE '/Users/agnt/Downloads/20616_26627_bundle_archive/aac_intakes.csv' 
INTO TABLE `ANIMAL_INS`
FIELDS TERMINATED BY ',' 
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;
```

### Mysql5.7 setup using docker

- Install docker desktop (Win10)
```
[NOTE] Docker를 설치하기 전에
  '작업관리자' > 성능 > 가상화 ('사용')에 사용안함 으로 설정되어 있다면
  BIOS 설정에서 virtualization(가상화)를 enabled로 바꿔줘야 합니다.
    → 구글 'how to enable docker virtualization on BIOS setting'
```

- Docker CLI
``` sh
# docker 설치여부 확인
docker version

# docker 이미지 조회
docker images

# 이미지 삭제
docker rmi REPO

# dangling 이미지 조회 및 삭제
docker images -f dangling=true
docker images purge

# docker 컨테이너 프로세스 확인
docker ps -a


# docker 이미지를 export 및 import 할 수 있습니다.
# 이미지 export
docker save -o .\IMAGE_NAME TAG_NAME

# 이미지 import (해당경로로 이동)
docker load -i .\IMAGE_NAME

# download mysql 5.7 image from docker hub
# image tags: https://hub.docker.com/r/ysql/mysql-server/tags
docker pull mysql:5.7
docker pull mysql/mysql-server:8.0
docker images

# 'mysql57'을 컨테이너 이름으로 하는 docker 컨테이너 실행
docker run -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=root --name mysql57 mysql:5.7
docker run -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=root --name mysql8 mysql/mysql-server

# docker 프로세스 확인
docker ps -a

# run되는 컨테이너 정지시키기
docker container pause

# stop된 컨테이너 전부 삭제
docker container prune

# stop된 모든 컨테이너 삭제
docker system prune
```

- 데이터베이스 (스키마) 생성
```
MYSQL을 docker로 실행(docker run)혹은 로컬 MYSQL Server가 실행된 상태에서
MySQL workbench를 실행하려면 database를 만들어야 합니다.
```

``` sh
# mysql57 컨테이너 서버에 접속 가능
# winpty docker exec -it mysql57 //bin//bash
docker exec -it mysql57 //bin//bash

# 'docker exec -it mysql57 //bash//bash' 커맨드

mysql -u root -p
Enter password: root
```

``` sql
CREATE DATABASE /*!32312 IF NOT EXISTS*/STARPASS;
USE STARPASS;
```

# Linux Volume control

* Volume up/down
```commandline
amixer -D pulse sset Master 3277+
amixer -D pulse sset Master 3277-
```

* Mute/unmute toggle
```commandline
amixer -D pulse sset Master toggle
```

* check volume percentage
```commandline
amixer get Master
```

* play/pause/next/previous
```commandline
rhythmbox-client --play-pause
rhythmbox-client --previous
rhythmbox-client --next
```

# Spring Tool Suite 4 plugins
```
http://vrapper.sourceforge.net/update-site/stable
http://eclipse-color-theme.github.com/update
http://dl.bintray.com/harawata/eclipse
http://propedit.sourceforge.jp/eclipse/updates/
```

# Applying .jar to application
download tar.gz and do the following
```commandline
sudo mkdir /opt/APP_NAME
sudo tar -xzvf ide.tar.gz -C /opt/APP_NAME
```

[How to move everything to parent directory](https://superuser.com/questions/88202/how-do-i-move-files-and-directories-to-the-parent-folder-in-linux/542214)
```commandline
cd /opt/APP_NAME/APP_NAME_DUP
sudo find . -maxdepth 1 -exec mv {} .. \;
cd ..
sudo rm -rf APP_NAME_DUP
```

```commandline
sudo vim /opt/APP_NAME/bin/APP_NAME64.vmoptions
sudo vim /opt/APP_NAME/bin/APP_NAME.vmoptions

-javaagent:/full/path/to/bin/myFile.jar
```

```commandline
cp ~/myFile.jar /opt/APP_NAME/bin/
sudo chown root.root myFile.jar
sudo chmod 755 myFile.jar
sudo java -jar /opt/APP_NAME/bin/myFile.jar
cd /opt/APP_NAME/bin/ && ./APP_NAME.sh
```

# APP_NAME create shortcut
```commandline
sudo vim /usr/share/applications/APP_NAME.desktop
```
```
[Desktop Entry]
Version=1.0
Name=ide
Exec=/opt/APP_NAME/bin/APP_NAME.sh
Path=/opt/APP_NAME/bin
Icon=/opt/APP_NAME/bin/APP_NAME.png
Terminal=false
Type=Application
Categories=Application;Development;IDE
Keywords=APP_NAME;
StartupWMClass=ide
```

[Duplicating or missing launcher icon](https://askubuntu.com/questions/403766/duplicate-icons-for-manually-created-gnome-launcher-items):
```
$ xprop WM_CLASS
# click application to print StartupWMClass
# insert 'StartupWMClass=app_name' line in .desktop
```

# create symlink
```commandline
sudo ln -s /full/path/to/script.sh /usr/local/bin/name_of_new_command
vim /full/path/to/script.sh 
    # --snip--
    java -jar path/to/jar/jarName.jar "$*"
    # "$*" takes all arguements given to the bash script and send them directly
    # to your java program.
    name_of_new_command arg1 arg2 arg3 arg4
```


# Useful Commands
```commandline
lshw -short
lshw -class network

dmesg
```


# tlp
improve power usage and battery life in laptop


# powertop


# Java
```commandline
java -version
javac -version # openjdk not installed
# openjdk
sudo apt-get install default-jdk

# oracle jdk
sudo yum localinstall jdk-8u172-linux-x64.rpm
```

set default java command
```commandline
sudo alternatives --config java
1 ...
2 ...
```


# Samba
- setting on Ubuntu
```commandline
sudo apt-get update
sudo apt-get install samba
sudo gedit /etc/samba/smb.conf &
```

```
[share]
    comment = Ubuntu File Server Share
    path = /srv/samba/share
    browsable = yes
    guest ok = yes
    read only = no
    create mask = 0755
```

```commandline
sudo mkdir -p /srv/samba/share
sudo chown nobody.nogroup /srv/samba/share
sudo gedit /etc/init/nmbd.conf

sudo service smbd restart
sudo service nmbd restart

sudo touch share/test.txt
```
- setting on Windows
check network folder


# Git command
git workflows that I often use. It might contain incorrect information. Non-commital on a timeline.

## Install git
```commandline
# on Windows : https://git-scm.com/download/win
# on Linux (fedora, ubuntu)
sudo dnf install git
sudo apt install git
```

## branching
```commandline
git branch junk/testbranch
git checkout junk/testbranch
# changes made
git add *
git commit -m 'branch commit'

git push -u origin junk/testbranch
# remote branch name has been set as same 'junk/testbranch'
# one can change different remote branch name
# git push origin local-branch-name:remote-branch-name

git checkout junk/testbranch
git pull origin junk/testbranch # git pull origin remote_branch
git checkout master
git pull origin master # git pull
git merge --no-ff --no-commit test

git status # resolve merge conflict

git commit -m 'merge test branch, junk/testbranch'
git push
```


# update branch from master branch
```commandline
git checkout wip/sat
git merge master
git push origin wip/sat
```


# Create remote (upstream) branch by pushing with -u
push a new local branch to a remote Git repository and track it. 

[update remote if remote branch is not showing up](https://stackoverflow.com/a/24827745)

[push local branch to remote and track it](https://stackoverflow.com/a/1519032)

```commandline
git remote update

git push upstream wip/junho

# Git will set up the tracking information during the push.
git push -u origin <branch>
```

# Make a branch track existing remote (upstream) branch
[track existing remote branch](https://stackoverflow.com/a/2286030)
```commandline
git branch -u upstream/wip/sat
git branch --set-upstream-to=upstream/wip/sat
```

# Create local branch and make it track existing remote (upstream) branch
```commandline
git fetch --all

# create a local branch named wip/junho, 
# tracking the remote branch origin/wip/junho
# When you push your changes the remote branch will be updated.
git checkout --track origin/wip/junho
```

# fetch remote branch
[fetch remote branch](https://stackoverflow.com/a/9537923/9122475)

```commandline
git checkout wip/sat
git config --list 
# ...
# branch.wip/sat.remote=origin

git branch --set-upstream-to=upstream/wip/sat
git config --list 
  # ...
  # branch.wip/sat.remote=upstream
git pull
# (wip/sat|MERGE) -> resolve conflicts manually

git branch --set-upstream-to=origin/wip/sat
git commit -m 'comments"
git push -u origin wip/sat
# make pull request to upstream remote!


git remote -r
  origin/wip/sat
  ...

git checkout --track origin/wip/sat

git branch
  master
* wip/sat
```

### git checkout master
![git checkout master](merge1.jpeg)
### git merge bugFix
![git merge bugFix](merge2.jpeg)

### git checkout bugFix, git merge master
![git checkout bugFix; git merge master](merge3.jpeg)


REMOTE REPO 'origin' <-> LOCAL
```commandline
git clone https://github.com/github_user/repo_name.git
cd repo_name

# local -> remote
mkdir -p $HOME/repo_name
cd $HOME/repo_name
git init
git remote add origin https://github.com/github_user/repo_name.git

# after making changes : 
git add '*'  #ADD TO STAGE
git commit -m 'Add all local files' 
git push -u origin master

git show HEAD //latest commit log

git checkout HEAD filename //discard changes and restore back to the last commit
git checkout -- filename  //does the same thing as 'checkout HEAD'

git add scene-2.txt
git reset HEAD scene-2.txt //unstage file from the staging area using
// It does not discard file changes from the working directory, 
// it just removes them from the staging area.

git reset commit_SHA //undo commits

git reset commit_b_SHA
//Before reset :  a-b-c-d-HEAD
//After reset :  a-b-HEAD

git diff HEAD
git diff --staged
git reset octofamily/octodog.txt    #unstaging
git checkout -- octocat.txt
git branch clean_up

git branch //check what branch is it currently

git branch bugFix // create new branch
git checkout bugFix //switch to new branch

git checkout -b bugFix //create & switch to new branch at the same time
// you can make commits on the new branch that have no impact on master.

git checkout master
git merge bugFix
// merge the new branch bugFix(giver) into master(receiver) branch
// no merge conflict since master had not changed

git checkout bugFix
git merge master //merge master into bugFix

// Since bugFix was an ancestor of master, 
// git didn't have to do any work; it simply just moved bugFix 
// to the same commit master was attached to.

// Now all the commits are the same color, 
// which means each branch contains all the work in the repository!


// merge conflict : 
//  <<<<<<< HEAD
//  master version of line
//  =======
//  fencing version of line
//  >>>>>>> fencing

git add conflict_file_name
git commit -m 'merge conflict resolved'

git branch -d branch_name //delete branch after merged to 'master' branch

//  Imagine that you’re a science teacher, developing some quizzes with Sally, 
//  another teacher in the school. You are using Git to manage the project.
//  To collaborate followings are required :
//  A complete replica of the project on your own computers
//  A way to keep track of and review each other’s work
//  Access to a definitive project version

//  You can accomplish all of this by using remotes. 
//  A remote is a shared Git repository that allows multiple 
//  collaborators to work on the same Git project from different locations
//  Collaborators work on the project independently, 
//  and merge changes together when they are ready to do so.

//  Sally has created the remote repository, science-quizzes 
//  in the directory curriculum, which teachers on the school’s shared network 
//  have access to. In order to get your own replica of science-quizzes, 
//  you’ll need to clone it with:
//  remote_location = file_path or web address(url)
git clone remote_location clone_name
git clone science-quizzes my-quizzes
cd my-quizzes

//  list of a Git project’s remotes with the command:
git remote -v


//  Sally changed the science-quizzes Git project in some way. 
//  If so, your clone will no longer be up-to-date.
//  An easy way to see if changes have been made to the remote 
//  and bring the changes down to your local copy is with:
git fetch

//  Even though Sally’s new commits have been fetched to 
//  your local copy of the Git project, those commits are 
//  on the origin/master branch. 
//  Your local master branch has not been updated yet, 
//  so you can’t view or make changes to any of the work she has added.

//  In Lesson III, Git Branching we learned how to merge branches. 
//  Now we’ll use the git merge command to integrate origin/master 
//  into your local master branch. The command:
git merge origin/master


//  Git has performed a “fast-forward” merge, 
//  bringing your local master branch up to speed 
//  with Sally’s most recent commit on the remote.

//  In the output, notice that the HEAD commit has changed. 
//  The commit message now reads same as Sally's
git log


//  Now that you’ve merged origin/master into your local master branch, 
//  you’re ready to contribute some work of your own. The workflow for Git 
//  collaborations typically follows this order:

//  1. Fetch and merge changes from the remote
//  2. Create a branch to work on a new project feature
//  3. Develop the feature on your branch and commit your work
//  4. Fetch and merge from the remote again 
//      (in case new commits were made while you were working)
//  5. Push your branch up to the remote for review

//  Steps 1 and 4 are a safeguard against merge conflicts, 
//  which occur when two branches contain file changes that cannot 
//  be merged with the git merge command. Step 5 involves git push, 
//  a command you will learn in the next exercise.

```

## Fork
```commandline
# Click 'fork' from the owner's repository, then a copy of the repo will appear in your Repository.
# Clone the forked repo to your local directory
git clone https://github.com/fggo/myproject.git

cd myproject # on (master) branch

# set config info
git config --global user.name 'YOUR_NAME'
git config --global user.email 'YOUR_EMAIL'

# In your local clone of your forked repository, you can add the original GitHub repository as a "remote".
# "Remotes" are like nicknames for the URLs of repositories - origin is one, for example.
# origin :  default remote 'origin', once a repo is cloned, it points to your fork on GitHub,
# not the original repo it was forked from.
# To keep track of the original repo, you need to add another remote named upstream
# Manage the set of repositories ("remotes") whose branches you track.
# add new remote upstream repository (original repository from the owner),
# which will be synced with the fork(copied repository).

git remote add upstream https://github.com/YoonYeoSong/Parking.git
git remote -v
    origin  https://github.com/fggo/Parking.git (fetch)
    origin  https://github.com/fggo/Parking.git (push)
    upstream  https://github.com/YoonYeoSong/Parking.git (fetch)
    upstream  https://github.com/YoonYeoSong/Parking.git (push)

# Sync your fork by git fork (or git pull) changes from original repo (upstream)
# push changes to make forked repository up-to-date it only updates the forked repo(copy)
git pull upstream master
git push origin master


# now go to the original repo,
# click pull request -> create pull request
# now original owner of repo can confirm 'merge pull request'
# ALL DONE!

git push --delete origin <branchname>
git push --delete upstream <branchname>
```


## Windows 10 msys2 설치

- [Download](https://github.com/msys2/msys2-installer/releases/download/2021-06-04/msys2-x86_64-20210604.exe)

``` sh
# install tmux, git
pacman -S tmux
pacman -S git

vim ~/.bash_profile
export HOME=/c/Users/nffz
source ~/.bash_profile
```


- tmux config
```
# .tmux.conf
unbind C-b
set -g prefix C-a

bind -n M-Left select-pane -L
bind -n M-Right select-pane -R
bind -n M-Up select-pane -U
bind -n M-Down select-pane -D

set -g mouse on
```

- vim config
```
# .vimrc
if has("gui_running")
  if has("gui_gtk2")
    set guifont=Inconsolata\ 14
  elseif has("gui_macvim")
    set guifont=Menlo\ Regular:h14
  elseif has("gui_win32")
    set guifont=Consolas:h10:cANSI
  endif
endif

syntax on
set background=dark
set lines=50 columns=100

winpos 800 100


set backspace=indent,eol,start
set listchars=space:.,tab:>-,trail:.,extends:>,precedes:<
" eol:$

" hi SpecialKey ctermfg=grey guifg=grey70
" hi Whitespace ctermbg=red guibg=red

set list


set nu

nnoremap zz :update<CR>

set enc=utf-8
set fileencodings=utf-8,cp949
set langmenu=cp949

set tabstop=4
set shiftwidth=4

inoremap jj <esc>
inoremap ㅓㅓ <esc>

" show indent line
" let g:indentLine_char = '|'

noremap <F11> :set list!<CR>

nnoremap yy yy"+yy
vnoremap y ygv"+y

colorscheme morning
```

- git config display branch
```
# /home/유저이름/.bashrc
# https://stackoverflow.com/a/64934557

if test -f ~/.config/git/git-prompt.sh
then
    . ~/.config/git/git-prompt.sh
else
    PS1='\[\033]0;$TITLEPREFIX:$PWD\007\]' # set window title
    PS1="$PS1"'\n'                 # new line
    PS1="$PS1"'\[\033[32m\]'       # change to green
    PS1="$PS1"'\u@\h '             # user@host<space>
    PS1="$PS1"'\[\033[35m\]'       # change to purple
    PS1="$PS1"'$MSYSTEM '          # show MSYSTEM
    PS1="$PS1"'\[\033[33m\]'       # change to brownish yellow
    PS1="$PS1"'\w'                 # current working directory
    if test -z "$WINELOADERNOEXEC"
    then
        GIT_EXEC_PATH="$(git --exec-path 2>/dev/null)"
        COMPLETION_PATH="${GIT_EXEC_PATH%/libexec/git-core}"
        COMPLETION_PATH="${COMPLETION_PATH%/lib/git-core}"
        COMPLETION_PATH="$COMPLETION_PATH/share/git/completion"
        if test -f "$COMPLETION_PATH/git-prompt.sh"
        then
            . "$COMPLETION_PATH/git-completion.bash"
            . "$COMPLETION_PATH/git-prompt.sh"
            PS1="$PS1"'\[\033[36m\]'  # change color to cyan
            PS1="$PS1"'`__git_ps1`'   # bash function
        fi
    fi
    PS1="$PS1"'\[\033[0m\]'        # change color
    PS1="$PS1"'\n'                 # new line
    PS1="$PS1"'$ '                 # prompt: always $
fi

MSYS2_PS1="$PS1"               # for detection by MSYS2 SDK's bash.basrc

# Evaluate all user-specific Bash completion scripts (if any)
if test -z "$WINELOADERNOEXEC"
then
    for c in "$HOME"/bash_completion.d/*.bash
    do
        # Handle absence of any scripts (or the folder) gracefully
        test ! -f "$c" ||
        . "$c"
    done
fi
```

- 깃 log한글 깨짐현상
```
Options > Text > Locale 'en_US'
Options > Text > Character Set 'UTF-8'
```

- 깃 윈도우 환경
```sh
git config --globalcore.autocrlf true
```
