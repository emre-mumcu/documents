# How To Configure SSH Key-Based Authentication on a Linux Server

SSH, or secure shell, is an encrypted protocol used to administer and communicate with servers. While there are a few different ways of logging into an SSH server, SSH keys provide an extremely secure way of logging into your server.

SSH key pairs are two cryptographically secure keys that can be used to authenticate a client to an SSH server. Each key pair consists of a public key and a private key.

The private key is retained by the client and should be kept absolutely secret. Any compromise of the private key will allow anyone to log into servers that are configured with the associated public key without additional authentication. As an additional precaution, the key can be encrypted on disk with a passphrase.

The associated public key can be shared freely without any negative consequences. The public key can be used to encrypt messages that only the private key can decrypt. This property is employed as a way of authenticating using the key pair.

The **public key (id_rsa.pub)** is uploaded to a remote server that you want to be able to log into with SSH. The key is added to a special file within the user account you will be logging into called `~/.ssh/authorized_keys`.

When a client attempts to authenticate using SSH keys, the server can test the client on whether they are in possession of the private key. If the client can prove that it owns the private key, a shell session is spawned or the requested command is executed.

First of all lets ensure that SSH agent is running on remote server:

```bash
$ eval "$(ssh-agent -s)"
# Agent pid 2430 -> ssh-agent is running
# ssh-agent: command not found -> ssh-agent is NOT running
```

## Step 1. Creating SSH Keys

The first step to configure SSH key authentication to your server is to generate an SSH key pair on your local computer. To do this, we can use a special utility called ssh-keygen, which is included with the standard OpenSSH suite of tools.

Open the Terminal application on your computer and run the following command:

```zsh
% ssh-keygen -t rsa -b 4096 -C "email@mydomain.com"
```

If you had previously generated an SSH key pair, you may see a prompt that tells a key is already exists and if you want to overwrite it. If you choose to overwrite the key on disk, you will not be able to authenticate using the previous key anymore. Be very careful when selecting yes, as this is a destructive process that cannot be reversed.

Next, you will be prompted to enter a passphrase (optional) for the key. This is an optional passphrase that can be used to encrypt the private key file on disk. A passphrase is an optional addition but if you enter one, you will have to provide it every time you use this key.

By default, the keys will be stored in the `~/.ssh` directory within your user’s home directory. The private key will be called `id_rsa` and the associated public key will be called `id_rsa.pub`. The private key `id_rsa` should be keet secure, while public key `id_rsa.pub` can be shared. 

You now have a public and private key that you can use to authenticate. 

**The next step is to place the public key on your server so that you can use SSH key authentication to log in.**

## Setp 2. Copying an SSH Public Key (id_rsa.pub) to Your Server

There are multiple ways to upload your public key to your remote SSH server.

The simplest way to copy your public key to an existing server is to use a utility called ssh-copy-id. Because of its simplicity, this method is recommended if available.

### ssh-copy-id

The ssh-copy-id tool is included in the OpenSSH packages in many distributions, so you may already have it available on your local system. For this method to work, you must currently have password-based SSH access to your server.

To use the utility, you need to specify the remote host that you would like to connect to, and the user account that you have password-based SSH access to. This is the account where your public SSH key will be copied.

```zsh
% ssh-copy-id username@remote_host
```

After running the previous command, the utility will scan your local account for the `id_rsa.pub` key that we created earlier. When it finds the key, it will prompt you for the password of the remote user’s account. 

Type in the password (your typing will not be displayed for security purposes) and press ENTER. The utility will connect to the account on the remote host using the password you provided. It will then copy the contents of your `~/.ssh/id_rsa.pub` key into a file in the remote account’s home `~/.ssh` directory called authorized_keys.

```text
Output
Number of key(s) added: 1
```

At this point, your id_rsa.pub key has been uploaded to the remote account. You can continue onto the next section.

### Copying Your Public Key Manually

If you do not have ssh-copy-id available, but you have access to an account on your server, you can upload your keys manually:

To display the content of your `id_rsa.pub` key, type this into your local computer:

```zsh
% cat ~/.ssh/id_rsa.pub
```

You will see the key’s content, which may look something like this:

```text
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCqql6MzstZYh1TmWWv11q5O3pISj2ZFl9HgH1JLknLLx44+tXfJ7mIrKNxOOwxIxvcBF8PXSYvobFYEZjGIVCEAjrUzLiIxbyCoxVyle7Q+bqgZ8SeeM8wzytsY+dVGcBxF6N4JS+zVk5eMcV385gG3Y6ON3EG112n6d+SMXY0OEBIcO6x+PnUSGHrSgpBgX7Ks1r7xqFa7heJLLt2wWwkARptX7udSq05paBhcpB0pHtA1Rfz3K2B+ZVIpSDfki9UVKzT8JUmwW6NNzSgxUfQHGwnW7kj4jp4AT0VZk3ADw497M2G/12N0PPB5CnhHf7ovgy6nL1ikrygTKRFmNZISvAcywB9GVqNAVE+ZHDSCuURNsAInVzgYo9xgJDW8wUw2o8U77+xiFxgI5QSZX3Iq7YLMgeksaO4rBJEa54k8m5wEiEE1nUhLuJ0X/vh2xPff6SQ1BL/zkOhvJCACK6Vb15mDOeCSq54Cr7kvS46itMosi/uS66+PujOO+xt/2FWYepz6ZlN70bRly57Q06J+ZJoc9FfBCbCyYH7U/ASsmY095ywPsBo1XQ9PqhnN1/YOorJ068foQDNVpm146mUpILVxmq41Cj55YKHEazXGsdBIbXWhcrRf4G2fJLRcGUr9q8/lERo9oxRm5JFX6TCmj6kmiFqv+Ow9gI0x8GvaQ== email@mydomain.com
```

Once you have access to your account on the remote server, you should make sure the `~/.ssh` directory is created. This command will create the directory if necessary, or do nothing if it already exists:

```zsh
% mkdir -p ~/.ssh
```

Now, you can create or modify the `authorized_keys` file within this directory. You can add the contents of your `id_rsa.pub` file to the end of the `authorized_keys` file, creating it if necessary, using this:

```zsh
% nano ~/.ssh/authorized_keys
```

Copy and paste the public_key_string with the output from the `cat ~/.ssh/id_rsa.pub` command that you executed on your local system. It should start with ssh-rsa AAAA... or similar.

## Step 3 — Authenticating to Your Server Using SSH Keys

If you have successfully completed one of the procedures above, you should be able to log into the remote host without the remote account’s password.

The process is mostly the same:

```zsh
% ssh username@remote_host [-p PORT]
```

## Step 4 — Disabling Password Authentication on your Server

If you were able to login to your account using SSH without a password, you have successfully configured SSH key-based authentication to your account. However, your password-based authentication mechanism is still active, meaning that your server is still exposed to brute-force attacks.

Before completing the steps in this section, make sure that you either have SSH key-based authentication configured for the root account on this server, or preferably, that you have SSH key-based authentication configured for an account on this server with sudo access. This step will lock down password-based logins, so ensuring that you will still be able to get administrative access is essential.

Once the above conditions are true, log into your remote server with SSH keys, either as root or with an account with sudo privileges. Open the SSH daemon’s configuration file:

```zsh
% sudo nano /etc/ssh/sshd_config
```

Inside the file, search for a directive called `PasswordAuthentication`. This may be commented out. Uncomment the line by removing any `#` at the beginning of the line, and set the value to `no`. This will disable your ability to log in through SSH using account passwords:

```text
PasswordAuthentication no
```

Save and close the file when you are finished. To actually implement the changes we just made, you must restart the service.

```zsh
sudo systemctl restart ssh
```

After completing this step, you’ve successfully transitioned your SSH daemon to only respond to SSH keys.

# OPTIONAL: For MAC Users

Add Your Key to the Keychain: After creating SSH private and public keys, add private key to local machine's Key-Chain:

```zsh
% ssh-add --apple-use-keychain /path/to/your/private/key
```

**SSH Directory and Private Key Specifications**
 
SSH requires that private key files be restricted to the owner only for security reasons. The recommended permissions for an SSH private key are 0600, which means that only the owner can read and write the file.

```zsh
% chmod 600 /path/to/your/private/key
% chmod 600 ~/.ssh/authorized_keys
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


# References

* https://www.digitalocean.com/community/tutorials/how-to-configure-ssh-key-based-authentication-on-a-linux-server