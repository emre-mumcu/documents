# Git Credential Manager

Git Credential Manager (GCM) is a tool that securely stores and manages credentials (like usernames, passwords, or access tokens) used to authenticate with remote Git repositories—especially those hosted on services like GitHub, GitLab, Azure DevOps, and Bitbucket.

When you perform an operation like git clone, git pull, or git push on a remote repository, Git may require authentication. GCM helps by:

* Storing your credentials securely (e.g., in Windows Credential Store, macOS Keychain, or Linux Secret Service).
* Reusing credentials so you don’t have to enter them every time.
* Supporting modern authentication methods like OAuth and personal access tokens (PAT), which are required by most Git hosting services now (instead of basic username/password auth).

## Check which credential manager is used

```bash
PS> git config --show-origin --get credential.helper
```

You might see outputs like:

* manager-core (modern Git Credential Manager)
* store (plain-text credential storage — not recommended)
* cache (temporary in-memory storage)
* Or something OS-specific like osxkeychain or wincred

*Always prefer GCM or system credential helpers over store, because store saves passwords in plain text.*

## Credential Manager Commands

```bash
# Check All Credential Helpers (Global + Local + System)
$ git config --list --show-origin | grep credential.helper
PS> git config --list --show-origin | findstr credential.helper

# Unset existing credential helper
PS> git config --global --unset credential.helper

# Enable Manager (Manager-Core) which is a Secure Credential Manager
PS> git config --global credential.helper manager

# Enable Store which is a NOT Secure Credential Manager
# Git saves credentials in plain text in .git-credentials file
PS> git config --global credential.helper store

# For Windows
PS> git config --global credential.helper wincred

# Check which credential.helper is active
PS> git config --show-origin --get credential.helper
```

```bash
# Get Existing Credentials
$ echo -e "protocol=https\nhost=github.com" | git credential fill
$ git credential-manager get
$ echo -e "protocol=https\nhost=github.com" | git credential-manager get
PS> "protocol=https`nhost=github.com`n" | git credential fill
PS> git credential-manager get
PS> "protocol=https`nhost=github.com" | git credential-manager get
```

```bash
# Clear Saved Credentials
$ echo -e "protocol=https\nhost=github.com" | git credential-manager erase
PS> echo "protocol=https`nhost=github.com" | git credential-manager erase
```


```bash
# Manually insert credentials using Git's credential store
# NOTE: store text at the end of the following commands does NOT mean the insecure store manager name, it is one of the options get/store/erase
$ printf "protocol=https\nhost=github.com\nusername=emre-mumcu\npassword=***\n" | git credential-manager store

PS> echo "protocol=https`nhost=github.com`nusername=emre-mumcu`npassword=***" | git credential-manager store
```

```bash

```

NOTE: echo -e enables interpretation of escape sequences like \n (newline).