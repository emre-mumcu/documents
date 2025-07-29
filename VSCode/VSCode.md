Shortcuts

Shift + Option + ↓ → Duplicate line down
Shift + Option + ↑ → Duplicate line up
Shift + Option + S → Toggle line comment
Shift + Option + A → Toggle multiline comment
Shift + Control + T → Toggle terminal

# To set up a code formatter for C# in Visual Studio Code (VS Code), follow these steps:

Go to the Extensions View and search for C# and install the official C# extension by Microsoft.

Open the Command Palette and yype and select "Preferences: Open Settings (JSON)." 

Add the following setting to specify the default formatter for C#:

```json
"[csharp]": {
    "editor.defaultFormatter": "ms-dotnettools.csharp"
}

// Enable Format on Save (Optional)
"editor.formatOnSave": true
```

Save the file.

# Find & Replace

Remove All Comments:
Regexp: #.*

Remove All Empty Lİnes:
Regexp: ^$\n


Toggle Integrated Terminal: Ctrl+Shift+T
Toogle View Panel : Cmd+J

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

Also update the:

Gitlens > Current Line: Enabled seting and unchek the check mark. (gitlens.toggleLineBlame)

Diff Decorations Gutter

Diff Decorations Gutter Width 1
Diff Decorations Gutter Visibility hover

# CodeLens

Editor: Code Lens Font Size 10


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
* Toggle Single Line Comment    : Ctrl + ö
* Format Document               : Shift+Alt+F
* Formaty Selection             : Ctrl+K Ctrl+F

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
