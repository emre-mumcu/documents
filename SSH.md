# Using SSH Key

If you don’t already have an SSH key, you’ll need to generate one. Open the Terminal application on your Mac and use the following command:

```zsh
% ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

If you already have an SSH key and just need to use it to connect to a server from your macOS Terminal, follow these steps.

1) Esure the SSH agent is running:
% eval "$(ssh-agent -s)"
2) Add Your Key to the Keychain:
% ssh-add --apple-use-keychain /path/to/your/private/key
3) Connect to the Remote Server

```zsh
% ssh username@remote_server_address
```

# SSH Directory and Private Key Specifications
 
SSH requires that private key files be restricted to the owner only for security reasons. The recommended permissions for an SSH private key are 0600, which means that only the owner can read and write the file.

```zsh
% chmod 600 /path/to/your/private/key
```

Check the permissions of your private key to make sure they are set correctly:

```zsh
% ls -l /path/to/your/private/key

# [Optional] Fixing .ssh Directory Permissions
# Make sure the .ssh directory also has the correct permissions:
% chmod 700 ~/.ssh

# Verify Directory Permissions:
% ls -ld ~/.ssh
```

# Summary
* Private Key: Should have permissions 0600 (-rw-------).
* .ssh Directory: Should have permissions 0700 (drwx------).

# Codecamp

```zsh
% chmod 600 /Users/emre/Downloads/ssh-key-2024-09-16.key
% ssh-add --apple-use-keychain /Users/emre/Downloads/ssh-key-2024-09-16.key
% chmod 600 ~/.ssh/authorized_keys
% chmod 700 ~/.ssh

% ssh opc@89.168.98.232
```