# Git workflow for small teams

Git allows multiple people to contribute to the same project without overwriting each other's work. This is a guide to the common tasks that will allow us to work together safely and efficiently.

A similar guide, along with some useful diagrams is available at <https://www.atlassian.com/git/workflows#!workflow-feature-branch>


## Glossary

* branch - A collection of commits. Any commit can have a new branch created off of it.
* commit - A version of the project saved to a branch.
* feature - A section of the project that needs to be implemented.
* master - The central branch that other branches are based on.
* origin - The central repository on the server.
* push - Send changes from your current branch upstream to the server.
* pull - Fetch changes from another branch and merge them with your current local branch.
* repository - The collection of all branches that make up a project.


## Project planning

Before beginning work on a project, it's important to define its requirements and specify how everything should function. Split the work into individual features that can be implemented one at a time.


## Cache your GitHub credentials

Accessing GitHub with HTTPS is much easier to set up than SSH, plus it also works through strict firewalls and proxies when SSH may fail. However, it gets annoying to type your username and password all the time. You can tell Git to remember your credentials for an hour by entering the following.

	git config --global credential.helper cache
	git config --global credential.helper 'cache --timeout=3600'


## Setting up a repository

If you are starting a new repository, log into the GitHub website and create a new repository. If someone has already created it, they will send you the link for it.

On the repository's web page copy the HTTPS clone URL and use it in the following command.

	git clone URL

This will create a directory to hold the project. Run git commands in that directory to interact with the repository.


## Feature branch workflow

In this workflow, each individual feature is branched off of the master branch and then merged back in once complete. The master branch should only ever contain stable, well-tested code because new features will be based on it. Feature branches can still be shared for multiple people to work on and test without affecting master.


### 1. Begin working on a feature

Make sure you are starting with the latest version of master.
	
	git checkout master
	git pull

Create a new local branch and check it out to work on it.

	git checkout -b feature-name

You can now work on the project files safely without affecting the master branch.


### 2. Save a version of your work locally

Once you have reached a milestone in the development of your feature, you will want to save what you have done as a new version.

Check the files that have been changed, stage all changes to be saved and commit them with a description.

	git status
	git add .
	git commit -m "Short description of the changes"


### 3. Push your local branch to the server

Back up your work and allow other team members to access what you've done.

	git push -u origin feature-name

After you have done this the first time, you can just run the following in the future.

	git push

Once you have finished and tested your feature, make sure you push the final version up to the server.


### 4. Synchronise your branch with master

While you are working on your feature, the master branch is likely to be updated by other members of the team. If so, the version that your feature branch was based on will now be out of date. Before you can publish your feature, you need to make sure that it is compatible with the latest version of the master branch.

	git pull --rebase origin master

This downloads the central repository's latest version of the master branch and then applies your feature branch's commits to it locally. You may have to resolve some conflicts if files have been edited by other people.

Once you have resolved these conflicts, your local feature branch will be based on the latest version of the master branch. Push it up to the server so that other members of the team can check it out for testing.
	
	git push


### 5. Publish your feature to master

This is the final step in adding your new feature to the stable codebase on the master branch. The whole team's work is based on this branch so you need to be sure that you haven't introduced bugs that will affect other people. However, in reality bugs will always arise and they should be fixed by starting a new branch specifically to deal with them.

	git checkout master
	git pull
	git pull --rebase origin feature-name

Since your feature branch has already been rebased to master, this shouldn't cause any conflicts unless someone has just updated master. If this is the case, checkout your feature branch and repeat step 4.

Now your local master is up to date with the central repository but also includes your feature branch. When you're certain that everything is working as expected, push it up to the server.

	git push

Anyone pulling from the central repository will now get your new feature as part of the master branch.


## Useful commands

Show a summary of the current branch and files that have been changed. Run this often to keep track of what you're doing and which branch you currently have checked out.

	git status

Show a colourful graph that shows how branches are related.

	git log --graph --oneline --decorate --all


## Reset to an earlier commit

WARNING - this can lose your work. If you've made mistakes and want to roll back your local branch to a previous commit, you can the following.

First, list commits and find the commit that you want to roll back to.

	git log

Copy the hash for that commit and check it out.

	git checkout hash
	git reset --hard
	git checkout