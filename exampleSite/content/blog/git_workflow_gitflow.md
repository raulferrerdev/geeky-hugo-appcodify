---
title: "Git and workflow with GitFlow"
description: "Learn how to use Git and Gitflow to be able to control changes in the code of your projects. Understand what branch, checkout, clone, or commit mean."
date: 2021-07-20
categories: ["Deployment"]
tags: ["Development"]
type: "regular" # available types: [featured/regular]
draft: false
---
When developing software we find ourselves with the need to manage the changes that are being made in the code and that, when working as a team, all team members always have a copy of this code in which they can work and, later, integrate these changes. To facilitate this work we have version control systems, such as **Git**, which allow us to track and manage changes that occur in the code over time: for this we are going to see the use of **Git** and the workflow with **GitFlow**.
{{<ads1>}}

# What is Git

**Git** is a version control software developed by Linus Torvarlds (the creator of Linux), in order to coordinate the work with his collaborators.

Keep in mind that **Git** has a distributed architecture, so instead of the code being in a single place, when a developer makes a working copy of this code, it generates a repository that can contain the full history of changes that have been made in that code.
## Repository, revision, commit … Some vocabulary

When working with **Git** and version control, we come across a series of terms that it is necessary to know to know what is being done:

* **Branch**. A branch is a fork in your code at a certain point that allows you to develop your code without affecting the other branches. For example, we can make a branch to add new functionality to our code.
* **Checkout**. In **Git** this command allows us to both create branches and move between them.
* **Clone**. It allows us to obtain a copy of the repository locally to be able to work with it.
* **Commit**. This is a commit of a group of changes that has been made to the code. This confirmation contains both these changes as well as a descriptive message and some additional information (author, date …).
* **Conflict**. It is a problem in the integration of the changes that, coming from sources, want to be made in the same document and the system is unable to solve it. In this case, it is the developer himself who must solve the conflict.
* **Diff (change)**. A change or diff corresponds to a specific variation of the code. Version control is done by tracking these differences between file versions.
* **Head**. Head refers to the commit the repository is in at all times (and which usually coincides with the last commit in the working branch).
* **Merge**. Merge is the merger of two branches, that is, when we want to integrate the changes made in the code from one branch to another. For example, integrate the changes of a branch in which we have added a new functionality to the main branch.
* **Pull**. It is the reverse process of push, what is achieved is to download the code from the central repository to the local one to update it.
* **Pull request**. This is a request made to the owner of the code to update it with our changes.
* **Push**. It allows us to upload the code changes that we have in our local repository to the central repository to update it, so that the rest of the developers can download it.
* **Repository**. It is the place where both the code and the history of changes that have been made are stored.
* **Tag (label)**. As its name suggests, it allows us to label different revisions of a project to be able to identify them more easily (they are often used, for example, with versions of a project released to production).

# Let’s do it

Having seen some vocabulary, let’s see how to start using git in our projects in a basic way.

First we create a directory for our project:

```shell
$ mkdir gitexample
$ cd gitexample
```


Next we initialize the repository with **git init**:

```shell
$ git init
Initialized empty Git repository in /home/username/gitexample/.git/
```

Now we are going to create a file and add it to the repository with **git add**:

```shell
$ touch readme.md
$ git add readme.md
```

**git add** allows us to prepare the changes to be able to confirm them later. The most used options are **git add** {file}, **git add** {directory}, or *git add .* , which adds all the files.

What we are going to do now is use git commit to fix the modifications made:

```shell
$ git commit -m “Added readme.md”
[master (root-commit) cdd097f] Added readme.md
1 file changed, 0 insertions(+), 0 deletions(-)
create mode 100644 readme.md
```


The **-m** flag allows us to commit the changes with the addition of a message (which allows us later in the project to get an idea of what changes were made at each point).

Now we have already made some changes to our repository. To know what state it is in, we use **git status**:

```shell
$ git status
On branch master
nothing to commit, working tree clean
```

That tells us that we are in the **master** branch (which is the main one and the one that is created when the repository is started) and that all changes are verified.

Now we are going to see how to create a branch and position ourselves in it. For example, let’s create the **develop** branch:

```shell
$ git branch develop
$ git checkout develop
```


With this we create the **develop** branch and move to it. All of this can be done in a single step with the following command:

```shell
$ git checkout -b develop
Switched to a new branch 'develop'
```

Now we are going to suppose that we make a change in the *readme.md* file.

If we now do **git status**, it returns the following information:

```shell
$ git status
On branch develop
Changes not staged for commit:
(use "git add <file>..." to update what will be committed)
(use "git restore <file>..." to discard changes in working directory)
modified:   readme.md
no changes added to commit (use "git add" and/or "git commit -a")
```


That is, we are in the **develop** branch and we have unverified changes (and we have to do it).

Therefore, we do:

```shell
$ git add readme.md
$ git commit -m "Updated readme.md"
[develop 94feba9] Updated readme.md
 1 file changed, 1 insertion(+)
```


Ok, now we have the **develop** branch with new, verified code, and we want it to be in the **master** branch as well. What we will have to do is go to the **master** branch and merge the **develop** branch:

```shell
$ git checkout master
$ git merge --no-ff develop
```


When we do this, the text editor appears that asks us to add a message to the **merge**.

Once the message is added, we get on the screen:

```shell
Merge made by the 'recursive' strategy.
readme.md | 1 +
1 file changed, 1 insertion(+)
```

By using the flag **–no-ff** (not fast-forward), we have merged from **develop** to **master** keeping **develop**.
# GitFlow: organising your repositories

When we work on a project, especially if we do it with more developers, it is necessary to establish an organization system of the version control repository, which allows us in an agile way to create the new functionalities, correct the errors that appear and integrate it all to get the code into production.

For this we will use a branch and work system developed by [Vincent Driessen](https://nvie.com/posts/a-successful-git-branching-model/) and which we know from **GitFlow**. According to this system, we should have a couple of main branches and other secondary or support branches.
{{< image src="images/posts/git_workflow_gitflow_1.png" alt="Git and gitflow">}}

## master (main)

This branch contains the code for each version that has been uploaded to production. In addition, any code that is uploaded to this branch must be prepared to be released to production.
## develop (main)

This branch includes the code for the next iteration or version of the project, and it integrates the new functionalities that are being developed for that version.

{{< image src="images/posts/git_workflow_gitflow_2.png" alt="Git and gitflow">}}
## release (secondary)

This branch derives from **develop**, and contains the code for the version that will be released to production shortly. In this branch you can also correct any error before publishing it. After publishing the version, this branch will need to be integrated into both **develop** and **master**.

To name this branch, the following convention is commonly used: **release**-*{version number}*. For example, to create a release branch for version 2.3.5 from develop we would do the following:

```shell
$ git checkout develop
$ git checkout -b release-2.3.5

Or with a single command:

$ git checkout -b release-2.3.5 develop
```


## feature (secondary)

These are branches that, like the **release** branch, derive from the **develop** branch and contain the code corresponding to new functionalities. They are usually branches that only exist in the local repository of each developer. Every time a feature is finalized and approved, its branch is integrated into **develop**.

{{< image src="images/posts/git_workflow_gitflow_3.png" alt="Git and gitflow">}}
By convention this type of branch is named as **feature**/{*feature name*}. For example, to create a branch that contains the functionality of synchronizing the information of a user we could do the following:

```shell
$ git checkout develop
$ git checkout -b feature/userinfosync

Or with a single command:

$ git checkout -b feature/userinfosync develop
```

## hotfix (secondary)

A **hotfix** branch is created to correct a bug that needs to be fixed urgently in production code. That is why it derives from the **master** branch and, once corrected, is integrated into both the **master** branch and the **develop** branch.

{{< image src="images/posts/git_workflow_gitflow_4.png" alt="Git and gitflow">}}
The naming of these branches follows a pattern similar to that of **release** branches: **hotfix**-*{new version number}*. Thus, if we have to make a hotfix in version 2.3, we would create the **hotfix** branch in the following way:

```shell
$ git checkout master
$ git checkout -b hotfix-2.3.1

Or with a single command:

$ git checkout -b hotfix-2.3.1 master
```


That is, from **master** we create a **hotfix** branch with the value of the new version that will be uploaded to production (the old one is 2.3 and the corrected one will be 2.3.1).
### Tagging the master branch

Every time we release a version to production and integrate the code in the master branch, it is highly recommended to tag that integration, so that we can have the final code of each version identified.

For this we will use the **git tag** command. For example, if we want to tag the version 2.3.1 code in master, we will do the following:

```shell
$ git tag -a 2.3.1 -m "Version 2.3.1"
```


With **-a** we have labeled the version and with **-m** we have added a message.
# Git extensions for GitFlow implementation

We have just seen a series of commands that allow us to manage the repositories of our code: create branches, move between them, integrate them … But thanks to a series of **git** extensions, we can do all this in a simpler way. For this we will simply have to install [git-flow](https://github.com/nvie/gitflow) on our computer.

The git-flow documentation explains how to install these extensions in different environments. In the case of macOS, we can do it with **Homebrew**:

```shell
$ brew install git-flow
```


Once installed, we can initialize a repository, with a basic structure of branches, using the following command:

```shell
$ git flow init
```

And answer the questions posed to us:

```shell
Which branch should be used for bringing forth production releases?
   - devel
   - master
Branch name for production releases: [master]

Which branch should be used for integration of the "next release"?
   - devel
Branch name for "next release" development: [master] devel

How to name your supporting branch prefixes?
Feature branches? [feature/]
Release branches? [release/]
Hotfix branches? [hotfix/]
Support branches? [support/]
Version tag prefix? []
```

# Managing the different branches with git-flow
## Feature

```shell
$ git flow feature
```


It shows us a list of all the **feature** type branches.

```shell
$ git flow feature start {feature_name}
```


With this instruction we create a new branch called *{feature_name}* that derives from **develop** and positions us in that branch.

```shell
$ git flow feature finish {feature_name}
```


This command **merges** the *{feature_name}* branch into the **develop** branch, positions us in the **develop** branch and deletes the *{feature_name}* branch.

```shell
$ git flow feature publish {feature_name}
```


This command is used to publish the *{feature_name}* branch to a remote repository.

```shell
$ git flow feature pull origin {feature_name}
```


With this command we download the branch *{feature_name}* from the remote repository.
## Release

```shell
$ git flow release
```


It shows us a list of all the **release** type branches.

```shell
$ git flow release start {release_name} [BASE]
```


With this instruction we create a new branch called *{release_name}* that derives from **develop** and positions us in that branch. *[BASE]* is an optional value that corresponds to the sha1 hash of the integration (**commit**) from which we want this branch to derive.

```shell
$ git flow release finish {release_name}
```


This command performs different tasks: it labels the branch with {release_name}, merges the {release_name} branch into the develop branch and the master branch, and ends up deleting the release branch (remember to publish the tags with git push origin – tags).

```shell
$ git flow release publish {release_name}
```
{{<ads2>}}


This command is used to publish the branch *{release_name}* to a remote repository.
## Hotfix

```shell
$ git flow hotfix
```


It shows us a list of all the **hotfix** type branches.

```shell
$ git flow hotfix start {version} [BASE]
```


With this instruction we create a new branch called *{version}* that derives from **master** and places us in that branch.

```shell
$ git flow hotfix finish {version}
```


This command performs different tasks: tags the branch with *{version}*, **merge** the *{version*} branch into the **develop** branch and into the **master** branch. It also tags the **master** branch with the **hotfix** version.
