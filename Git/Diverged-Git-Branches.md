# Dealing with diverged git branches

```zsh
% git pull --rebase
% git status
% git push
% git pull
```

One of the most challenging problems in Git is when a local branch and a remote branch have diverged.

## What does “diverged” mean?

If you have a local main and a remote main, there are 4 basic configurations:

**1: up to date** 
The local and remote main branches are in the exact same place. Something like this:

```txt
a - b - c - d
            ^ LOCAL
            ^ REMOTE
```

**2: local is behind** 
Here you might want to git pull. Something like this:

```txt
a - b - c - d - e
    ^ LOCAL     ^ REMOTE
```

**3: remote is behind** 
Here you might want to git push. Something like this:

```txt
a - b - c - d - e
    ^ REMOTE    ^ LOCAL
```

**4: they’ve diverged**
This is the situation which is the topic of this document. It looks something like this:

```txt
a - b - c - d - e
        \       ^ LOCAL
         -- f 
            ^ REMOTE
```

## Recognizing when branches are diverged

There are 3 main ways to tell that your branch has diverged.

**way 1: git status**
The easiest way to is to run git fetch and then git status. You’ll get a message something like this:

```txt
$ git fetch
$ git status
On branch main
Your branch and 'origin/main' have diverged, <-- here's the relevant line!
and have 1 and 2 different commits each, respectively.
  (use "git pull" to merge the remote branch into yours)
```

**way 2: git push**
When I run git push, sometimes I get an error like this:

```txt
$ git push
To github.com:jvns/int-exposed
 ! [rejected]        main -> main (non-fast-forward)
error: failed to push some refs to 'github.com:jvns/int-exposed'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

This doesn’t always mean that local main and the remote main have diverged (it could just mean that my main is behind). So if that happens run `git fetch` and `git status` to check.

**way 3: git pull**
If I git pull when my branches have diverged, I get this error message:

```txt
$ git pull
hint: You have divergent branches and need to specify how to reconcile them.
hint: You can do so by running one of the following commands sometime before
hint: your next pull:
hint:
hint:   git config pull.rebase false  # merge
hint:   git config pull.rebase true   # rebase
hint:   git config pull.ff only       # fast-forward only
hint:
hint: You can replace "git config" with "git config --global" to set a default
hint: preference for all repositories. You can also pass --rebase, --no-rebase,
hint: or --ff-only on the command line to override the configured default per
hint: invocation.
fatal: Need to specify how to reconcile divergent branches.
```

1) If you set git config pull.rebase false, it’ll automatically start merging the remote main.
2) If you set git config pull.rebase true, it’ll automatically start rebasing onto the remote main
3) If you set git config pull.ff only, it’ll exit with the error fatal: Not possible to fast-forward, aborting.

There’s no “best” way to resolve branches that have diverged – it really depends on your workflow for git and why the situation is happening.

There are 3 alternatives for a solution of this problem:

1) To keep both sets of changes:

```zsh
# Alternate 1) This is to keep both sets of changes. It rebases main onto the remote main branch. You can configure `git config pull.rebase true` to do this automatically every time. (Alternate 1 preferred over Alternate 2)
% git pull --rebase

# Alternate 2) This starts a merge between the local and remote main and (if it succeeds) opens a text editor so that you can confirm that you want to commit the merge. 
% git pull --no-rebase
```

2) The remote changes are useless and to overwrite them with local: 

```zsh
# Alternate 1) Do this for private repositories where only committer is you. If the repository has many different committers, force-pushing can cause a lot of problems.
% git push --force

# Alternate 2) This makes sure that nobody else has changed the branch since the last time you pushed or fetched, so that you don’t accidentally blow their changes away.
% git push --force-with-lease
```

3) The local changes are useless and to overwrite them with remote: 

```zsh
% git reset --hard origin/main
```

You can use this as the reverse of `git push --force` (since there’s no `git pull --force`). Do this when you know that local work is deprecated and replace it with whatever’s on the remote branch.

# References

* https://jvns.ca/blog/2024/02/01/dealing-with-diverged-git-branches/