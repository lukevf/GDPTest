# Gitting started 

## Cloning this repository

The first step is to `clone` the central repository, this creates a local
copy of the entire project. 

``` $ git clone https://github.com/lukevf/GDPTest.git ```

N.B. when you clone a repository git creates a shortcut `origin` that
points to the original file location (GitHub) and it's really useful.

## Working on your local repository

Once the files are on you computer you can modify and change them without
effecting the files on the central repository. The basic Git commit process
is edit, stage commit.

### Staging

``` git status ```

It is possible to stage the entire contents of a folder by replacing
`main.c` with `.`. This is quicker but can sometimes lead to wasting space
version controlling swap files ect.

If you have been tinkering around but actually prefered a file how it was
you can undo your changes since last commit useing the command:-

``` $ git checkout -- <file> ```

Once you have staged all of the files they get commited:-

``` $ git commit -m "<message for log>" ```

`-m "<message for log>"` is useful for quick one line descriptions of the
changes that you made. If you leave it off git will open a text editor and
you can write a more detailed log entry to describe the commit.

So far no changes have been made to the master files stored on GitHub. We
now push the changes to GitHub (N.B. I'm not 100% that you will be allowed
to push changes to my repo but maybe give it a shot):-

``` $ git push origin master ```

If somebody has pushed changes to GitHub, you need to pull the changes so
that your local copy is up to date

``` $ git pull origin master ```

When dealing with loads of files it can all get a bit confusing. By running
`$ git status` you get an output saying what's going on with all of the
files and also where you local files are in relation to the master branch.

## Conceptual introduction and jargon

### Basic workflow

- The first step is to `clone` the repository from the remote host. This
	downloads all of the files from GitHub to your computer.
	
- Once you have cloned the files you can start modifying your local copies
	of the files. 
	
- Once you're happy with the modifications/want to save you progress you
	`commit` the changes to git. This creates a line in the sand and you can
	always restore files back to a state that has been committed.

- Having committed changes (can be more than one commit) you can then
	`push` the updated files back to the server.

- If changes have been pushed by another member, you need to pull their
	changes to update your local files. 

This sort of workflow is shown schematically below. The `master` branch
(more on branches below) is pulled to the local machine from the `origin`al
server. Two local commits are made (nodes 1 and 2) and finally the working
modified code is pushed back to the origin server.

```
local/master :     0-- 1 -- 2
                  /          \
origin/master: --0----------- 2	
```

### Branches


