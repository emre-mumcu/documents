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

You can also open Command Prompt (admin) and run the “reg add “HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32″ /f /ve” command.