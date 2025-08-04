# Rebinding the Project to a Git Repository

If you accidentally deleted the `.git` folder in your project, you can re-bind your source code to the git repository.

## Method 1: Re-linking the Project to an Existing GitHub Repository (Keep Local Changes + Keep GitHub Repository History)

Open a command prompt and run the following commands:

```bash
# Navigate to the project folder
PS> cd /project-folder

# Initialize git repository
PS> git init

# If the repository on GitHub is using the main branch (it probably is):
PS> git branch -M main

# Upate the repository url
PS> git remote add origin https://github.com/existing-repository.git

# Fetch
PS> git fetch origin

# Create a new branch to save your local changes (safety step). This avoids losing local work.
PS> git checkout -b local-backup
PS> git add .
PS> git commit -m "Backup: my local untracked files before merge"

# Switch to the branch you want to merge with (main)
PS> git checkout -b main origin/main

# Merge your local backup into the fetched GitHub branch
PS> git merge local-backup

# Now Git will try to merge your local files into the GitHub repo. If there are file conflicts, Git will let you resolve them manually.

# Push the updated merged project back to GitHub
PS> git push origin main
```

You now should have the GitHub history and your local changes.

## Method 2: Cloning Github Repository fromscratch

This method is simply a standart cloning of a Github repository. Open a command prompt and run the following commands:

```bash
PS> git clone https://github.com/existing-repository.git
```

This will download the entire repository from GitHub, include all commit history, branches, and tags, areate a .git folder linked to GitHub, and put the contents into a new folder named after the repository.

WARNING: If you have local files not send to git or existing files which are updated after the last commit, they are not recovered.

BUT: If you clone the Github repository in a different folder, then you can compare its content with the existing project folder using a folder compare tool like Beyond Compare, you can manually pick the updates and apply to newly created folder, then you can push them to repository using new clone.

## Method 3: If your local files are newer and should overwrite GitHub

Open a command prompt and run the following commands:

```bash
PS> git init
PS> git remote add origin https://github.com/existing-repository.git
PS> git add .
PS> git commit -m "Reconnect project to existing GitHub repo"
PS> git branch -M main  
PS> git push -f origin main  
```

This will update git repository with local files and folders.

## Method 4: If your Github files are newer and should overwrite local files

Open a command prompt and run the following commands:

```bash
PS> git init
PS> git remote add origin https://github.com/existing-repository.git
# Download the commit history from the remote repo
PS> git fetch 
# Reset your local files to match the remote main branch exactly, all your local files will be replaced by what's currently on GitHub
PS> git reset origin/main 

```

WARNING: Any local changes or new files will be lost unless committed or backed up.