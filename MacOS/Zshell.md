# zsh (MacOS Shell)

The shell is the command-line interpreter, or language, that will process the commands of the script. Historically, the default shell for macOS was bash. But with the introduction of macOS Catalina Apple changed the default from the bash shell to zsh (zshell). Both are part of the Bourne family of shells. Bash, or the Bourne Again Shell, is a hallmark of Linux systems. ZSH, also called the Z shell, is an extended version of the Bourne Shell (sh), with new features and support for plugins and themes. Since it's based on the same shell as Bash, ZSH has many of the same features, and switching over is a breeze. You can check which shell is you are using by the following command:

```zsh
% echo $0
```

If the result is `-zsh` then you are using `Z-Shell`. You can get the version of the Z-Shell by the following command:

```zsh
% zsh --version
```

Zsh configuration files are kept in the user's home directory and are named with a dot as the first character to keep them hidden by default. Zsh shell offers four configuration files:

* ~/.zshenv
* ~/.zprofile
* ~/.zshrc
* ~/.zlogin

To understand the differences among Zsh configuration files, consider various shell uses, which can be classified as interactive or non-interactive, login or non-login sessions.

1. On macOS, each new terminal session is treated as a login shell, so opening any terminal window starts an interactive login session. Also, a system administrator who connects to a remote server via SSH initiates an interactive login session.
2. If a terminal window is already open and you run the command zsh to start a subshell, it will be interactive and non-login. Beginners rarely use subshells.
3. Automated shell scripts run without login or any user prompting. These are non-interactive and non-login.
4. Few people ever encounter a non-interactive login shell session. It requires starting a script with a special flag or piping output of a command into an SSH connection.

These 4 use cases necessitate different shell configurations, which explains why Zsh supports four different configuration files. Here's how the configuration files are used:

1. `~/.zshenv`: This is loaded universally for all types of shell sessions (interactive or non-interactive, login or non-login). It is the only configuration file that gets loaded for non-interactive and non-login scripts like cron jobs. However, macOS overrides this for PATH settings for interactive shells.
2. `~/.zprofile`: Loaded for login shells (both interactive and the rare non-interactive sessions). MacOS uses this to set up the shell for any new terminal window. Subshells that start within the terminal window inherit settings but don't load ~/.zprofile again.
3. `~/.zshrc`: Loaded only for interactive shell sessions. It is loaded whenever you open a new terminal window or launch a subshell from a terminal window.
4. `~/.zlogin`: Only used for login shell configurations, loaded after .zprofile. This is loaded whenever you open a new terminal window.

# How to Use Each File

With that in mind, let's consider which configuration files you should use.

1. ~/.zshenv: It is universally loaded, so you could use it to configure the shell for automated processes like cron jobs. However, it is best to explicitly set up environmental variables for automated processes in scripts and leave nothing to chance. As a beginner, you will not use this configuration file. In fact, few experienced macOS developers use it.
2. ~/.zprofile: Homebrew recommends setting the PATH variable here. There's a reason PATH should be set in ~/.zprofile and not the universal ~/.zshenv file: the macOS runs a utility path_helper (from /etc/zprofile) that sets the PATH order before ~/.zprofile is loaded.
3. ~/.zshrc: This is the configuration file that most developers use. Use it to set aliases and a custom prompt for the terminal window. You can also use it to set the PATH (which many people do) but ~/.zprofile is preferred.
4. ~/.zlogin: This is rarely used. Only important in managing the order of initialization tasks for login shells in complex environments. It can be used to display messages or system data.

MacOS now launches any new terminal window as a login shell, loading both ~/.zprofile and ~/.zshrc files without concern for the shell startup time.
The key advantage of the ~/.zprofile file (versus ~/.zshenv) is that it sets environment variables such as PATH without override from macOS. The ~/.zshrc file could be used for the same but, by convention and design, is intended for customizing the look and feel of the interactive terminal.
Summary

If you're looking for simple guidelines, here's the current best practice.

* Use ~/.zprofile to set the PATH and EDITOR environment variables.
* Use ~/.zshrc for aliases and a custom prompt, tweaking the appearance and behavior of the terminal.
* If you write automated shell scripts, check and set environment variables in the script.
 
# PATH Environmnet Variable

The Mac `$PATH` is a list of directories (folders) where the computer's commands and programs are stored for use by the command line, or Terminal. The shell (by default, Zsh, the Z shell) is the program that runs in the Mac Terminal that interprets Unix commands. Shell settings, or environment variables, are stored in configuration files and set when you login or launch the Terminal application. The $PATH variable enables the system to locate the necessary programs without needing the full path for execution.

The `$PATH` environment variable determines the directories the shell searches for executable files. It's a list of directory paths, separated by colons (:). For example, a default $PATH looks like /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin.

Consider the Homebrew package manager for macOS as an example. On Apple silicon (M1, M2, and M2 CPUs), Homebrew installs files into the /opt/homebrew/ folder, which is not part of the default shell $PATH. After you Install Homebrew, you must set the shell $PATH to use any software programs installed by Homebrew.

## Decoding the default Mac $PATH

The default $PATH includes the contents of `/etc/paths` and all the files in the `/etc/paths.d` directory. On login, maOS uses a utility /usr/libexec/path_helper to set the default $PATH environment variable from the contents of the /etc/paths file and all the files in the /etc/paths.d directory. The path_helper utility is executed at login from /etc/zprofile. It does not duplicate paths, and just appends additional unique values to any existing path.

Directories named bin are for "binaries" or executable command line programs. The sbin directories are for system management programs. The bin and /usr/bin directories are for basic command line programs provided by Apple. The /usr/local/bin is for user-installed executables.

## Where to set the Custom Entries to the $PATH

Set the $PATH environment variable in the `~/.zprofile` (Zsh Profile) file. 

The tilde ~/ is a Unix abbreviation for your home directory. That is, the .zprofile file belongs in your home directory. By default, the ~/.zprofile configuration file does not exist for a user on a new Mac. You'll need to manually create the file in your home directory to properly configure your development environment.

You can create a new file from the command line with the vi command:

```zsh
% vi ~/.zprofile
```

After you create the file, you can add your new PATH variables as follows:

```zsh
export PATH=/path/to/directory:$PATH
```

The new $PATH variable will be exported to the environment as a combination of the previous $PATH plus a new directory. The new directory comes first; it will take precedence over any directories present in the previous $PATH. A : colon character separates the new directory path from the existing directories in the previous $PATH.

To verify your change, run:

```zsh
% echo $PATH
 ```

Reset the shell session

Changes to the `~/.zprofile` file will not take effect in the Terminal until you've quit and restarted the terminal. Alternatively you can use the source command to reset the shell environment:

```zsh
% source ~/.zprofile
```
The source command reads and executes a shell script file, in this case resetting the shell environment with your new $PATH setting.
 
# Get the active Shell

There are three approaches to finding the name of the current shell's executable:

```zsh
echo $0
```

This will print the program name which in the case of the shell is the actual shell.

```zsh
ps -ef | grep $$ | grep -v grep
```

This will look for the current process ID in the list of running processes. Since the current process is the shell, it will be included. This is not 100% reliable.

```zsh
echo $SHELL
```

The path to the current shell is stored as the SHELL variable for any shell.
 
# Edit the $PATH

Use a text editor to edit the `~/.zprofile` file. The export command sets environment variables in the zsh shell. Here's the format for setting the $PATH:

```zsh
export PATH=/path/to/directory:$PATH
```

The new $PATH variable will be exported to the environment as a combination of the previous $PATH plus a new directory. The new directory comes first; it will take precedence over any directories present in the previous $PATH. A : colon character separates the new directory path from the existing directories in the previous $PATH.

ZSH allows you to use special mapping of environment variables:

```zsh
# append
path+=('/home/david/pear/bin')

# or prepend
path=('/home/david/pear/bin' $path)

# export to sub-processes (make it inherited by child processes)
export PATH

#To verify your change, run:
% echo $PATH
```

Here's an example of adding the Homebrew directory used with Apple M1, M2, M3 computers:

```zsh
export PATH=/opt/homebrew/bin:$PATH
```

You might see this form with double quotes: export PATH="/opt/homebrew/bin:$PATH" but you won't need the double quotes unless a directory name contains spaces or special characters.

This example differs from what Homebrew recommends. Homebrew provides its own shellenv utility for setting its path in the ~/.zprofile file.

```zsh
% eval "$(/opt/homebrew/bin/brew shellenv)"
```

# Reset the shell session

Changes to the `~/.zprofile` file will not take effect in the Terminal until you've quit and restarted the terminal. Alternatively (this is easier), you can use the source command to reset the shell environment:

```zsh
% source ~/.zprofile  # Or just restart your terminal
```

The source command reads and executes a shell script file, in this case resetting the shell environment with your new $PATH setting.

```zsh
# To get the active shell, there are three approaches to finding the name of the current shell's executable:

# 1) This will print the program name which in the case of the shell is the actual shell.
echo $0

# 2) This will look for the current process ID in the list of running processes. Since the current process is the shell, it will be included. This is not 100% reliable.
ps -ef | grep $$ | grep -v grep

# 3) The path to the current shell is stored as the SHELL variable for any shell.
echo $SHELL
```

# References
https://www.freecodecamp.org/news/how-do-zsh-configuration-files-work/
https://mac.install.guide/terminal/path