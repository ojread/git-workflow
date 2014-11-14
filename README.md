# Git workflow for a small team

We will essentially use this workflow: http://nvie.com/posts/a-successful-git-branching-model/ and the diagrams there are very good at explaining the relationship between branches. This document will list the commands needed to perform common tasks and explain what's happening.


## General guidelines

- All day-to-day work should be done on a branch created for the feature or bug that you are working on. Never be tempted to work directly on the develop or master branches.
- The master branch 
- Regularly run `git status` to check your current branch and see a summary of changes. Always do this before committing changes to avoid creating a mess.
- Follow this guide carefully and collaboration will be safe and easy.


## Connect to a remote Git repo

To share work with others, you first need to connect your local working directory to the shared repository.

If the repo has already been set up on a service such as GitHub:

1. Go to the repo's web page and copy its clone URL.
1. Open a terminal window and navigate to the directory that you want to contain the repo's directory.
1. `git clone clone-url` - Download the repo.

If you have started a project locally and want to add Git support to it:

1. Create an empty repo on GitHub and copy its clone URL. We will use this as a remote repo where we can share the project with others.
1. Open a terminal window and navigate to the root directory of your project.
1. `git init` - Set up Git in this directory.
1. `git remote add origin clone-url` - Tell Git where to find the remote repo.
1. `git commit -am "First commit"` - Save a new version locally with a descriptive message. 
1. `git push -u origin master` - Upload your repo to the server.


## Day-to-day workflow

In this example, we are beginning work on a contact form for a website.

1. ### Start working on a new feature.
	1. `git checkout develop` - Switch to the branch that you want to base your work on.
	1. `git pull` - Make sure you have the latest version.
	1. `git checkout -b contact-form` - Create a branch to work on the new feature and switch to it.

1. ### Develop the feature
	1. Work on the feature in the project directory until you want to save your progress.
	1. `git status` - Check you are still on the correct branch and see a summary of the changes you have made.
	1. `git commit -am  "Description of changes"` - Save a new version of your work.
	1. Repeat until the feature is complete and fully tested.

1. ### Add your feature to the main development branch.
	1. `git checkout develop` - Switch back to the parent branch.
	1. `git pull` - Make sure you have the latest version.
	1. `git merge --no-ff contact-form` - Add your work to the develop branch. The --no-ff option preserves the history of the feature branch.
	1. Resolve any conflicts between the branches and follow the prompts to complete the merge. Check that your feature is still working and not conflicting with other people's work.
	1. `git branch -d contact-form` - Delete the feature branch once it is no longer needed.
	1. `git push origin develop` - Upload your updated branch for others to access.



## Other useful commands

### Show the log of commits

`git log --graph --oneline --decorate --all` - Show a colourful graph that shows how branches and commits are related.


### Cache your GitHub credentials

Accessing GitHub with HTTPS is much easier to set up than SSH, plus it also works through strict firewalls and proxies when SSH may fail. However, it gets annoying to type your username and password all the time. You can tell Git to remember your credentials for an hour by entering the following.

	git config-global credential.helper cache
	git config-global credential.helper 'cache --timeout=3600'


### Revert to an earlier version

WARNING - this will delete your work since the chosen commit. If you tried something that didn't work out and want to revert to a previous commit, you can do the following:

1. `git log` - list commits and find the one that you want to revert to. Copy its hash.
1. `git checkout hash` - switch to the commit with the given hash.
1. `git reset --hard` - clear commits ahead of this one.
1. `git checkout` - refresh the current branch.