# How To Remove Credentials From Git

Managing credentials securely is important for maintaining the security of your projects and systems. Sometimes, you might need to remove credentials from Git to prevent unauthorized access or to switch to different credentials.

## Removing Credentials from Git on Local Machine

1. Using Git Credential Helper

Git uses credential helpers to manage authentication. You can remove stored credentials by clearing the cache or removing specific credentials.

```bash
# To clear all stored credentials, use the following command
# This command clears all cached credentials from the credential helper
% git credential-cache exit
```

2. Clear Windows Credential Manager

Windows stores certain application and network credentials in the Credential Manager.

* Open Control Panel. 
* In the Control Panel, go to User Accounts > Credential Manager. 
* Under Windows Credentials or Generic Credentials, look for any entries related to Github. 
* Click on the entry and select Remove.
