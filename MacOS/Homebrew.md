# Homebrew

```zsh
# Install Homebrew
% /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Validate Install
% brew doctor

# List installed brews
% brew list

# Update
% brew update

# Configuration
% brew config
```

## Dotnet SDK

```zsh
# install
brew install --cask dotnet-sdk

# uninstall

# List installed .NET versions
dotnet --list-sdks
dotnet --list-runtimes

# To see how you installed dotnet, run the following commands and look for anything like dotnet or dotnet-sdk
brew list --cask
brew list

# Depending on how you installed dotnet (cask vs formula), use the coreespondig command (you CAN run both to ensure!):
brew uninstall --ignore-dependencies --force --cask dotnet
brew uninstall --ignore-dependencies --force dotnet-sdk

# To completely clean up:
rm -rf ~/.dotnet
rm -rf ~/.nuget
sudo rm -rf /usr/local/share/dotnet

# Also remove any exported path from your shell profile. Check ~/.zshrc, ~/.bash_profile, or ~/.bashrc for lines like:
export DOTNET_ROOT=$HOME/.dotnet
export PATH=$PATH:$HOME/.dotnet

# Remove or comment them out. Then run:
source ~/.zshrc

# Verify Removal
which dotnet
dotnet --version
```

## Jdk

```zsh
# Oracle Jdk
brew install --cask oracle-jdk
brew install --cask oracle-jdk@17
brew install --cask oracle-jdk@21

# Open Jdk
brew install java
brew install openjdk@8
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

## Pyhton

```zsh
% brew install python
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

# Popular Brews

```
wget
python3
youtube-dl
tree
docker
htop
bash
mariadb
sqlite
rabbitmq
httpie
certbot
nginx
zsh-syntax-highlighting
postgresql
gimp
links
midnight-commander
```