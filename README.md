# Git workflow for a small team

We will essentially use this workflow: http://nvie.com/posts/a-successful-git-branching-model/ and the diagrams there are very good at explaining the relationship between branches. This document will list the commands needed to perform common tasks and explain what's happening.


## General guidelines
- All day-to-day work should be done on a branch created for the feature or bug that you are working on. Never be tempted to work directly on the develop or master branches.
- You should regularly run `git status` to check your current branch and see a summary of changes. Always run this before committing changes or pushing to the remote repo.
- Follow this guide carefully and collaboration will be safe and easy.


## Connect to a remote Git repo
To share work with others, you first need to connect your local working directory to the shared repository.

If the repo has already been set up on a service such as GitHub:
1. Go to the repo's web page and copy its clone URL.
1. Open a terminal window and navigate to the directory that you want to contain the repo's directory.
1. `git clone clone-url` - download the repo.

If you have started a project locally and want to add Git support to it:
1. Create an empty repo on GitHub and copy its clone URL. We will use this as a remote repo where we can share the project with others.
1. Open a terminal window and navigate to the root directory of your project.
1. `git init` - set up Git in this directory.
1. `git add .` - stage all of your files to be saved.
1. `git commit -m "First commit"` - save a new version locally with a descriptive message. 
1. `git remote add origin clone-url` - tell Git where to find the remote repo.
1. `git push -u origin master` - upload your repo to the server.


## Start working on a new branch
1. `git checkout develop` - switch to the branch that you want to base your work on (usually develop).
1. `git pull` - make sure you have the most recent version from origin.
1. `git checkout -b feature-name` - create a new branch called feature-name and switch to it.

You can now safely work on the project's files and save versions to this branch.


## Save a version of your work locally
While working on your feature branch, you will want to save versions as you get things working.

1. `git add .` - stage all of your files to be saved.
1. `git commit -m "Description of changes"` - save a new version locally with a descriptive message.


## Revert to an earlier version
WARNING this will lose your work since the chosen commit. If you tried something that didn't work and want to revert to a previous commit, you can do the following:

1. `git log` - list commits and find the one that you want to revert to. Copy its hash.
1. `git checkout hash` - switch to the commit with the given hash.
1. `git reset --hard` - clear commits ahead of this one.
1. `git checkout` - refresh the current branch.


## Push your local branch up to the remote repo
This will back up your work and allow other team members to access what you've done.

`git push -u origin feature-name` - upload your branch to the server.

To push this branch again, you only have to enter `git push` in the future.


## Merge your finished feature into the develop branch
Once your feature is complete and fully tested, it is ready to be merged into the develop branch.

1. `git checkout develop` - switch to the develop branch.
1. DO WE NEED TO PULL THE LATEST?
1. `git merge --no-ff feature-name` - combine the two branches, retaining their relationship.
1. There are likely to be conflicts between files that have been altered on both branches. Edit these files and make sure that your changes are compatible and don't break anything.
1. `git branch -d feature-name` - delete your local branch if you don't need it any more.
1. `git push origin develop` - push your combined branch to the server.




# Useful commands

`git status` - Show a summary of the current branch and files that have been changed. Run this often to keep track of what you're doing and which branch you currently have checked out.

`git log --graph --oneline --decorate --all` - Show a colourful graph that shows how branches are related.


## Cache your GitHub credentials

Accessing GitHub with HTTPS is much easier to set up than SSH, plus it also works through strict firewalls and proxies when SSH may fail. However, it gets annoying to type your username and password all the time. You can tell Git to remember your credentials for an hour by entering the following.

	git config-global credential.helper cache
	git config-global credential.helper 'cache --timeout=3600'
