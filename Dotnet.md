# Fixing the HTTPS Developer Certificate Error in .NET on macOS Sequoia

MacOS 15 Sequoia has introduced changes to its security APIs which has broken the dotnet CLI's ability to generate and trust HTTPS developer certificates.

The command dotnet dev-certs https fails with the message:

```
There was an error creating the HTTPS developer certificate.
```

## Workaround

1) Just in case, delete any certs that currently exist. Open a terminal and run: 

```zsh
% dotnet dev-certs https --clean
```

2) Download the tar.gz file of the "main" release from the .NET SDK package table. You can also access the links directly below.

https://aka.ms/dotnet/9.0.1xx/daily/dotnet-sdk-osx-arm64.tar.gz
https://aka.ms/dotnet/9.0.1xx/daily/dotnet-sdk-osx-x64.tar.gz

3) Unpack the downloaded file.

4) Remove the quarantine attribute from the unpacked folder. From your terminal run:

```zsh
% xattr -d com.apple.quarantine -r <folderName>
% xattr -d com.apple.quarantine -r dotnet-sdk-9.0.100-rc.2.24473.22-osx-arm64
```

5) Navigate to the unpacked folder: cd dotnet-sdk-9.0.100-rc.2.24473.22-osx-x64

6) From within this folder, run the following to generate and trust the certificate. 

```zsh
% ./dotnet dev-certs https --trust
```

Note the ./ before dotnet – this ensures you're using the version you just downloaded, not the globally installed one.

# Dotnet SDK

The .NET SDK allows you to develop apps with .NET. If you install the .NET SDK, you don't need to install the corresponding runtime. The ASP.NET Core Runtime allows you to run apps that were made with .NET that didn't provide the runtime. As an alternative to the ASP.NET Core Runtime, you can install the .NET Runtime, which doesn't include ASP.NET Core support.

# Dotnet Uninstall Tool

The .NET Uninstall tool automates removing .NET SDKs and runtimes from your system. The tool supports Windows and macOS. Linux isn't supported.
https://learn.microsoft.com/en-us/dotnet/core/additional-tools/uninstall-tool-overview?pivots=os-macos
    1. Download the tar.gz file below.
    2. Open terminal and change working directory to the directory with dotnet-core-uninstall.tar.gz.
    3. Use the following commands to install the tool and show help:
        ◦ mkdir -p ~/dotnet-core-uninstall
        ◦ tar -zxf dotnet-core-uninstall.tar.gz -C ~/dotnet-core-uninstall
        ◦ cd ~/dotnet-core-uninstall
        ◦ ./dotnet-core-uninstall -h

# Dotnet Developer Certificate

To generate a developer certificate run 'dotnet dev-certs https'. To trust the certificate (Windows and macOS only) run 'dotnet dev-certs https --trust'.

```zsh
% dotnet dev-certs https --trust
```

# dotnet-aspnet-codegenerator

The dotnet aspnet-codegenerator command runs the ASP.NET Core scaffolding engine. Running the dotnet aspnet-codegenerator command is required to scaffold from the command line or when using Visual Studio Code. 

```zsh
% dotnet tool install --global dotnet-aspnet-codegenerator
% dotnet tool update -g dotnet-aspnet-codegenerator
% dotnet tool uninstall -g dotnet-aspnet-codegenerator
```

If you are using zsh, you can add it to your profile by running the following command:

```zsh
cat << \EOF >> ~/.zprofile
# Add .NET Core SDK tools
export PATH="$PATH:/Users/emre/.dotnet/tools"
EOF
```

And run `zsh -l` to make it available for current session. You can only add it to the current session by running the following command:

```zsh
% export PATH="$PATH:/Users/emre/.dotnet/tools"
```

https://learn.microsoft.com/en-us/aspnet/core/fundamentals/tools/dotnet-aspnet-codegenerator?view=aspnetcore-8.0

# Samples

Change the current working directory to the project folder 

## Create MVC Controller 
dotnet aspnet-codegenerator -p . controller -name DemoController -outDir .\Controllers 

## Create WebApi Controller 
dotnet aspnet-codegenerator -p . controller -name DemoController -outDir .\Controllers -api 

# Publish .NET apps with the .NET CLI

Applications you create with .NET can be published in two different modes; self-contained or framework-dependent, and the mode affects how a user runs your app.

Publishing your app as self-contained produces an application that includes the .NET runtime and libraries, and your application and its dependencies. Self-contained executable produces a platform-specific executable and includes a local copy of the .NET runtime. Users of the application can run it on a machine that doesn't have the .NET runtime installed.

Publishing your app as framework-dependent produces an application that includes only your application itself and its dependencies. Users of the application have to separately install the .NET runtime. Framework-dependent deployment produces a cross-platform .dll file that uses the locally installed .NET runtime. Framework-dependent executable produces a platform-specific executable that uses the locally installed .NET runtime.

## Framework-dependent deployment (FDD)

When you publish your app as an FDD, a <PROJECT-NAME>.dll file is created in the ./bin/<BUILD-CONFIGURATION>/<TFM>/publish/ folder. To run your app, navigate to the output folder and use the dotnet <PROJECT-NAME>.dll command.

```zsh
# Framework-dependent deployment	
% dotnet publish -c Release -p:UseAppHost=false
```

The default build configuration is Debug, so this command specifies the Release build configuration. 

## Framework-dependent executable (FDE)

Framework-dependent executable (FDE) is the default mode for the basic dotnet publish command. You don't need to specify any other parameters, as long as you want to target the current operating system.

In this mode, a platform-specific executable host is created to host your cross-platform app. This mode is similar to FDD, as FDD requires a host in the form of the dotnet command. The host executable filename varies per platform and is named something similar to `<PROJECT-FILE>.exe`. You can run this executable directly instead of calling dotnet `<PROJECT-FILE>.dll`, which is still an acceptable way to run the app.

```zsh
# Framework-dependent executable	
% dotnet publish -c Release -r <RID> --self-contained false
% dotnet publish -c Release
```

Whenever you use the -r switch, the output folder path changes to: ./bin/<BUILD-CONFIGURATION>/<TFM>/<RID>/publish/

## Self-contained deployment (SCD)

When you publish a self-contained deployment (SCD), the .NET SDK creates a platform-specific executable. Publishing an SCD includes all required .NET files to run your app but it doesn't include the native dependencies of .NET (for example, for .NET 6 on Linux or .NET 8 on Linux). These dependencies must be present on the system before the app runs.

You must use the following switches with the dotnet publish command to publish an SCD:

```
-r <RID>
--self-contained true
```

```zsh
# Self-contained deployment	
dotnet publish -c Release -r <RID> --self-contained true
```

If you simply run dotnet publish all files are created...

## Runtime Identifier (RID)

```zsh
# Windows RIDs
win-x64
win-x86
win-arm64

# Linux RIDs
linux-x64 Most desktop distributions like CentOS, Debian, Fedora, Ubuntu, and derivatives
linux-musl-x64 Lightweight distributions using musl like Alpine Linux
linux-musl-arm64 Used to build Docker images for 64-bit Arm v8 and minimalistic base images
linux-arm Linux distributions running on Arm like Raspbian on Raspberry Pi Model 2+
linux-arm64 Linux distributions running on 64-bit Arm like Ubuntu Server 64-bit on Raspberry Pi Model 3+
linux-bionic-arm64 Distributions using Androids bionic libc, for example, Termux

# macOS RIDs
osx-x64 (Minimum OS version is macOS 10.12 Sierra)
osx-arm64

# iOS RIDs
ios-arm64

# Android RIDs
android-arm64

# On any platform, run the app by using the dotnet command:
dotnet project-name.dll

# On MacOS or Linux, enter 
./project-name
```