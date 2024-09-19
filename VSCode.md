# Extension List

Launch Visual Studio Code Quick Open (Ctrl + P)

```zsh
% ext install htmltagwrap
```

# Export/List Extensions

```zsh
# Unix:
% code --list-extensions | xargs -L 1 echo code --install-extension
# Windows (PowerShell, e. g. using Visual Studio Code's integrated Terminal):
% code --list-extensions | % { "code --install-extension $_" }
```

# Disable Git Indicators

Open settings and search for "Diff Decorations Gutter Action" and set to none. Or add this to your settings.json file:

```
"scm.diffDecorationsGutterAction": "none"
```

# Sort Lines

Open keyboard shortcuts (Cmd+K   Cmd+S) and search for  editor.action.sortLines and set key-bindings to the following: 

* editor.action.sortLinesAscending
* editor.action.sortLinesDescending

# Remove Unused Usings

You can use quick fix to remove unused usings. Press (cmd+.) to open quick fix menu and select remove unused usings.

# VSCode Shortcuts

* AddLine Comment			: cmd + K  cmd + C
* Toggle Line Comment		: shift + opt + S
* Toggle Block Comment 		: shift + opt + A
* Quick Fix				    : cmd + .

# Token Color Customizations

Open settings: cmd+, and search:

editor.tokenColorCustomizations

Click "Edit in settings.json"

Edit the following section:

    "editor.tokenColorCustomizations": { 
        "comments": "#707070" 
    }

# Disable AI Code Suggestions

Open settings: cmd+, and search:

Editor > Inline Suggest: Enabled

Uncheck the Controls whether to automatically show inline suggestions in the editor.

Exclude files & folders in explorer tree

* Open VS User Settings (File > Preferences > Settings on Windows, Code > Settings > Settings on Mac). This will open the setting screen.
* Search for files:exclude in the search at the top.
* Click [Add Pattern] button and add the file / folder names to exclude.

To hide obj and bin folders, add the following patterns:

* **/obj
* **/bin

# Formatting

VS Code has great support for source code formatting. The editor has two explicit format actions:

1) Format Document (⇧⌥F) - Format the entire active file.
2) Format Selection (⌘K ⌘F) - Format the selected text.

You can invoke these from the Command Palette (⇧⌘P) or the editor context menu.

# Set VSCode the default text editor for MacOS

Open console and run the following command:

defaults write com.apple.LaunchServices/com.apple.launchservices.secure LSHandlers -array-add '{LSHandlerContentType=public.plain-text;LSHandlerRoleAll=com.microsoft.VSCode;}'
