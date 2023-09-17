---
layout: post
title: "Linux CLI Cheatsheet"
date: 2014-03-25 12:00
comments: true
categories: linux
---

lorem ipsum...

###  Bash Commands

| `uname -a` | Show system and kernel |
| --- | --- |
| `head -n1 /etc/issue` | Show distri­bution |
| `mount` | Show mounted filesy­stems |
| `date` | Show system date |
| `uptime` | Show uptime |
| `whoami` | Show your username |
| `man command` | Show manual for command |
| `watch -n 1 'date'` | Watch changes to the `date` each second then display output |

<br>

###  Bash Variables

| `env` | Show enviro­nment variables |
| --- | --- |
| `export NAME=value` | Set `$NAME` to value |
| `$NAME` | Output value of `$NAME` variable |
| `$PATH` | Executable search path |
| `$HOME` | Home directory |
| `$SHELL` | Current shell |

<br>

### Commmand Execution & IO

`cmd`, `cmd1` and `cmd2` refers to a command.

| `cmd1 ; cmd2` | Run `cmd1` then `cmd2` |
| --- | --- |
| `cmd1 && cmd2` | Run `cmd2` if `cmd1` is successful |
| `cmd1 \|\| cmd2` | Run `cmd2` if `cmd1` is not successful |
| `cmd &` | Run `cmd` in a subshell |
| `cmd1 \| cmd2` | Pipe stdout of `cmd1` to `cmd2` |
| `cmd1 \| cmd2` | Pipe stderr of `cmd1` to `cmd2` |
| `cmd < file` | Input of `cmd` from file |
| `cmd1 <(cmd2)` | Output of `cmd2` as file input to `cmd1` |
| `cmd > file` | Standard output `stdout` of `cmd` to file |
| `cmd > /dev/null` | Discard `stdout` of `cmd` |
| `cmd >> file` | Append `stdout` to file |
| `cmd 2> file` | Error output `stderr` of `cmd` to file |
| `cmd &> file` | Every output of `cmd` to file |
| `cmd 1>&2` | stdout to same place as stderr |
| `cmd 2>&1` | stderr to same place as stdout |

<br>

### Directories

pwd
Show current directory
mkdir dir
Make directory dir
cd dir
Change directory to dir
cd ..
Go up a directory
ls
List files

<br>

### Files

touch file1
Create file1
cat file1 file2
Concat­enate files and output
less file1
View and paginate file1
file file1
Get type of file1
cp file1 file2
Copy file1 to file2
mv file1 file2
Move file1 to file2
rm file1
Delete file1
head file1
Show first 10 lines of file1
tail file1
Show last 10 lines of file1
tail -F file1
Output last lines of file1 as it changes

<br>

### File Permis­sions

| `chmod 775 file` | Change mode of file to 775 |
| --- | --- |
| `chmod -R 600 folder` | Recurs­ively chmod folder to 600 |
| `chown user:group file` | Change file owner to user and group to group |

<br>

### File Permission Bits

First digit is owner permis­sion, second is group and third is everyone.
Calculate permission digits by adding numbers below.

| `4` | read \(r) |
| --- | --- |
| `2` | write (w) |
| `1` | execute (x) |

<br>

### ls Options

-a
Show all (including hidden)
-R
Recursive list
-r
Reverse order
-t
Sort by last modified
-S
Sort by file size
-l
Long listing format
-1
One file per line
-m
Comma-­sep­arated output
-Q
Quoted output

<br>

### Search Files

grep pattern files
Search for pattern in files
grep -i
Case insens­itive search
grep -r
Recursive search
grep -v
Inverted search
grep -o
Show matched part of file only
find /dir/ -name name*
Find files starting with name in dir
find /dir/ -user name
Find files owned by name in dir
find /dir/ -mmin num
Find files modifed less than num minutes ago in dir
whereis command
Find binary / source / manual for command
locate file
Find file (quick search of system index)

<br>

### Processes

ps
Show snapshot of processes
top
Show real time processes
kill pid
Kill process with id pid
pkill name
Kill process with name name
killall name
Kill all processes with names beginning name
