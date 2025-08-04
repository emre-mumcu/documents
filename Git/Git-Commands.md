





















## eee

git add .
Adds changes (new files, modified files, deleted files) in the current directory and its subdirectories.
It won’t stage deletions outside the current directory unless the deleted file is in the current directory tree.

git add -A (or --all)
Adds all changes in the entire working directory (new, modified, and deleted files), regardless of where you are in the project.
Safest when you want to stage everything, especially deletions, from any location in the repo.





The -u flag in the git push -u command stands for --set-upstream.

In Git, the -u or --set-upstream flag in the git push -u origin <branch> command does two important things:

Pushes your local branch to the remote repository (like git push origin <branch>).

Sets up a tracking relationship between your local branch and the remote branch.

After running git push -u origin <branch>, Git remembers that your local branch is linked to the remote branch (origin/<branch>).

In the future, you can simply use git push or git pull without specifying the remote or branch name, and Git will automatically know where to push/pull from.


Example:

git checkout -b new-feature  # Create and switch to a new branch
git push -u origin new-feature
After this:

The branch new-feature is pushed to the remote origin.

Git tracks origin/new-feature as the upstream branch for new-feature.

Later, you can just run git push instead of git push origin new-feature.

Without -u:
If you just run git push origin <branch>, Git pushes the branch but does not set up tracking. You'd have to specify the remote and branch every time.

Summary:
-u = --set-upstream (sets tracking for future pushes/pulls).

Makes future Git commands shorter (git push instead of git push origin branch).



git push -u origin feature/login
This pushes feature/login to the remote origin, and sets it so that future commands like git pull or git push will automatically use origin feature/login.

Without -u: You’d need to explicitly say:
git push origin feature/login
every time.


## fff


