# WinGet

WinGet is a command line tool enabling users to discover, install, upgrade, remove and configure applications on Windows 10, Windows 11, and Windows Server 2025 computers. This tool is the client interface to the Windows Package Manager service.

WinGet the Windows Package Manager is available on Windows 11, modern versions of Windows 10, and Windows Server 2025 as a part of the App Installer. The App Installer is a System Component delivered and updated by the Microsoft store on Windows Desktop versions, and via Updates on Windows Server 2025.

When running WinGet in an Administrator Command Prompt, you will not see elevation prompts if the application requires it. Always use caution when running your command prompt as an administrator, and only install applications you trust.

## Use WinGet

One of the most common usage scenarios is to search for and install a favorite tool.

To search for a tool, type winget search `appname`. After you have confirmed that the tool you want is available, you can install the tool by typing winget install `appname`. The WinGet tool will launch the installer and install the application on your PC.

```shell
# search for Google Chrome
PS> winget search Chrome

# Install Google Chrome
winget install -e --id Google.Chrome
# Chrome will be downloaded from:
# https://dl.google.com/dl/chrome/install/googlechromestandaloneenterprise64.msi
```

## Commands

WinGet tool supports the following commands.

| Command |	Description |
| --- | --- |
| info | Displays metadata about the system (version numbers, architecture, log location, etc). Helpful for troubleshooting. |
| install | Installs the specified application. |
| show | Displays details for the specified application. |
| source | Adds, removes, and updates the Windows Package Manager repositories accessed by the WinGet tool. |
| search | Searches for an application. |
| list | Display installed packages. |
| upgrade | Upgrades the given package. |
| uninstall | Uninstalls the given package. |
| hash | Generates the SHA256 hash for the installer. |
| validate | Validates a manifest file for submission to the Windows Package Manager repository. |
| settings | Open settings. |
| features | Shows the status of experimental features. |
| export | Exports a list of the installed packages. |
| import | Installs all the packages in a file. |
| pin | Manage package pins. |
| configure | Configures the system into a desired state. |
| download | Downloads the specified application's installer. |

```shell
PS> winget --info
PS> winget list
```

# References

* https://learn.microsoft.com/en-us/windows/package-manager/winget/