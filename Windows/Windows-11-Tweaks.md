# How to get rid of annoying right click menu design of Windows 11

In Windows 11, the right-click context menu was redesigned with a more streamlined look, but it doesn't provide all the options that the classic menu did. To switch back to the old right-click menu (the one from Windows 10), you need to modify a few settings in the Registry Editor.

# Steps to Restore the Old Right-Click Menu in Windows 11

1. Open the Registry Editor:

* Press Win + R to open the Run dialog.
* Type regedit and press Enter.
* If prompted by User Account Control (UAC), click Yes to allow Registry Editor to make changes.
* Navigate to the following key: `HKEY_CURRENT_USER\Software\Classes\CLSID`

2. Create a New Key:

Right-click on the CLSID folder on the left pane, select New > Key, and name it: `{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}`

3. Now, right-click the newly created key and select New > Key again, and name it: `InprocServer32`

4. Modify the Default Value:

* Click on the InprocServer32 key.
* On the right side, double-click the (Default) entry and open it and click save without entering any data. This is an important step for this hack to work. Make sure that data is empty string NOT null (value not assigned). 

5. Restart Your Computer:

Close the Registry Editor and restart your computer.
Once you restart, the right-click context menu should revert to the old version from Windows 10. To undo the changes and switch back to the new menu, just delete the key you created in the Registry Editor.

**NOTE**

You can also open Command Prompt (admin) and run:

```bash
# Force Classic Context Menu
PS> reg add "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}" /f
PS> reg add "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32" /ve /d "" /f

# Revert back to Windows 11 new context menu
PS> reg delete "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}" /f

# Apply immediately: After running either set of commands, restart Explorer:
PS> taskkill /f /im explorer.exe 
PS> start explorer.exe
```

**NOTE**

Save the following content as `Windows11_Classic_Context_Menu.reg`, then double-click it and restart Explorer (or reboot).

```reg
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}]
@=""

[HKEY_CURRENT_USER\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32]
@=""
```

To revert back to the Windows 11 new menu, save the following content as `Windows11_Disable_Classic_Context_Menu.reg`, then double-click it and restart Explorer (or reboot).

```reg
Windows Registry Editor Version 5.00

[-HKEY_CURRENT_USER\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}]
```

# Visual Studio Code Context Menu

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

# 2. Useful Windows Shortlinks

Win+R (Run)

shell:startup

