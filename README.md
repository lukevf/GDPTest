# Gitting started 

GDPTest is a repository that you can mess around with and practice git. The
following document contains the 10 or so commands that are needed to form a
basic git workflow. I hope it's clear but guess it probably isn't so
modifications are welcome.

## Cloning this repository

The first step is to `clone` the central repository, this creates a local
copy of the entire project. 

```git clone https://github.com/lukevf/GDPTest.git ```

N.B. when you clone a repository git creates a shortcut `origin` that
points to the original file location (GitHub) and it's really useful.

## Working on your local repository

Once the files are on you computer you can modify and change them without
effecting the files on the central repository. The basic local Git process
is edit, stage commit. In git a commit is the file eqivelant of a save
point in a game that you can revert back to if you get killed by evil bugs
with laser guns. This is the first huge benefit of git. Staging is the
process of selcting which files to include in a particular commit. The
following commands are used for this:-

- `git status` shows the state of your local repository: which files have
	been changed; which files are staged and files which aren't currentley
	being managed by git.
- `git add <file>` stages a file for commit.
- `git commit` commits the staged files. N.B. Each commit requires a log
	entry -- a description of what you've done. If you run `git commit` it
	opens vim where you type in you log entry and then write an quit. To save
	youself the vim hassle you can just run `git commit -m "<message for
	log>"` instead.
- `git checkout -- <file>` restores a file to it's last committed state

## Publishing you new code

Once you are happy with all of your local changes it is time to `push` the
new code to the GitHub using the command.

```git push origin master``` 

If somebody has pushed changes to GitHub, you can pull the changes so that
your local copy is up to date.

```git pull origin master```

If in the interval between your last `pull` and wanting to `push` someone
else has `push`ed to GitHub you need to intergate thier commits with you
own. The challenge is shown schematically below. `origin/master` is pulled
at commit 0 to `local/master`. The local files (`local/master`) are then
modified and in the process two commits (A and B) are made. In the meantime
someone else has pushed thier 3 commits (1, 2 and 3) to GitHub.

```
                                    om
origin/master: --- 0 --- 1 --- 2 --- 3
                    \
local/master :       --- A --- B
                              lm
```

When this happens `git push origin master` will return an error. To
integrate the commits 1, 2 and 3 run the following:-

```git pull --rebase origin master```

Using `--rebase` changes the tree so that you aren't trying to merge the
branches but instead concate them. See the schematic below.

```
                                    om
origin/master: --- 0 --- 1 --- 2 --- 3
                                      \
local/master :                         --- A --- B
                                                lm
```

Once the rebase has occured your local files are considered ahead by 2
commits (`lm` is A and B ahead of `om`) and you can then `push` to GitHUb
using the standard command. Resulting in the schematic below.

```
                                                om
origin/master: --- 0 --- 1 --- 2 --- 3 --- A --- B
                                                  \
local/master :                                     - 
                                                  lm
```

### Conflicts

Sometimes there will be a conflict (two people have changed the same file)
in which case `git pull --rebase origin master` gives the following
message:- 

``` CONFLICT (content): Merge conflict in <some-file> ```

In this case git will pause the rebase at the commit that caused the
conflict. Running `git status` will list the conflicts. Edit the conflicted
file so that the code works and then continue the rebase.

```
git add <some-file>
git rebase --continue
```

It's easy to loose track of what's going on when there's a conflict.
Runiing `git rebase --abort` takes you back to where you were before
running `git pull --rebase origin master`.

## An example
```
[luke@thinkbox Desktop]% git clone https://github.com/lukevf/GDPTest.git
Cloning into 'GDPTest'...
remote: Counting objects: 3, done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.

[luke@thinkbox Desktop]% cd GDPTest 
```
I then create 2 new files `luke.h` and `luke.c` 
```
[luke@thinkbox GDPTest]% git status
On branch master
Your branch is up-to-date with 'origin/master'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        luke.c
        luke.h

nothing added to commit but untracked files present (use "git add" to track)

[luke@thinkbox GDPTest]% git add luke.c luke.h

[luke@thinkbox GDPTest]% git commit -m "Made a new function luke() but it's still a bit buggy"
[master 673943f] Made a new function luke() but it's still a bit buggy
 2 files changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 luke.c
 create mode 100644 luke.h

```
I then fix the bugs in `luke.c` and try to push
```
[luke@thinkbox GDPTest]% git status
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   luke.c

no changes added to commit (use "git add" and/or "git commit -a")

[luke@thinkbox GDPTest]% git add luke.c

[luke@thinkbox GDPTest]% git commit -m "Debugged the function luke() and it is now deployable"
[master 16cbf48] Debugged the function luke() and it is now deployable
 1 file changed, 1 insertion(+)

[luke@thinkbox GDPTest]% git push origin master
Username for 'https://github.com': lukevf
Password for 'https://lukevf@github.com': 
To https://github.com/lukevf/GDPTest.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'https://github.com/lukevf/GDPTest.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```
Bugger
```
[luke@thinkbox GDPTest]% git pull --rebase origin master
remote: Counting objects: 4, done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 4 (delta 0), reused 4 (delta 0), pack-reused 0
Unpacking objects: 100% (4/4), done.
From https://github.com/lukevf/GDPTest
 * branch            master     -> FETCH_HEAD
   76b3c68..f01eac5  master     -> origin/master
First, rewinding head to replay your work on top of it...
Applying: Made a new function luke() but it's still a bit buggy
Applying: Debugged the function luke() and it is now deployable

[luke@thinkbox GDPTest]% git push origin master
Username for 'https://github.com': lukevf
Password for 'https://lukevf@github.com':
Counting objects: 6, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (6/6), 628 bytes | 628.00 KiB/s, done.
Total 6 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), done.
To https://github.com/lukevf/GDPTest.git
   f01eac5..8cced41  master -> master

```
And that's it
