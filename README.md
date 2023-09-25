# AHEAD Git/Github Demo

Git/Github demo.

## GitHub CLI 

### Install the GitHub CLI:

If you haven't already, [download and install the Github CLI](https://pages.github.com/). At minimum, the Github CLI will allow you to authenticate, create, and clone code repositories. However, the CLI also provides functionality to create and manage Pull Requests, Releases, Gists, and more.

### Authenticate with GitHub

Open your terminal and run the following command to authenticate with your GitHub account.

```
$ gh auth login

? What account do you want to log into? GitHub.com
? What is your preferred protocol for Git operations? HTTPS
? Authenticate Git with your GitHub credentials? Yes
? How would you like to authenticate GitHub CLI? Login with a web browser

! First copy your one-time code: 842R-6OSP
Press Enter to open github.com in your browser...
```

Follow the above prompts and navigate to the browser to complete the authentication process.

A successful authentication will look like such.

```
✓ Authentication complete.
- gh config set -h github.com git_protocol https
✓ Configured git protocol
✓ Logged in as nathantym
```

## Cloning a Repository

### Clone a Repository

Cloning a code repository will download its remote contents into the current working directory on your local machine. 

```
$ gh repo clone nathantym/git-demo

Cloning into 'git-demo'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (3/3), done.

$ ls

git-demo
```

### Navigate to the Repository

Finally, change your current working directory to the cloned repository.

```
$ cd git-demo
$ ls
README.md
```

## Pulling Changes from Remote

### Pull Latest Changes

Initially, clone will download the most current version of the repository and place you on the main branch. The main branch of the repository is often called main, but can often be seen as master - as of recent, the standard is to name, and even rename, the branch main. 

As many developers can be working within the same repository, the main branch, and even other branches, will often be updated frequently. To ensure the local repository has the latest code, the git pull command can be used to pull any code that the local repository may not have.

Pull is a single git command, but its underlying functionality is running two other commands. First, Fetch is run to download the contents from the local repository. Next, Merge is run to take the new refs and merge them into a new local merge commit. Fetch and Merge are worth looking into, but Pull makes this an easy process. 

```
$ git pull origin main
```

## Creating a Branch

### Create a feature branch

Chheckout is a command used to switch to another local branch. Adding the -b flag will do the same functionality as the branch command, which is to simply create a branch, but it will create the branch and automatically switch to it. 

The branch command also provides functionality to check what branch you are on. Prior to creating a branch, the following command can be run to verify that you are on the correct branch to branch off of (if you are on the main branch and create a branch feature/feature_name, it will contain all of the code from the main branch.)

```
$ git branch
* main
```

And then creating the new branch.

```
$ git checkout -b feature/feature_name

Switched to a new branch 'feature/feature_name'

# short hand for
# git branch feature/feature_name
# git checkout feature/feature_name
```

As a side note, the feature/ part of the branch name is a common convention for branch naming. Other examples are fix/ chore/ ci/.

## Committing Code

### Make Code Changes

Update code or any other necessary files to be pushed into the repository.

### Check the Status of Updated Code

After making updates to the codebase, and prior to updating the remote codebase, the status command can be used to check what files have been updated. 

```
$ git status

On branch feature/feature_name
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	hello.py

nothing added to commit but untracked files present (use "git add" to track)
```

### Add Updated Files

In the above example, we can see that there has been one new file updated. Before pushing the changes out to the remote repository, updated files must be added to the staging area.

```
# add single file
$ git add file1.py

# add multiple files
$ git add file1.py file2.py

# add all files
$ git add .
```

### Commit Files

After files are added to the staging area, they can be committed to the local repository history. The -m flag will add a message/log alongside the commit.

```
$ git commit -m 'feature: updated script'

[feature/feature_name cae7ce6] feature: updated script
 1 file changed, 1 insertion(+)
 create mode 100644 hello.py

$ git status

On branch feature/feature_name
nothing to commit, working tree clean
```

Lastly, the log command can be used to review the commit history on the current branch.

```
$ git log

commit cae7ce6cd140259c22f88b462a2eb86c60f3df76
Author: Nathan Tym <nathantym@gmail.com>
Date:   Mon Sep 25 10:25:19 2023 -0500

    feature: updated script

commit 6f9940b4660112d13352b4026c8b515ba0029bc5 (origin/main, origin/HEAD, main)
Author: Nathan <nathantym@gmail.com>
Date:   Sun Sep 24 15:53:16 2023 -0500

    Initial commit
```

### Optional: Squash Commits

As a best practice, branch names, commit messages, and other titles or messages should be concise. It may be the case that multiple commits need to be made, and perhaps the messages are not always such. The squash command can be used to take multiple commits and merge them into one, while applying one message for all of the commits. 

```
$ git log

commit 13a97a60dd35d03fc757cd96b3340723b20858fe (HEAD -> feature/feature_name)
Author: Nathan Tym <nathantym@gmail.com>
Date:   Mon Sep 25 10:28:14 2023 -0500

    feature: updated script again again

commit 5b94a531aa23591003b0ae4752f4a100f56c8075
Author: Nathan Tym <nathantym@gmail.com>
Date:   Mon Sep 25 10:27:23 2023 -0500

    feature: updated script again

commit cae7ce6cd140259c22f88b462a2eb86c60f3df76
Author: Nathan Tym <nathantym@gmail.com>
Date:   Mon Sep 25 10:25:19 2023 -0500

    feature: updated script

commit 6f9940b4660112d13352b4026c8b515ba0029bc5 (origin/main, origin/HEAD, main)
Author: Nathan <nathantym@gmail.com>
Date:   Sun Sep 24 15:53:16 2023 -0500

    Initial commit
```

In the above example, we have three commits that are not concise; they do not tell us in a clear manner what the change was, nor does it do such in a clean format.

By using the rebase command, with the number of commits (HEAD~3) we want to squash, we can edit the history and ensure the commit(s) are clean.

```
$ git rebase -i HEAD~3

[detached HEAD a348978] feature: updated logging in script
 Date: Mon Sep 25 10:25:19 2023 -0500
 1 file changed, 2 insertions(+)
 create mode 100644 hello.py
Successfully rebased and updated refs/heads/feature/feature_name.
```

## Push Changes

### Push Code

Finally, the push command can be used to push the changes to the remote repository. 

```
$ git push origin feature/feature_name

Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 16 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 335 bytes | 335.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
remote:
remote: Create a pull request for 'feature/feature_name' on GitHub by visiting:
remote:      https://github.com/nathantym/git-demo/pull/new/feature/feature_name
remote:
To https://github.com/nathantym/git-demo.git
 * [new branch]      feature/feature_name -> feature/feature_name
```

## Optional: Create Pull Requeat

### Pull Request

In the above example, [the link can be used to navigate to the page](https://github.com/nathantym/git-demo/pull/new/feature/feature_name) to create a Pull Request. 








