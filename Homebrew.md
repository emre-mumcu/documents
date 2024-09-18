# Homebrew

```zsh
# Install Homebrew
% /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Validate Install
% brew doctor

# List installed brews
% brew list
```

## Dotnet SDK

```zsh
% brew install --cask dotnet-sdk
```

## Node.js

```zsh
# Install latest version
% brew install node

# Install specified version
% brew install node@20

# Validate install
% node -v
% npm -v
```

## Angular CLI

```zsh
# Install latest version
% brew install angular-cli

# Validate install
% ng version
```

## PGLoader

```zsh
# Install latest version
% brew install pgloader
```

## SevenZip

```zsh
# Install latest version
% brew install sevenzip

# Validate install
% 7zz --help

# Extract 7z file with password to specified destination
% 7zz x MyArch.7z -oDestination/Path -pPassword
```

## Git

```zsh
# Install latest version
% brew install git
```

## Mkcert

```zsh
# Install latest version
% brew install mkcert

# Install nss if you use Firefox
% brew install nss

# Install as CA Root (mkcert -CAROOT ??)
% mkcert -install

# Create PEM certificates
% mkcert localhost 127.0.0.1 ::1
% mkcert example.com "*.example.com" example.test

# Convert PFX certificates to PEM certificates
% openssl pkcs12 -inkey localhost-key.pem -in localhost.pem -export -out localhost.pfx	
% openssl pkcs12 -info -in localhost.pfx

# Create p12 certificate 
% mkcert -pkcs12 localhost

# Get info from p12 certificate
% openssl pkcs12 -info -in localhost.p12

# Convert p12 to PFX certificate
% openssl pkcs12 -in localhost.p12 -out localhost.pfx -nodes -password pass:***
```

## DBeaver-Community

```zsh
% brew install --cask dbeaver-community
```

## Fontawesome

```zsh
% brew install --cask font-fontawesome
```
