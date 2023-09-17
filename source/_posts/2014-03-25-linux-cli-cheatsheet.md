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