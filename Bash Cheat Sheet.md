# Bash Cheat Sheet

## 0. Shell Shortcuts
+ `CTRL+a`  -  go to start of the line
+ `CTRL+c`  -  halts the current command
+ `CTRL+d`  -  logs out of current session, similar to exit
+ `CTRL+e`  -  go to the end of the line
+ `CTRL+l`  -  clears screen and redisplay the line
+ `CTRL+z`  -  stops the current command, resume with fg in the foreground or bg in the background
+ `!!`      -  repeats the last command
+ `exit`    -  logs out of current session

## 1. Bash Basics
+ `env`                 - displays environment variables
+ `export`              - displays all environment variables
+ `echo $PATH`          - displays the executable search path
+ `echo $USER`          - displays the users username
+ `echo $UID`           - displays the users ID
+ `echo $HOME`          - displays the users home directory
+ `echo $SHELL`         - displays the shell you're using
+ `echo $BASH_VERSION`  - displays bash version
+ `echo $HOSTNAME`      - name of the current host
+ `echo $OSTYPE`        - string describing the operating system bash is running on
+ `$$`                  - prints process ID of the current shell
+ `$!`                  - prints process ID of the most recently invoked background job
+ `$?`                  - displays the exit status of the last command
+ `$#`                  - number of command line aruments given to the script
+ `$IFS`                - the (input) field separator
+ `$0`                  - name of the shell or shell script
+ `bash`                - if you want to use bash (type exit to go back to your normal shell)
+ `whereis bash`        - locates source/binary and manuals sections for specified files
+ `which <program>`     - returns the pathnames of the files which would be executed in the current environment
+ `command -v <cmd> \|\| type <cmd>`    - better find path of program
+ `clear`               - clears content on window (hide displayed lines)

### 1.1. File Commands
+ `ls`                            - lists your files
+ `ls -lrt`                       - lists your files in 'long format', reverse order, and last modified
+ `ls -a`                         - lists all files, including hidden files
+ `ln -s <filename> <link>`       - creates symbolic link to file
+ `touch <filename>`              - creates or updates your file
+ `cat > <filename>`              - places standard input into file
+ `more <filename>`               - shows the first part of a file (move with space and type q to quit)
+ `head <filename>`               - outputs the first 10 lines of file
+ `tail <filename>`               - outputs the last 10 lines of file (useful with -f option)
+ `vim <filename>`                - lets you create and edit a file
+ `mv <filename1> <filename2>`    - moves a file
+ `cp <filename1> <filename2>`    - copies a file
+ `rm <filename>`                 - removes a file
+ `diff <filename1> <filename2>`  - compares files, and shows where they differ
+ `wc <filename>`                 - tells you how many lines, words and characters there are in a file
+ `chmod -options <filename>`     - lets you change the read, write, and execute permissions on your files
+ `gzip <filename>`               - compresses files
+ `gunzip <filename>`             - uncompresses files compressed by gzip
+ `gzcat <filename>`              - lets you look at gzipped file without actually having to gunzip it
+ `grep <pattern> <filenames>`    - looks for the string in the files
+ `grep -r <pattern> <dir>`       - search recursively for pattern in directory
+ `grep -v <pattern> <dir>`       - inverted search for pattern in directory
+ `grep -i <pattern> <dir>`       - case insensitive search for pattern in directory
+ `find <dir> -name "name*"`      - find file with "name" in dir
+ `locate <file>`                 - find file (quick system index search)
+ `tar -zxvf <file.tar.gz>`       - unzip, extract, verbose, use tar file in current dir
+ `somecmd | tee -a output.txt`   - append output of "somecmd" to multiple files 

### 1.2. Directory Commands
+ `mkdir -p <dirname>`  - makes a new directory \/ `-p` makes missing dirs
+ `cd`                  - changes to home
+ `cd <dirname>`        - changes directory
+ `pwd`                 - tells you where you currently are
+ `pushd`               - put the current directory on the stack and change directory to the one specified as a parameter
+ `popd`                - go back to the directory on the stack

### 1.3. SSH, System Info & Network Commands
+ `ssh user@host`            - connects to host as user
+ `ssh -p <port> user@host`  - connects to host on specified port as user
+ `ssh-copy-id user@host`    - adds your ssh key to host for user to enable a keyed or passwordless login
+ `whoami`                   - returns your username
+ `passwd`                   - lets you change your password
+ `quota -v`                 - shows what your disk quota is
+ `date`                     - shows the current date and time
+ `cal`                      - shows the month's calendar
+ `uptime`                   - shows current uptime
+ `w`                        - displays whois online
+ `finger <user>`            - displays information about user
+ `uname -a`                 - shows kernel information
+ `cat /etc/*-release`       - shows distribution release info (redhat derivatives)
+ `man <command>`            - shows the manual for specified command
+ `df`                       - shows disk usage
+ `du <filename>`            - shows the disk usage of the files and directories in filename (du -s give only a total)
+ `last <yourUsername>`      - lists your last logins
+ `ps -u yourusername`       - lists your processes
+ `kill <PID>`               - kills (ends) the processes with the ID you gave
+ `killall <processname>`    - kill all processes with the name
+ `top`                      - displays your currently active processes
+ `bg`                       - lists stopped or background jobs ; resume a stopped job in the background
+ `fg`                       - brings the most recent job in the foreground
+ `fg <job>`                 - brings job to the foreground
+ `ping <host>`              - pings host and outputs results
+ `whois <domain>`           - gets whois information for domain
+ `dig <domain>`             - gets DNS information for domain
+ `dig -x <host>`            - reverses lookup host
+ `wget <file>`              - downloads file

## 2. Basic Shell Programming
### 2.1. Variables
+ `varname="value"`                - defines a variable
+ `varname=$(command)`             - defines a variable to be in the environment of a particular subprocess
+ `echo "$varname"`                - checks a variable's value
+ `export VARNAME=value`           - defines an environment variable (will be available in subprocesses)
+ `array[0] = val`                 - several ways to define an array
+ `array[1] = val`
+ `array[2] = val`
+ `array=([2]=val [0]=val [1]=val)`
+ `array(val val val)`
+ `${array[i]}`                  - displays array's value for this index. If no index is supplied, array element 0 is assumed
+ `${#array[i]}`                 - to find out the length of any element in the array
+ `${#array[@]}`                 - to find out how many values there are in the array
+ `declare -a`                   - the variables are treaded as arrays
+ `declare -f`                   - uses funtion names only
+ `declare -F`                   - displays function names without definitions
+ `declare -i`                   - the variables are treaded as integers
+ `declare -r`                   - makes the variables read-only
+ `declare -x`                   - marks the variables for export via the environment
+ `${varname:-word}`             - if varname exists and isn't null, return its value; otherwise return word
+ `${varname:=word}`             - if varname exists and isn't null, return its value; otherwise set it word and then return its value
+ `${varname:?message}`          - if varname exists and isn't null, return its value; otherwise print varname, followed by message and abort the current command or script
+ `${varname:+word}`             - if varname exists and isn't null, return word; otherwise return null
+ `${varname:offset:length}`     - performs substring expansion. It returns the substring of $varname starting at offset and up to length characters
+ `${variable#pattern}`          - if the pattern matches the beginning of the variable's value, delete the shortest part that matches and return the rest
+ `${variable##pattern}`         - if the pattern matches the beginning of the variable's value, delete the longest part that matches and return the rest
+ `${variable%pattern}`          - if the pattern matches the end of the variable's value, delete the shortest part that matches and return the rest
+ `${variable%%pattern}`         - if the pattern matches the end of the variable's value, delete the longest part that matches and return the rest
+ `${variable/pattern/string}`   - the longest match to pattern in variable is replaced by string. Only the first match is replaced
+ `${variable//pattern/string}`  - the longest match to pattern in variable is replaced by string. All matches are replaced
+ `${#varname}`                  - returns the length of the value of the variable as a character string
+ `*(patternlist)`               - matches zero or more occurences of the given patterns
+ `+(patternlist)`               - matches one or more occurences of the given patterns
+ `?(patternlist)`               - matches zero or one occurence of the given patterns
+ `@(patternlist)`               - matches exactly one of the given patterns
+ `!(patternlist)`               - matches anything except one of the given patterns
+ `[abz]`                        - matches any characters inside []. In example, matches "abz"
+ `[!abz]`                       - matches any characters NOT inside [].  In example, matches everyhing except "abz"
+ `[abc-g]`                      - matches any characters inse [] with range. In example, matches "abcdefg"
+ `[[:alnum:]]`                  - matches alphanumeric characters class
+ `[[:alpha:]]`                  - matches alphabetic characters class
+ `[[:blank:]]`                  - matches a blank character; that is, a space or a tab
+ `[[:cntrl:]]`                  - matches control character class
+ `[[:digit:]]`                  - matches digits,  0-9. Other languages can include other characters
+ `[[:graph:]]`                  - matches any printable character except space
+ `[[:lower:]]`                  - matches lower-case characters
+ `[[:print:]]`                  - matches any printable character including space
+ `[[:punct:]]`                  - matches any printable character which is not a space or alphanumeric character
+ `[[:space:]]`                  - matches white-space characters.
+ `[[:upper:]]`                  - matches upper-case characters.
+ `[[:xdigit:]]`                 - matches hexadecimal digts, 0-F
+ `[[:word:]]`                   - matches letters, digits, and the character _.
+ `$(UNIX command)`              - command substitution: runs the command and returns standard output

### 2.2. Functions
> The function refers to passed arguments by position (as if they were positional parameters).
> `$@` is equal to `"$1"` `"$2"`... `"$N"`, where N is the number of positional parameters. 
> `$#` holds the number of positional parameters.

```
functname() {
  shell commands
}
```
+ `unset -f functname`  - deletes a function definition
+ `declare -f`          - displays all defined functions in your login session

### 2.3. Flow Control
| Operators                              |                                           |
|:--------------------------------------:|:-----------------------------------------:|
| [[ statement1 ]]  **&&** [[ statement2 ]]  | and operator                              |
| [[ statement1 ]] **\|\|** [[ statement2 ]]   | or operator                               |
| [[ statement1 **-a** statement2 ]]  |and operator inside a test conditional expression |
| [[ statement1 **-o** statement2 ]]  | or operator inside a test conditional expression |

| String Comparison  |                                              |
|:------------------:|:--------------------------------------------:|
| [[ str1 **==** str2 ]] | str1 matches str2                            |
| [[ str1 **!=** str2 ]] | str1 does not match str2                     |
| [[ str1 **=~** str2 ]] | str1 is like str2                            |
| [[ **-n** str1 ]]      | str1 is not null (has length greater than 0) |
| [[ **-z** str1 ]]      | str1 is null (has length 0)                  |

| Bash Operators               |                                                                                                    |
|:----------------------------:|:--------------------------------------------------------------------------------------------------:|
| [[ **-a** file ]]            | file exists                                                                                        |
| [[ **-d** file ]]            | file exists and is a directory                                                                     |
| [[ **-e** file ]]            | file exists; same -a                                                                               |
| [[ **-f** file ]]            | file exists and is a regular file (i.e., not a directory or other special type of file)            |
| [[ **-r** file ]]            | you have read permission                                                                           |
| [[ **-s** file ]]            | file exists and is not empty                                                                       |
| [[ **-w** file ]]            | your have write permission                                                                         |
| [[ **-x** file ]]            | you have execute permission on file, or directory search permission if it is a directory           |
| [[ **-N** file ]]            | file was modified since it was last read                                                           |
| [[ **-O** file ]]            | you own file                                                                                       |
| [[ **-G** file ]]            | file's group ID matches yours (or one of yours, if you are in multiple groups)                     |
| [[ file1 **-nt** file2 ]]    | file1 is newer than file2                                                                          |
| [[ file1 **-ot** file2 ]]    | file1 is older than file2                                                                          |

|Integers                 | Meaning               |
|:-----------------------:|:---------------------:|
| [[ int1 **-lt** int2 ]] | less than             |
| [[ int1 **-le** int2 ]] | less than or equal    |
| [[ int1 **-eq** int2 ]] | equal                 |
| [[ int1 **-ge** int2 ]] | greater than or equal |
| [[ int1 **-gt** int2 ]] | greater than          |
| [[ int1 **-ne** int2 ]] | not equal             |

```
if [[ "$condition" ]]; then
  statements
[elif [[ "$condition" ]]; then 
  statements...]
[else
  statements]
fi
```
```
for c in {0..5}; do
begin
  echo $c
end
# Outputs 1 2 3 4 5 on separate lines
```
```
for name in svr1 svr2 svr3 svr4 svr5; do
  statements that can use "$name"
done
# Runs cmd on each item in list
```
```
for (( c=1;c<=5;c++ )); do
  echo "$c"
done
# Outputs 1 2 3 4 5 on separate lines
```
```
case expression in
  pattern1 )
    statements 
           ;;
  pattern2 )
    statements 
           ;;
esac
```
```
select name [in list]; do
  statements that can use "$name"
done
```
```
while condition; do
  statements
done
```
```
until condition; do
  statements
done
```

## 3. Command-Line Processing Cycle
> The default order for command lookup is functions, followed by built-ins, with scripts and executables last.
> There are three built-ins that you can use to override this order: `command`, `builtin` and `enable`.

+ `command`  - removes alias and function lookup. Only built-ins and commands found in the search path are executed
+ `builtin`  - looks up only built-in commands, ignoring functions and commands found in PATH
+ `enable`   - enables and disables shell built-ins
+ `eval`     - takes arguments and run them through the command-line processing steps all over again

## 4. Input/Output Redirectors
|Redirects            |                                                                         |
|:-------------------:|:-----------------------------------------------------------------------:|
| cmd1 **\|** cmd2    | pipe; takes standard output of cmd1 as standard input to cmd2           |
| **>** file          | directs standard output to file                                         |
| **<** file          | takes standard input from file                                          |
| **>>** file         | directs standard output to file; append to file if it already exists    |
| **>\|** file        | forces standard output to file even if noclobber is set                 |
| n **>\|** file      | forces output to file from file descriptor n even if noclobber is set   |
| **<>** file         | uses file as both standard input and standard output                    |
| n **<>** file       | uses file as both input and output for file descriptor n                |
| **<<** label        | here-document                                                           |
| n **>** file        | directs file descriptor n to file                                       |
| n **<** file        | takes file descriptor n from file                                       |
| n **>>** file       | directs file description n to file; append to file if it already exists |
| n **>&**            | duplicates standard output to file descriptor n                         |
| n **<&**            | duplicates standard input from file descriptor n                        |
| n **>&** m          | file descriptor n is made to be a copy of the output file descriptor    |
| n **<&** m          | file descriptor n is made to be a copy of the input file descriptor     |
| **&>** file         | directs standard output and standard error to file                      |
| **<&-**             | closes the standard input                                               |
| **>&-**             | closes the standard output                                              |
| n **>&-**           | closes the ouput from file descriptor n                                 |
| n **<&-**           | closes the input from file descripor n                                  |


## 5. Process Handling
+ `my_cmd &`         - runs job in the background and prompts back the shell
+ `jobs`             - lists all jobs (use with -l to see associated PID)
+ `fg`               - brings a background job into the foreground
+ `fg %+`            - brings most recently invoked background job
+ `fg %-`            - brings second most recently invoked background job
+ `fg %N`            - brings job number N
+ `fg %string`       - brings job whose command begins with string
+ `fg %?string`      - brings job whose command contains string
+ `kill -l`          - returns a list of all signals on the system, by name and number
+ `kill PID`         - terminates process with specified PID
+ `ps`               - prints a line of information about the current running login shell and any processes running under it
+ `ps -a`            - selects all processes with a tty except session leaders
+ `disown <PID|JID>` - removes the process from the list of jobs
+ `wait`             - waits until all background jobs have finished


## 6. Tips and Tricks
+ `alias test=$(echo "test")`                            - sets an alias called "test" \/ temporary and only for that shell
+ `echo "alias test=$(echo "test")" >> ~/.bash_profile`  - sets an alias called "test" \/ permanent alias in bash shell
+ `unalias test`                                         - removes alias "test" \/ if added with alias test='ls'
+ `chmod (perm) <file>`                                  - permissions are: 4-read(r) \/ 2-write(w) \/ 1-execute(x)
+ `#!usr/bin/env bash` \|\| `#!/bin/bash`                - should be at the top of all bash scripts
+ `man <somecmd>`                                        - help manuals for somecmd
+ `apropos <somecmd>`                                    - search help manuals 
+ `chown user:group file`                                - change owner and group for file
+ `basename`                                             - strip dir and suffix from filenames
+ `dmesg`                                                - print kernel & driver messages


## 7. Debugging Shell Programs
+ `bash -n scriptname`  - don't run commands; check for syntax errors only
+ `set -o noexec`       - alternative (set option in script)
+ `bash -v scriptname`  - echo commands before running them
+ `set -o verbose`      - alternative (set option in script)
+ `bash -x scriptname`  - echo commands after command-line processing
+ `set -o xtrace`       - alternative (set option in script)
+ `bash -C scriptname`  - prevent overwriting of files by redirection 
+ `set -o noclobber`    - alternative (set option in script)
+ `shopt -s nullglob`   - filename patterns which match no files to expand to a null string, rather than themselves (set in script)
+ `bash -e scriptname`  - abort script at first error, when a command exits with non-zero status (except in loops, if-tests, lists)
+ `set -o errexit`      - alternative (set option in script)
