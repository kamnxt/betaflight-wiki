# Contributing to Betaflight

Contributing to Betaflight involves preparing your development environment and making a fork of the repository and working with Git.
Look in https://github.com/betaflight/betaflight/tree/master/docs/development for installation notes for your environment.
This document gives some instructions how to handle Git. First make a fork of the repository you want to work on from the github website.
Now you clone your fork to your development machine.

    git clone https://github.com/yourname/betaflight.git

## Global configuration

Please configure this to have the correct author in your commits

    git config --global user.name "Your Name"
    git config --global user.email "your@email.domain"

If you omit to configure this you get a warning and have to use the following commands to rectify:

    git config --global --edit
    git commit --amend --reset-author

## Setup remotes

    git remote add upstream https://github.com/betaflight/betaflight.git
    git remote -v

## Create branch and make changes

Git also has the new `switch` command.

    git checkout -b fix

## Stage files for commit

Commit your changes after making initial changes:

    git add .
    git commit -m "message"
    git push origin branch

## Make more changes and commit on top of last commit

	git commit --amend
	git push --forward-with-lease origin branch

## Update master branch with upstream updates and update your fork

    git checkout master
    git pull --rebase upstream master
    git push -f origin master

## Update your local branch with upstream changes 

    git checkout branch
    git branch --set-upstream-to=upstream/master branch
    git pull --rebase

If you look at `git reflog --oneline` you will see these lines:

    shacode HEAD@{0}: rebase (finish): returning to refs/head/branch
    shacode HEAD@{1}: rebase (pick): your branch commit description
    shacode (upstream/master, origin/master, origin/HEAD, master) HEAD@{2}: rebase (start): checkout longsha

## Unstage file from working area

	git restore --staged <file> to unstage a file from working area.

## Recover from unwanted commit without push

    git reset HEAD^

## If you want to completely remove the unstaged changes run

    git reset --hard HEAD

## Unwanted commits in your latest push.

First try:

    git rebase -i origin/branch~2 branch
    git push --force-with-lease origin branch

If this fails, backup your changed files (maybe also could use git stash)

    git reset HEAD~ --hard
    git checkout branch

And restore your saved files (or use git stash pop)

    git add .
    git commit -m "Make new commit"
    git push -f origin your_branch

## See general changes

    git diff

## See changes in particular file

    git log -- src/main/cms/cms` .c 

## Bisection

Do bisection:

    git bisect reset
    git bisect start
    git checkout 4.1.1

Build and make sure it works

    git bisect good
    git checkout 4.1.2

Build and make sure it fails

    git bisect bad

Then git will automatically bisects commits between the two versions, checks out a new bisecting commit.
You will build and test it, and tell git if the commit was good or bad.

## Links

https://devconnected.com/how-to-remove-files-from-git-commit/