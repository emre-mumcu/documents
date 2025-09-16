# 1. VSCode Context Menu

**Adding new context menu entries for "Open with VSCode"**

Note: Create a new .reg file and paste the following content in it and save. After that double click the .reg file to add context menu entries.

```reg
Windows Registry Editor Version 5.00

# File Right Click
[HKEY_CLASSES_ROOT\*\shell\OpenWithVSCode]
@="Open with VSCode"
"Icon"="C:\\Program Files\\Microsoft VS Code\\Code.exe"

# Folder Right Click
[HKEY_CLASSES_ROOT\*\shell\OpenWithVSCode\command]
@="\"C:\\Program Files\\Microsoft VS Code\\Code.exe\" \"%1\""

[HKEY_CLASSES_ROOT\*\shell\OpenWithVSCode]
"ExplorerCommandHandler"="{d969a300-e7ff-11d0-a93b-00a0c90f2719}"
```

**Making existing VSCode context menu entries visible in Windows 11 context menu**

*Find existing VSCOde entries in Registry:*

HKEY_CLASSES_ROOT\Directory\shell\Open with Code
HKEY_CLASSES_ROOT\Directory\shell\VSCode
HKEY_CLASSES_ROOT\*\shell\VSCode

You can find keys named "Open with Code" in the previous locations.

*Making those keys visible in Wİndows 11 context menu:*

Check the locations `HKEY_CLASSES_ROOT\*\shell\VSCode` for files and `HKEY_CLASSES_ROOT\Directory\shell\VSCode` for folders. Inside, you should see entries like Open with Code or Open with VSCode.

Windows 11’s new context menu requires an extra value: `ExplorerCommandHandler`.

Go to `HKEY_CLASSES_ROOT\*\shell\VSCode`. In the right pane, create a new String Value.

Name  : ExplorerCommandHandler
Value : {d969a300-e7ff-11d0-a93b-00a0c90f2719}

Go to `HKEY_CLASSES_ROOT\Directory\shell\VSCode`. In the right pane, create a new String Value.

Name  : ExplorerCommandHandler
Value : {d969a300-e7ff-11d0-a93b-00a0c90f2719}

(Optional) If you want the VSCode icon in the menu, add another String Value:

Name  : Icon
Value : "C:\Program Files\Microsoft VS Code\Code.exe"

Now “Open with Code” will show directly in the new Windows 11 context menu. No need to click “Show more options” anymore.

*Note that path values may vary depending on the VSCode installation.*