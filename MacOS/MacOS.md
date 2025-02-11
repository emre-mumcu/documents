# MacOS Tips & Tricks

## MacOS Recovery

* Command-R: When you press and hold these two keys at startup, Recovery will offer the current version of the most recently installed macOS.
* Option-Command-R: When you press and hold these three keys at startup, Recovery might offer the latest macOS that is compatible with your Mac.
* Shift-Option-Command-R: When you press and hold these four keys at startup, Recovery might offer the macOS that came with your Mac, or the closest version still available.

## Auto Power On/Off

```zsh
# Disable auto power on when the lid is open or power cable is connected:
% sudo nvram AutoBoot=%00

# Enable auto power on when the lid is open or power cable is connected:
% sudo nvram AutoBoot=%03
```

## Bootable USB for MacOS

```zsh
% sudo /Applications/Install\ macOS\ Sonoma.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume
```

## Open Finder From Terminal

```zsh
% open .
```

# Macbook AutoBoot Settings 

```zsh
# Disable auto power on when the lid is open or power cable is connected:
> sudo nvram AutoBoot=%00

# Enable auto power on when the lid is open or power cable is connected:
> sudo nvram AutoBoot=%03
```

# Httpie Backup

To backup the Httpie collections backup and restore the following folder:

```zsh
/Users/{userName}/Library/Application Support/HTTPie
```

# CPU Info

```zsh
% sysctl -a | grep machdep.cpu
```

# MacOS Shortcuts

* Screen capture	    : cmd + shift + 3 / 4 / (4+space) / 5
* Spotlight		        : cmd + spacebar
* Emoji Keyboard	    : kntrl + cmd + space
* Close Application	    : cmd + Q
* Force Quit Menu	    : cmd + opt + esc


# Use Function Keys (F1-F12)

Apple -> System Settings -> Keyboard -> Keyboard Shortcuts -> Function Keys -> Use F1, F2 etc. keys as standart keys:

Activate for F1, F2 keys; Deactivate for functions (sound, brightness etc)

# Disable Spelling in Notes App

Open Notes application and select Edit -> Spelling and Grammer uncheck the following:
* Check Spelling While Typing
* Check Grammer With Spelling

# Disable AutoCorrect

Open System Settings -> Keyboard -> Text Input 
Click the Edit button next to the input souce and disabethe followings:

* Correct spelling automatically
* Capitalize words automatically
* Show inline predictive text
* Use smart quotes and dashes

# MacOS Folder Structure

```zsh
% man hier
```

# Clear Caches

```zsh
/Users/emre/Library/Containers/com.unsplash.Wallpapers
```

How to manually clear the application cache on Mac:
1. Open Finder, select the Go menu, and click Go to Folder.
2. Type /Users/[YourUserName]/Library/Caches in the window and click Go.
3. You’ll see the user caches for all your apps. You can go into each folder, select the files inside, and drag them to the Trash. Make sure to empty the Trash to get rid of the files.

# 50 mac Tips

* networkquality command
* Preview App has a reduct tool to remove senstive data from pdf.
* Recover closed chrome tabs: cmd + shift + T
* When mouse pointer is on a file, if you press spacebar then preview is opened, if you press cmd+click then file will be shown in finder. Cmd+click will show the file in finder everywhere in mac.
* Preview app can copy text from image, even translate the text from different languages.
* In Photo app, you can copy any image part from image.
* System Settings -> Keyboard -> Text Input -> Input Sources -> Edit ->  Correct spelling automatically  =>  Show inline predictive text

# Add Visual Studio Code to Path

```zsh
% vi ~/.zprofile
export PATH=/Applications/Visual\ Studio\ Code.app/Contents/Resources/app/bin:$PATH
% source ~/.zprofile
```

# Get Active Shell

```zsh
% echo $0
```

# Add Folder to PATH

```zsh
% vi ~/.zprofile
export PATH=/path/to/directory:$PATH
% source ~/.zprofile
```

# Copy (cp) Command

In the Terminal app  on your Mac, use the cp command to make a copy of a file. For example, to copy a folder named demo to another volume:

```zsh
% cp -R /path1/demo /path2/demo
```

The -R flag causes cp to copy the folder and its contents. 

Note that the folder name does not end with a slash, which would change how cp copies the folder and copies all the files in demo folder to specified path. Without the slash at the end, folder demo is copied to the specified path.

# Customize Shell Prompt

The standard prompt is configured in etc/zshrc. The prompt can be configured in the ~/.zshrc file in your home directory:

```zsh
% vi ~/.zshrc
PS1="%F{20}%n %#%F "
PS1="%F{77}%n %1d %# %F{255} "
PS1="%F{77}%n %1d %# %F{255} "
# Refresh terminal to changes take affect:
% exec zsh
# %F is how zshrc starts and stops using a foreground colour within the terminal
# %# A ‘#’ if the shell is running with privileges, a ‘%’ if not. 
```

https://zsh.sourceforge.io/Doc/Release/Prompt-Expansion.html#Prompt-Expansion
https://upload.wikimedia.org/wikipedia/commons/1/15/Xterm_256color_chart.svg


# Rename (mv) Command

```zsh
mv /path1/folder1 /path1/folder2
```

# DotSlash

Any way to avoid dot-slash (./) when running executable scripts in bash?

https://www.linfo.org/dot_slash.html

The solution to the question is to add export PATH=.:$PATH to your .bash_profile. This will include the current directory to the unix search path while it searches for the command. It is also wise to have yourself informed about the security risks of doing so.

# shasum

```zsh
% shasum -a 256 /path/to/file
```