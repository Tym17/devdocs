---
layout: default
group: cloud
subgroup: 100_project
title: Manage branches
menu_title: Manage branches
menu_order: 20
menu_node:
version: 2.0
github_link: cloud/project/project-webint-branch.md
redirect_from:
  - /guides/v2.0/cloud/project/project-priv-repos.html
  - /guides/v2.1/cloud/project/project-priv-repos.html
  - /guides/v2.2/cloud/project/project-priv-repos.html
---

Every {{site.data.var.ece}} *environment* has an associated active Git *branch*. You can manage your environments using either the Project Web Interface, the Magento Cloud CLI, or Git commands. For more information on Git branchs, see [Git documentation](https://git-scm.com/doc).

For more information about managing environments using the CLI, see [Get started with an environment]({{page.baseurl}}cloud/env/environments-start.html).

This topic discusses how to use the Web Interface to:

*	Add or delete an environment
*	Sync (`git pull`) from the environment's parent
*	Merge (`git push`) to the environment's parent

## Add or delete an environment {#project-branch-add}
Complete development of code and added extensions in a branch and, when complete, merge (`git push`) the branch with its parent or master. For Starter, you want to create your development branches from `staging` using our recommended architecture structure. For Pro, you want to branch from Integration `master`.

For branching strategies, review [Start]({{page.baseurl}}cloud/basic-information/starter-architecture.html) and [Pro]({{page.baseurl}}cloud/basic-information/starter-develop-deploy-workflow.html) architecture overviews.

Your account supports a limited number of active Git branches and an unlimited number of inactive branches. Manage active and inactive branches by deleting a branch. When deleted, it is deactivated and available from the project branches list. You can either activate the branch later or you can [delete it entirely]({{page.baseurl}}cloud/howtos/environment-tutorial-env-merge.html#tut-env-delete) using the CLI.

## Add a branch {#add}
To add a branch:

1.	[Log in to your project]({{page.baseurl}}cloud/project/project-webint-basic.html#project-login).
2.	In the left navigation bar, click the name of the parent environment.

	Your new branch is cloned from this environment. Choose a parent environment that is similar to the environment you're about to create.
3.	Click ![Create a branch]({{ site.baseurl }}common/images/cloud_branch-icon.png){:width="30px"}.
4.	In the provided field, enter a branch name. In many cases, the environment name is the same as its ID.

	<div class="bs-callout bs-callout-info" id="info">
   		<p>The environment <em>name</em> is different from the environment <em>ID</em> only if you use spaces or capital letters in the environment name. An environment ID consists of all lowercase letters, numbers, and allowed symbols. Capital letters in an environment name are converted to lowercase in the ID; spaces in an environment name are converted to dashes.</p>
   		<p>An environment name <em>cannot</em> include characters reserved for your Linux shell or for regular expressions. Forbidden characters include curly braces (<code>{ }</code>), parentheses, asterisk (<code>*</code>), angle brackets (<code>&lt; ></code>), ampersand (<code>&</code>), percent (<code>%</code>), and other characters.</p>
 	</div>
5.	Click **Branch**.
6.	Wait while the environment deploys.

	During deployment, its status is **In process**, as the following figure shows.

	![Branch is deploying]({{ site.baseurl }}common/images/cloud_branch-deploy.png)

	After a successful deployment, the status changes to **Success**:

	![Branch is deploying]({{ site.baseurl }}common/images/cloud_branch-success.png)
7.	Continue with one of the following:

	*	[Get started with an environment]({{page.baseurl}}cloud/env/environments-start.html)
	*	[How tos and tutorials]({{ page.baseurl }}cloud/howtos/how-to.html)

## Delete to make a branch inactive {#inactive}
To delete an environment and make it inactive:

1.	[Log in to your project]({{page.baseurl}}cloud/project/project-webint-basic.html#project-login).
2.	In the left pane, click the name of the branch to delete.
3.	Click **Configure environment** as the following figure shows.

	![Configure environment]({{ site.baseurl }}common/images/cloud_project-env.png)
4.	Click the **Settings** tab.
5.	Click **Delete** next to the environment's status, as the following figure shows.

	![Delete an environment]({{ site.baseurl }}common/images/cloud_env-delete.png)

	A deleted (that is, inactive) environment displays with its name stricken out as the following figure shows.

	![Delete an environment]({{ site.baseurl }}common/images/cloud_environment-deleted.png)

## Sync from the environment's parent {#project-branch-sync}
Syncing an environment (or branch) is the same as `git pull origin <parent>`. You sync to get updated code from a parent environment.

To sync an environment with its parent:

1.	[Log in to your project]({{page.baseurl}}cloud/project/project-webint-basic.html#project-login).
2.	In the left pane, click the name of the branch you want to sync.
3.	Click ![Sync an environment]({{ site.baseurl }}common/images/cloud_environment-sync.png){:width="30px"} (sync).

	The following prompt displays:

	![Choose what to sync]({{ site.baseurl }}common/images/cloud_environment-sync2.png)
4.	Select the check box next to each item to sync and click **Sync**.

## Merge with the environment's parent {#project-branch-merge}
Merging an environment is the same as `git push origin`. You merge to push updated code from an environment to its parent environment (that is, a Git branch).

To merge an environment with its parent:

1.	[Log in to your project]({{page.baseurl}}cloud/project/project-webint-basic.html#project-login).
2.	In the left pane, click the name of the branch you want to merge.
3.	Click ![Merge an environment]({{ site.baseurl }}common/images/cloud_environment-merge.png){:width="30px"} (merge).
4.	Click **Merge** to confirm the action.

## Pull code from a private Git repository {#private}
Your {{site.data.var.ece}} project can include code located in a private Git repository. For example, a you may have code for a custom module or theme in a private repo. To do so, you must add your project's public SSH key to your private Git repository and update your project's `composer.json`.

To add a deployment key to your private GitHub repository, you must be the administrator of that repository. GitHub allows you to use a deploy key for one repository only.

If your project needs to access multiple repositories, you can choose to
attach an SSH key to an automated user account. Because this account won't
be used by a human, it's referred to as a [*machine user*](https://developer.github.com/guides/managing-deploy-keys/){:target="_blank"}. You can then add the
machine account as collaborator or add the machine user to a team with
access to the repositories it needs to manipulate.

<div class="bs-callout bs-callout-info" id="info">
We highly recommend adding and merging this code to your project Git repositories. If you do not configure the connection, you will have build issues.
</div>

### Find your deploy key {#ssh}
To find your project SSH public key (also referred to as a *deploy key*):

1.	Log in to your project using the Web Interface.
2.	Click **Configure Project**.
3.	Click **Deploy Key**.

	The following figure shows an example.

	![Deploy Key]({{ site.baseurl}}common/images/cloud_deploy-key.png){:width="500px"}

4.	Copy the deploy key to the clipboard.
5.	See [Enter your GitHub deploy key](#cloud-deploykey-github).

### Enter your GitHub deploy key {#cloud-deploykey-github}
On GitHub, deploy keys are read-only by default. Your Magento project won't push code to the private repository.

To enter your project's public key as a GitHub deploy key:

1.	Log in to your GitHub repository as its administrator.
2.	Click **Settings** as the following figure shows.

	![GitHub settings]({{ site.baseurl}}common/images/cloud_gh-settings.png){:width="650px"}

	<div class="bs-callout bs-callout-info" id="info">
  		<p>If you don't see this option, you're not the repository administrator and you cannot complete this task. Ask your GitHub project administrator to do this.</p>
	</div>

3.	On the Settings page, in the left navigation bar, click **Deploy Keys** as the following figure shows.

	![GitHub deploy key]({{ site.baseurl}}common/images/cloud_gh-deploy-key.png){:width="200px"}

4.	Click **Add deploy key**.
5.	Follow the prompts on your screen to complete the task.

<div class="bs-callout bs-callout-info" id="info">
  <p>In <code>composer.json</code>, use the <code>&lt;user>@&lt;host>:&lt;<path>.git</code> format, or <code>ssh://&lt;user>@&lt;host>:&lt;port>/&lt;path>.git</code> if using a non-standard port.</p>
</div>

#### Related topics
*	[Basic project information]({{page.baseurl}}cloud/project/project-webint-basic.html)
*	[Project backup and restore (snapshot)]({{page.baseurl}}cloud/project/project-webint-snap.html)
*	[Get started with a project]({{page.baseurl}}cloud/project/project-start.html)
