# GIT

https://www.atlassian.com/git/tutorials/

## What is version control?
Version control, also known as source control, is the practice of tracking and managing changes to software code. Version control software keeps track of every modification to the code in a special kind of database.

The primary benefits you should expect from version control are as follows:

1. A complete long-term change history of every file. 
2. Branching and merging.
3. Traceability.

By far, the most widely used modern version control system in the world today is Git. Git is a mature, actively maintained open source project originally developed in 2005 by Linus Torvalds, the famous creator of the Linux operating system kernel.

Having a distributed architecture, Git is an example of a DVCS (hence Distributed Version Control System). Rather than have only one single place for the full version history of the software as is common in once-popular version control systems like CVS or Subversion (also known as SVN), in Git, every developer's working copy of the code is also a repository that can contain the full history of all changes.

Git repository are secured with a cryptographically secure hashing algorithm called SHA1. 

## Setting up a repository
A Git repository is a virtual storage of your project. It allows you to save versions of your code, which you can access when needed. 

### Initializing a new repository: git init
To create a new repo, you'll use the `git init` command. `git init` is a one-time command you use during the initial setup of a new repo.

Executing this command will create a new .git subdirectory in your current working directory. This will also create a new main branch. A HEAD file is also created which points to the currently checked out commit.

```zsh
% git init
% git init project-directory
```

#### Bare repositories --- git init --bare
The `--bare` flag creates a repository that doesn’t have a working directory, making it impossible to edit files and commit changes in that repository. You would create a bare repository to git push and git pull from, but never directly commit to it. Central repositories should always be created as bare repositories because pushing branches to a non-bare repository has the potential to overwrite changes. Think of `--bare` as a way to mark a repository as a storage facility, as opposed to a development environment.

```zsh
% git init --bare directory
```

#### git init templates

```zsh
% git init directory --template=template_directory
```

Initializes a new Git repository and copies files from the  ＜template_directory＞ into the repository.

### Cloning an existing repository: git clone
If a project has already been set up in a central repository, the clone command is the most common way for users to obtain a local development clone. Like git init, cloning is generally a one-time operation. Once a developer has obtained a working copy, all version control operations are managed through their local repository.

```zsh
% git clone repo-url
% git clone repo-url target-directory
# Clone the repository located at ＜repo＞ and only clone the ref for tag
% git clone --branch tag repo-url
```

When executed, the latest version of the remote repo files on the main branch will be pulled down and added to a new folder.

The -branch argument lets you specify a specific branch to clone instead of the branch the remote HEAD is pointing to, usually the main branch. In addition you can pass a tag instead of branch for the same effect.

### Saving changes to the repository: git add and git commit
Now that you have a repository cloned or initialized, you can commit file version changes to it.

The git add command adds a change in the working directory to the staging area. It tells Git that you want to include updates to a particular file in the next commit. However, git add doesn't really affect the repository in any significant way—changes are not actually recorded until you run git commit.

Developing a project revolves around the basic edit/stage/commit pattern. First, you edit your files in the working directory. When you’re ready to save a copy of the current state of the project, you stage changes with git add. After you’re happy with the staged snapshot, you commit it to the project history with git commit. The git reset command is used to undo a commit or staged snapshot.

In addition to git add and git commit, a third command git push is essential for a complete collaborative Git workflow. git push is utilized to send the committed changes to remote repositories for collaboration. This enables other team members to access a set of saved changes.

The primary function of the git add command, is to promote pending changes in the working directory, to the git staging area. The staging area is one of Git's more unique features.

```zsh
% git add file      #
% git add directory #
% git add -A        # Stage all files in the entire repository
% git add .         # Stage all files in the entire directory
% git add -u        # Stage modified and deleted files only in the entire repository
```

The git commit command captures a snapshot of the project's currently staged changes.

```zsh
% git commit -m "commit message"
```

### Repo-to-repo collaboration: git push
It’s important to understand that Git’s idea of a “working copy” is very different from the working copy you get by checking out source code from an SVN repository. Unlike SVN, Git makes no distinction between the working copies and the central repository—they're all full-fledged Git repositories.

Whereas SVN depends on the relationship between the central repository and the working copy, Git’s collaboration model is based on repository-to-repository interaction. Instead of checking a working copy into SVN’s central repository, you push or pull commits from one repository to another.

### Configuration & set up: git config
Git stores configuration options in three separate files, which lets you scope options to individual repositories (local), user (Global), or the entire system (system).

The git config command can accept arguments to specify which configuration level to operate on. The following configuration levels are available:

* --local: By default, git config will write to a local level if no configuration option is passed. Local configuration values are stored in a file that can be found in the repo's .git directory: `.git/config`
* --global: Global level configuration is user-specific, meaning it is applied to an operating system user. Global configuration values are stored in a file that is located in a user's home directory. `~/.gitconfig` on unix systems and `C:\Users\User\.gitconfig` on windows.
* --system: System-level configuration is applied across an entire machine. This covers all users on an operating system and all repos. The system level configuration file lives in a gitconfig file off the system root path. `$(prefix)/etc/gitconfig` on unix systems. On windows this file can be found at `C:\Documents and Settings\All Users\Application Data\Git\config` on Windows XP, and in `C:\ProgramData\Git\config` on Windows Vista and newer.  

Git supports colored terminal output which helps with rapidly reading Git output. You can customize your Git output to use a personalized color theme. The git config command is used to set these color values. By default, `color.ui` is set to auto which will apply colors to the immediate terminal output stream.


















## Resetting, checking out & reverting

### Revert

A revert is an operation that takes a specified commit and creates a new commit which inverses the specified commit. git revert can only be run at a commit level scope and has no file level functionality. This command creates a new commit that undoes the changes from a previous commit. This command adds new history to the project (it doesn't modify existing history).

### Checkout

A checkout is an operation that moves the HEAD ref pointer to a specified commit. The git checkout command can be used in a commit, or file level scope. A file level checkout will change the file's contents to those of the specific commit. This command checks-out content from the repository and puts it in your work tree. It can also have other effects, depending on how the command was invoked. For instance, it can also change which branch you are currently working on. This command doesn't make any changes to the history.


Checkout and reset are generally used for making local or private 'undos'. They modify the history of a repository that can cause conflicts when pushing to remote shared repositories. 

### Revert

Revert is considered a safe operation for 'public undos' as it creates new history which can be shared remotely and doesn't overwrite history remote team members may be dependent on.

You can also think of git revert as a tool for undoing committed changes, while git reset HEAD is for undoing uncommitted changes. Like git checkout , git revert has the potential to overwrite files in the working directory, so it will ask you to commit or stash changes that would be lost during the revert operation.

### Using these commands

If a commit has been made somewhere in the project's history, and you later decide that the commit is wrong and should not have been done, then git revert is the tool for the job. It will undo the changes introduced by the bad commit, recording the "undo" in the history.

If you have modified a file in your working tree, but haven't committed the change, then you can use git checkout to checkout a fresh-from-repository copy of the file.

If you have made a commit, but haven't shared it with anyone else and you decide you don't want it, then you can use git reset to rewrite the history so that it looks as though you never made that commit.

# Git RM

The git rm command can be used to remove individual files or a collection of files. The primary function of git rm is to remove tracked files from the Git index. Additionally, git rm can be used to remove files from both the staging index and the working directory. There is no option to remove a file from only the working directory. The files being operated on must be identical to the files in the current HEAD. If there is a discrepancy between the HEAD version of a file and the staging index or working tree version, Git will block the removal. This block is a safety mechanism to prevent removal of in-progress changes.

## Usage

```zsh
% git rm <options> <file-or-folder>
```
<file-or-folder>: Specifies the target files to remove. The option value can be an individual file, a space delimited list of files file1 file2 file3, or a wildcard file glob (~./directory/*).

**Options:**

-f/--force: is used to override the safety check that Git makes to ensure that the files in HEAD  match the current content in the staging index and working directory.

-n/--dry-run: is a safeguard that will execute the git rm command but not actually delete the files. Instead it will output which files it would have removed.

-r: is shorthand for 'recursive'. When operating in recursive mode git rm will remove a target directory and all the contents of that directory.

--: The separator option is used to explicitly distinguish between a list of file names and the arguments being passed to git rm.

--cached: specifies that the removal should happen only on the staging index. Working directory files will be left alone.

## How to undo git rm

Executing git rm is not a permanent update. The command will update the staging index and the working directory. These changes will not be persisted until a new commit is created and the changes are added to the commit history. This means that the changes here can be "undone" using common Git commands.

```zsh
% git reset HEAD
```

A reset will revert the current staging index and working directory back to the HEAD commit. This will undo a git rm.

```zsh
% git checkout .
```

A checkout will have the same effect and restore the latest version of a file from HEAD.

In the event that git rm was executed and a new commit was created which persist the removal, git reflog can be used to find a ref that is before the `git rm` execution.

**Sample**

```zsh
% git rm -r .DS_Store
% git commit -m ".DS_Store Removed"
% git push origin master
```

## Git Reflog

Git keeps track of updates to the tip of branches using a mechanism called reference logs, or "reflogs."

```zsh
% git reflog
```

# Abstract

```zsh
# Git global configuration
% git config --global user.name "Emre Mumcu"
% git config --global user.email "emumcu@outlook.com"
% git config --global http.sslVerify false
% git config --global init.defaultBranch main
# For Windows: If you use wincred for credential.helper, git is using the standard windows Credential Manager to store your credentials.
% git config --global credential.helper wincred

# NOTE: Pushing a large repository to remote github can cause the following error:
# error: RPC failed; HTTP 400 curl 22 The requested URL returned error: 400
# send-pack: unexpected disconnect while reading sideband packet
# fatal: the remote end hung up unexpectedly

# Use the folloeing beffer command to resolve thr error:
% git config --global http.postBuffer 1g
```

```zsh
# Fist Commit (Alternative 1)
% git init -b main
% git add -A
% git commit -m "First Commit"
% git remote add origin https://github.com/emre-mumcu/my-repo.git
% git remote -v
% git push -u origin main

# Fist Commit (Alternative 2)
% git init
% git add -A
% git commit -m "First Commit"
% git branch -M main
% git remote add origin https://github.com/my-repo.git
% git remote -v
% git push -u origin main

# Following Commits
% git add -A
% git commit -m “Second Commit”
% git push -u origin main

# Deleting your credentials via the command line
% git credential-osxkeychain erase
host=github.com
protocol=https
# [Press Return]

# To stop tracking a file and ignore it in Git
% git rm --cached example.txt
% git commit -m "Stop tracking example.txt"
# Add the file to .gitignore
example.txt

# To stop tracking and ignore a folder
% git rm -r --cached example_folder
% git commit -m "Stop tracking example_folder"
# Add the folder to .gitignore
example_folder/
```

### Notes

* git add -A 	stages all changes (git add --all)
* git add . 	stages new files and modifications, without deletions (on the current directory and its subdirectories).
* git add -u 	stages modifications and deletions, without new files
* git add -A 	is equivalent to `git add .` and `git add -u`


The important point about `git add .` is that it looks at the working tree and adds all those paths to the staged changes if they are either changed or are new and not ignored, it does not stage any 'rm' actions.

`git add -u` looks at all the already tracked files and stages the changes to those files if they are different or if they have been removed. It does not add any new files, it only stages changes to already tracked files.

`git add -A` is a handy shortcut for doing both of those.

`git add .` only affects the current directory and subdirectories, if this command is run in a sub folder, the subfolder and its childs will be affected, but parent folders will not be affected.
`git add -A` affects all directory tree

Running `git add .` in the root folder will be same result as `git add -A`

```zsh
git status
git remote show origin
git config --get remote.origin.url
git remote show
% git log --patch -- Snippets.md -> show the changes made in each commit.
% git log --follow -- Snippets.md -> history of a file that has been renamed
```

# Github Pages

## Step 1: Create a repository
Head over to GitHub and create a new public repository named username.github.io, where username is your username (or organization name) on GitHub. If the first part of the repository doesn’t exactly match your username, it won’t work, so make sure to get it right.

## Step 2: Clone the repository
Go to the folder where you want to store your project, and clone the new repository:

```zsh
git clone https://github.com/username/username.github.io
```

Enter the project folder and add an index.html file:

```zsh
% cd username.github.io
% echo "Hello World" > index.html
```

Add, commit, and push your changes:

```zsh
git add --all
git commit -m "Initial commit"
git push -u origin main
```

Fire up a browser and go to https://username.github.io

# Personel Access Tokens

Personal access tokens are an alternative to using passwords for authentication to GitHub when using the GitHub API or the command line.

GitHub currently supports two types of personal access tokens: fine-grained personal access tokens and personal access tokens (classic). GitHub recommends that you use fine-grained personal access tokens instead of personal access tokens (classic) whenever possible.

Fine-grained personal access tokens have several security advantages over personal access tokens (classic). Personal access tokens (classic) are less secure. However, some features currently will only work with personal access tokens (classic).

## Creating a fine-grained personal access token

* Verify your email address, if it hasn't been verified yet.
* In the upper-right corner of any page on GitHub, click your profile photo, then click  Settings.
* In the left sidebar, click  Developer settings.
* In the left sidebar, under  Personal access tokens, click Fine-grained tokens.
* Click Generate new token.
* Under Token name, enter a name for the token.
* Under Expiration, select an expiration for the token.
* Optionally, under Description, add a note to describe the purpose of the token.
* Under Resource owner, select a resource owner. The token will only be able to access resources owned by the selected resource owner. Organizations that you are a member of will not appear unless the organization opted in to fine-grained personal access tokens. For more information, see "Setting a personal access token policy for your organization."
* Optionally, if the resource owner is an organization that requires approval for fine-grained personal access tokens, below the resource owner, in the box, enter a justification for the request.
* Under Repository access, select which repositories you want the token to access. You should choose the minimal repository access that meets your needs. Tokens always include read-only access to all public repositories on GitHub.
* If you selected Only select repositories in the previous step, under the Selected repositories dropdown, select the repositories that you want the token to access.
* Under Permissions, select which permissions to grant the token. Depending on which resource owner and which repository access you specified, there are repository, organization, and account permissions. You should choose the minimal permissions necessary for your needs.
* Click Generate token.

## Creating a personal access token (classic)

* In the upper-right corner of any page on GitHub, click your profile photo, then click Settings.
* In the left sidebar, click  Developer settings.
* In the left sidebar, under  Personal access tokens, click Tokens (classic).
* Select Generate new token, then click Generate new token (classic).
* In the "Note" field, give your token a descriptive name.
* To give your token an expiration, select Expiration, then choose a default option or click Custom to enter a date.
* Select the scopes you'd like to grant this token. To use your token to access repositories from the command line, select repo. A token with no assigned scopes can only access public information. For more information, see "Scopes for OAuth apps."
* Click Generate token.
* Optionally, to copy the new token to your clipboard, click Copy.

## Using a personal access token on the command line

Once you have a personal access token, you can enter it instead of your password when performing Git operations over HTTPS.

For example, to clone a repository on the command line you would enter the following git clone command. You would then be prompted to enter your username and password. When prompted for your password, enter your personal access token instead of a password.

```zsh
% git clone https://github.com/USERNAME/REPO.git
Username: YOUR-USERNAME
Password: YOUR-PERSONAL-ACCESS-TOKEN
```

Personal access tokens can only be used for HTTPS Git operations. If your repository uses an SSH remote URL, you will need to switch the remote from SSH to HTTPS. 

If you are not prompted for your username and password, your credentials may be cached on your computer. You can update your credentials in the Keychain to replace your old password with the token. You'll need to update your saved credentials in the git-credential-osxkeychain helper if you change your username, password, or personal access token on GitHub.

## Updating your credentials via Keychain Access

* Click on the Spotlight icon (magnifying glass) on the right side of the menu bar.
* Type Keychain Access, then press the Enter key to launch the app.
* In Keychain Access, search for github.com.
* Find the "Internet password" entry for github.com.
* Edit or delete the entry accordingly.

## Deleting your credentials via the command line

Through the command line, you can use the credential helper directly to erase the keychain entry.

```zsh
% git credential-osxkeychain erase
host=github.com
protocol=https
# [Press Return]
```

If it's successful, nothing will print out. To test that it works, try and clone a private repository from GitHub.com. If you are prompted for a password, the keychain entry was deleted.

# Reconnect the Project to Git Repository

If you accidentally deleted a .git folder in your local system, and want to restore it from remote repository (e.g. on GitHub), then the following should work (assuming that you are in the root directory where you want to put .git folder):

```zsh
% git init
% git remote add origin <url>
% git fetch
% git reset origin/main
```

# Git Reference

Git is a free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency. You typically obtain a Git repository in one of two ways:

1) You can take a local directory that is currently not under version control, and turn it into a Git repository, or
2) You can clone an existing Git repository from elsewhere.

In either case, you end up with a Git repository on your local machine, ready for work. Before using git commands, set your username and email address for git configuration:

```zsh
# Open the command line.
# To set your global username/email configuration:
git config --global user.name "Emre Mumcu"
git config --global user.email "emre@mumcu.net"

# To set repository-specific username/email configuration:
# Run these commands in a folder containing .git folder
git config user.name "Emre Mumcu"
git config user.email "emre@mumcu.net"

# Verify your configuration by displaying your configuration file:
cat .git/config

# To disable SSL Verification:
git config --global http.sslVerify false
```

## Initializing a Repository in an Existing Directory

If you have a project directory that is currently not under version control and you want to start controlling it with Git, you first need to go to that project’s directory and then run the following command.

```zsh
% git init
```

This creates a new subdirectory named .git that contains all of your necessary repository files. At this point, nothing in your project is tracked yet. If you want to start version-controlling existing files, you should begin tracking files and do an initial commit. You can accomplish that with a few git add commands that specify the files you want to track, followed by a git commit:

```zsh
# First commit
% git add -A
% git commit -m "First Commit"
% git branch -M main
% git remote add origin https://****.git
% git remote -v
% git push -u origin master


# Following Commits
% git add -A
% git status
% git commit -m “Second Commit”
% git status
% git push -u origin master

# Notes
% git add -A # stages all changes (git add --all)
% git add .  # stages new files and modifications, without deletions (on the current directory and its subdirectories).
% git add -u # stages modifications and deletions, without new files
% git add -a # equivalent to [git add .]  + [git add -u]
```

The important point about `git add .` is that it looks at the working tree and adds all those paths to the staged changes if they are either changed or are new and not ignored, it does not stage any deleted ones. `git add -u` looks at all the already tracked files and stages the changes to those files if they are different or if they have been removed. It does not add any new files, it only stages changes to already tracked files. `git add -a` is a handy shortcut for doing both of those.

`git add .` only affects the current directory and subdirectories, if this command is run in a sub folder, the subfolder and its childs will be affected, but parent folders will not be affected. `git add -a` affects all directory tree. Running `git add .` in the root folder will be same result as `git add -a`

## Cloning an Existing Repository

If you want to get a copy of an existing Git repository, the command you need is `git clone`. Instead of getting just a working copy, Git receives a full copy of nearly all data that the server has. Every version of every file for the history of the project is pulled down by default when you run git clone. For example, if you want to clone the Git linkable library called libgit2, you can do so like this:

```zsh
% git clone https://github.com/myrepo.git
```

That creates a directory named myrepo, initializes a `.git` directory inside it, pulls down all the data for that repository, and checks out a working copy of the latest version. If you go into the new myrepo directory that was just created, you’ll see the project files in there, ready to be worked on or used. If you want to clone the repository into a directory named something other than myrepo, you can specify the new directory name as an additional argument:

```zsh
% git clone https://github.com/myrepo.git mynewrepo
```

That command does the same thing as the previous one, but the target directory is called mynewrepo.

Git has a number of different transfer protocols you can use. The previous example uses the `https://` protocol, but you may also see `git://` or `user@server:path/to/repo.git`, which uses the `SSH` transfer protocol. Getting Git on a Server will introduce all of the available options the server can set up to access your Git repository and the pros and cons of each.

# References
* https://www.atlassian.com/git/tutorials/setting-up-a-repository
* https://pages.github.com/ 