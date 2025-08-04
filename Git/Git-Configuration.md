# Git global configuration

```bash
PS> git config --global user.name "Emre Mumcu"
PS> git config --global user.email "emumcu@outlook.com"
PS> git config --global init.defaultBranch main

# If you are working locally and having untrusted SSL Certificates in certificate chain and you want to disable the SSL verfication:
PS> git config --global http.sslVerify false

# Setting the post buffer for a large repository (To resolve: RPC failed; HTTP 400 curl 22 The requested URL returned error: 400 send-pack: unexpected disconnect while reading sideband packet fatal: the remote end hung up unexpectedly)
PS> git config --global http.postBuffer 1g
```