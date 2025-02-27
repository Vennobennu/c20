---
title: Command-line interfaces
img: cmd.jpg
caption: cmd.exe on Windows 7
keywords:
  - cli
---
Basic knowledge of **command-line interfaces** (CLI) is a required skill in Halo modding. Not only does [Tool](~) use a CLI, many community tools like [Invader](~) do as well. CLI usage consists of typing commands into a shell program which runs other programs and displays their output.

On Windows, [Command Prompt][cmd], [PowerShell][], and the newer [Windows Terminal][wterm] are used as shells. On Linux, terminal emulators vary by distribution but [Bash][] is typically used as the shell.

CLIs are an alternative to _Graphical User Interfaces_ (GUIs). While they have a learning curve, the main advantage of CLIs are that the interaction is reified as plain text which is easily shared, scripted, and repeated. For example, the often-repeated workflow of [BSP compilation](~tool#structure-compilation), [radiosity](~tool#lightmaps), then [map](~h1/map) compilation can be scripted as a single command to save time during map development.

# Basic concepts
Across all shells, commands are entered in the format: `command <arguments>`. This should be familiar to users of Halo's developer console. The type and number of arguments varies by the command/program used, but the command name and each argument are always separated by spaces.

The shell always has a Current Working Directory (CWD). This is a location (folder) on the filesystem that gives context to file paths provided as arguments. All shells provide a way to change the CWD and see a list of files.

# Command Prompt (Windows)
Command prompt, or `cmd.exe`, is an [MS-DOS shell][wiki-dos] which ships with all versions of Windows. It can be opened by typing "cmd" into the Run utility, searching for it in the start menu, or entering it into Explorer's navigation bar to open a prompt at the current working directory. Some users choose to copy `C:\Windows\System32\cmd.exe` into their Halo CE directory and launch it from there.

The current working directory is always shown at the start of the prompt line. You can always open an Explorer window at the working directory by entering `explorer`.

## Navigation
Use the `dir` command to list files in the current working directory, and `cd` to navigate to a different directory on the same drive. Drive letter can be switched by entering just `D:` for example, and the parent directory can be referred to using `..`.

```dos
dir

cd "C:\Program Files (x86)\Microsoft Games\Halo Custom Edition"
cd tags
cd ..\data
```

## Running programs
A program (.exe) can be run by typings its name if it's in the current working directory. The ".exe" extension can be used or not. For example, assuming the current working directory is where the HEK has been installed, either of these commands will work [Tool](~):

```dos
tool build-cache-file levels\test\tutorial\tutorial
tool.exe build-cache-file levels\test\tutorial\tutorial
```

## Scripting
Multiple lines of commands can be added to a `.bat` batch file, which can be run from Explorer by double-clicking or from a command prompt by entering the filename. This allows you to automate tedious series of commands which are run frequently:

```dos
REM Comments in DOS begin with REM for "remark"
tool structure levels\test\tutorial tutorial
tool lightmaps levels\test\tutorial\tutorial tutorial 0.8 0.6
tool build-cache-file levels\test\tutorial\tutorial
```

# PowerShell (Windows)
PowerShell is a newer Windows shell which was intended to improve upon Command Prompt. For the purposes of Halo modding, you would use it very similarly to Command Prompt, though it has a few key differences:

* When running a program in the current directory, prefix it with `.\` and include the extension like `.\tool.exe`.
* You can switch drives using `cd D:\example`.

# Bash (Linux)
The **Bourne-again Shell** (Bash) is the most common default shell used on Unix-like systems. Getting help for a command's arguments differs by command, but usually one of `man <command name>`, `<command name> -h`, or `<command name> --help` will work.

## Navigation
In Bash, navigation is done with the `cd` (change directory) command. At any time, you can see a listing of files in the working directory (CWD) with `ls`. At any time, the shell's prompt will show you what directory you're in.

```sh
# The CWD is ~ (your user home is the default CWD)
ls
# This will output something like:
# Pictures/    bin/        Desktop/   Downloads/
# 'My Games'/  Documents/  wine-prefixes/

# You can also list files at a relative path:
ls Downloads

# When navigating to a directory with spaces in its name, use quotes:
cd "My Games"

# The CWD is now ~/My Games
# Backslashes can also be used to escape spaces. Forward slashes separate directories:
cd Halo\ CE/savegames/

# Use ".." to go back up levels
cd .. # one level
cd ../.. # two levels
# CWD should now be ~ again

# Note that "~" is a shell expansion that does not work when quoted.
# It is easiest to type directory paths by hitting the tab key for auto-complete.
cd ~/wine-prefixes/halo/drive_c/Program\ Files\ \(x86\)/Microsoft\ Games/Halo\ Custom\ Edition/

# Return to the previous directory (~) using the minus symbol
cd -

# Paths do not have to be relative to ~ or the CWD; they can also be absolute:
cd /home/youruser/projects

# Directories can be created with mkdir:
mkdir my-halo-project
```

## Running programs
Some commands like `cd` and `ls` are shell built-ins. Others are actual external programs that are invoked by the shell. Typically these programs are located by the shell using the `PATH` environment variable (try running `echo $PATH`), which is a list of directories which contain runnable programs. As an example, installing [Invader](~) will place its executables in `/usr/bin/` so they can be run in any CWD like `invader-build <arguments>`.

Since we are mainly interested in running Halo and [HEK](~) programs like [Tool](~), which are Windows programs, you will need to set up [Wine][] to run them on Linux. This page will not cover how to install and use Wine, but to run an EXE program you will need to invoke it like so:

```sh
# Assuming tool.exe is in the CWD:
wine tool.exe build-cache-file levels\\test\\tutorial\\tutorial
```

Because tool.exe is a Windows program, it expects paths with Windows directory separators (backslashes). However, in Bash the backslash has special meaning and must be escaped. In other words, use double backslashes where you would normally use a single one.

## Scripting
As an example shell script, create a `compile-map.sh` file with these contents in Halo's installation folder (where you would find `tool.exe`):

```sh
set -e
wine tool.exe structure levels\\test\\tutorial tutorial
wine tool.exe lightmaps levels\\test\\tutorial\\tutorial tutorial 0.8 0.6
wine tool.exe build-cache-file levels\\test\\tutorial\\tutorial
wine haloce.exe
```

Run this script with the command `sh compile-map.sh` and it will compile, light, and build the map for you, then launch the game. Testing can be further sped up by adding an [init.txt file](~arguments#init-txt) to Halo's home with the line `sv_map mapname slayer` to make the game immediately launch into your map.


[cmd]: https://en.wikipedia.org/wiki/Cmd.exe
[powershell]: https://en.wikipedia.org/wiki/PowerShell
[wterm]: https://en.wikipedia.org/wiki/Windows_Terminal
[bash]: https://en.wikipedia.org/wiki/Bash_%28Unix_shell%29
[wine]: https://www.winehq.org/
[wiki-dos]: https://en.wikipedia.org/wiki/MS-DOS#Windows_command-line_interface
