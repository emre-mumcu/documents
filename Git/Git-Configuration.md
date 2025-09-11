# Git global configuration

```bash
PS> git config --global user.name "Emre Mumcu"
PS> git config --global user.email "emumcu@outlook.com"
PS> git config --global init.defaultBranch main
PS> echo "protocol=https`nhost=github.com`nusername=emre-mumcu`npassword=***" | git credential-manager store
```

```bash
# If you are working locally and having untrusted SSL Certificates in certificate chain and you want to disable the SSL verfication:
PS> git config --global http.sslVerify false

# Setting the post buffer for a large repository (To resolve: RPC failed; HTTP 400 curl 22 The requested URL returned error: 400 send-pack: unexpected disconnect while reading sideband packet fatal: the remote end hung up unexpectedly)
PS> git config --global http.postBuffer 1g
```


```bash
# The detected Git repository is potentially unsafe as the folder is owned by someone other than yhe current user.

# The above warning in Git usually shows up when Git detects that the repository’s folder is owned by a different Windows user account than the one you’re currently running Git as. This is a security feature (introduced in Git 2.35) to prevent attacks where a malicious repo could trick Git into executing hooks or configs from directories owned by someone else.

# Solutions

# 1. Fix file/folder ownership: Make sure the folder belongs to your current Windows user:

# * Right-click the repo folder → Properties → Security → Advanced.
# * Change Owner to your current username.
# * Apply recursively to all subfolders.

# After that, the warning should go away.

# 2. Mark the folder as “safe” for Git
git config --global --add safe.directory "C:/path/to/your/repo"

# 3. Mark all repos as “safe” for Git
git config --global --add safe.directory '*'
```