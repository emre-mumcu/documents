# Abstract

```zsh
# Git global configuration
% git config --global user.name "Emre Mumcu"
% git config --global user.email "emumcu@outlook.com"
% git config --global http.sslVerify false
% git config --global init.defaultBranch main
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

Creating a personal access token (classic)
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