# Examples of various commands

## 0. My usuals:
```
#!/usr/bin/env bash

#========================================================== 
# title           : example_title 
# description     : example description
# author          : Pearson 
# date            : today 
# version         : 1.0     
# usage           : ./example_title 
# notes           : constantly changing 
# TODO            : 1) Add more stuff 
#                   2)  
#========================================================== 
```
```bash
## Logging function
logit () {
  printf $1
  printf $1 >> $logfile.txt
}
```
``` bash
## Check for root v1
if [[ "$USER" != "root" ]]; then
  echo "***ERROR*** You are not root"
  exit 1
fi
```
```bash
## Check for root v2
if [[ $UID -ne 0 ]]; then
  echo "***ERROR*** You are not root"
  exit 1
fi
```

## 1. File Commands
+ `ls`                            - Lists your files
+ `ls -lrt`                       - Lists your files in 'long format', reverse order, and last modified
+ `ls -a`                         - Lists all files, including hidden files
+ `ln -s <filename> <link>`       - Creates symbolic link to file
+ `touch <filename>`              - Creates or updates your file
+ `cat > <filename>`              - Places standard input into file
+ `more <filename>`               - Shows the first part of a file (move with space and type q to quit)
+ `head <filename>`               - Outputs the first 10 lines of file
+ `tail <filename>`               - Outputs the last 10 lines of file (useful with -f option)
+ `vim <filename>`                - Lets you create and edit a file
+ `mv <filename1> <filename2>`    - Moves a file
+ `mv filename.{old,new}`         - Quickly rename a file
+ `cp <filename1> <filename2>`    - Copies a file
+ `cp file{,.bak}`                - Create quick back-up/copy of a file
+ `rm <filename>`                 - Removes a file
+ `diff <filename1> <filename2>`  - Compares files, and shows where they differ
+ `wc <filename>`                 - Tells you how many lines, words and characters there are in a file
+ `chmod -options <filename>`     - Lets you change the read, write, and execute permissions on your files
+ `chown user:group <filename>`   - Change owner and group for file
+ `gzip <filename>`               - Compresses files
+ `gunzip <filename>`             - Uncompresses files compressed by gzip
+ `gzcat <filename>`              - Lets you look at gzipped file without actually having to gunzip it
+ `tar -zxvf <file.tar.gz>`       - Unzip, extract, verbose, use tar file in current dir
+ `somecmd | tee -a output.txt`   - Append output of "somecmd" to multiple files 

## 2. Directory Commands
+ `mkdir -p <dirname>`  - Makes a new directory \/ `-p` makes missing dirs
+ `cd`                  - Changes to home
+ `cd <dirname>`        - Changes directory
+ `cd ..`               - Changes to directory above current
+ `cd ~`                - Changes to users HOME directory
+ `pwd`                 - Tells you where you currently are
+ `pushd`               - Put the current directory on the stack and change directory to the one specified as a parameter
+ `popd`                - Go back to the directory on the stack

## 3. SSH, System Info & Network Commands
+ `ssh user@host`               - Connects to host as user
+ `ssh -p <port> user@host`     - Connects to host on specified port as user
+ `ssh-copy-id user@host`       - Adds your ssh key to host for user to enable a keyed or passwordless login
+ `mount | column -t`           - Currently mounted filesystems in nice layout
+ `mount -o remount,rw <mnt>`   - Remounts a mount point with read-write capability
+ `ssh user@host cat /path/to/remotefile | diff /path/to/localfile -`      - Compare a remote file with a local file
+ `whoami`                      - Returns your username
+ `passwd`                      - Lets you change your password
+ `quota -v`                    - Shows what your disk quota is
+ `date`                        - Shows the current date and time
+ `cal`                         - Shows the month's calendar
+ `uptime`                      - Shows current uptime
+ `w`                           - Displays whois online
+ `finger <user>`               - Displays information about user
+ `uname -a`                    - Shows kernel information
+ `cat /etc/*-release`          - Shows distribution release info (redhat derivatives)
+ `df`                          - Shows disk usage
+ `du <filename>`               - Shows the disk usage of the files and directories in filename (du -s give only a total)
+ `last <yourUsername>`         - Lists your last logins
+ `ps -u yourusername`          - Lists your processes
+ `kill <PID>`                  - Kills (ends) the processes with the ID you gave
+ `killall <processname>`       - Kill all processes with the name
+ `top`                         - Displays your currently active processes
+ `bg`                          - Lists stopped or background jobs ; resume a stopped job in the background
+ `fg`                          - Brings the most recent job in the foreground
+ `fg <job>`                    - Brings job to the foreground
+ `ping <host>`                 - Pings host and outputs results
+ `whois <domain>`              - Gets whois information for domain
+ `dig <domain>`                - Gets DNS information for domain
+ `dig -x <host>`               - Reverses lookup host
+ `wget <file>`                 - Downloads file
+ `pv sourcefile > destfile`    - Copy a file and watch it's progress
+ `rsync -P file1 file2`        - Copy a file and watch it's progress
+ `ps aux | sort -nk +4 | tail` - Display top ten processes - sorted by memory usage
+ `ps awwfux | less -S`         - 4-way scrollable process tree w/ deatails
+ `lshw -html > hardware.html`  - Computer hardware overview v1
+ `lsof -i`                     - List programs w/ open ports and connections

## 4. File/Pattern Searching
+ `grep -RnisI <pattern> *`                                                - Search for a <pattern> inside all files in current dir
+ `grep <pattern> <filenames>`                                             - Looks for the string in the files
+ `grep -r <pattern> <dir>`                                                - Search recursively for pattern in directory
+ `grep -v <pattern> <dir>`                                                - Inverted search for pattern in directory
+ `grep -i <pattern> <dir>`                                                - Case insensitive search for pattern in directory
+ `find . -type f | awk -F'.' '{print $NF}' | sort| uniq -c | sort -g`     -  List all file extensions in a dir
+ `find ./ -type f -exec chmod 644 {} +`                                   - Recrsively change perms on files, leave dirs alone
+ `find / -mmin -60 -type f`                                               - Find filed modified w/in the last 60 mins
+ `find / -type f -size +500M`                                             - Find all files larger than 500M
+ `find <dir> -name "name*"`                                               - Find file with "name" in dir
+ `locate <file>`                                                          - Find file (quick system index search)

## 5. Package Commands
+ `rpm -e <rpm_name>`           - Remove a specific rpm
+ `rpm -i <rpm_name>`           - Install a specific rpm
+ `rpm -U <rpm_name>`           - Upgrades a specific rpm
+ `rpm -qa <pattern>`           - Query rpm status
+ `yum update -qy`              - Update all packages -- quiet and assume yes
+ `yum update --skip-broken -y` - Update all packages skipping packages that are missing dependencies
+ `yum history redo last`       - Redo last remove/update/install transaction
+ `yum install <package>`       - Install a specific package
+ `yum reinstall <package>`     - Reinstall a specific package
+ `yum remove <package>`        - Remove/Uninstall a specific package
+ `yum check-update`            - Checks for updates
+ `yum list <package>`          - Show various information about available packages
+ `yum provides <package>`      - Show which package provides some feature or file
+ `yum info <package>`          - Show description and summary information about available packages
+ `yum clean all`               - Cleans various things which accumulate in the yum cache directory
+ `createrepo <path/to/rpms>`   - Create a local repository
+ `createrepo --update <path>`  - Update a local repository

## 7. Others that don't have a category (currently)
+ `#!usr/bin/env bash` \|\| `#!/bin/bash`                - Should be at the top of all bash scripts
+ `alias test=$(echo "test")`                            - Sets an alias called "test" \/ temporary and only for that shell
+ `echo "alias test=$(echo "test")" >> ~/.bash_profile`  - Sets an alias called "test" \/ permanent alias in bash shell
+ `unalias test`                                         - Removes alias "test" \/ if added with alias test='ls'
+ `man <somecmd>`                                        - Help manuals for somecmd
+ `apropos <somecmd>`                                    - Search help manuals 
+ `basename`                                             - Strip dir and suffix from filenames
+ `dmesg`                                                - Print kernel & driver messages
+ `touch /forcefsck`                                     - Run a fsck (file system check) on next boot
+ `ls -R | grep ":$" | sed -e 's/:$//' -e 's/[^-][^\/]*\//--/g' -e 's/^/   /' -e 's/-/|/'`    - Graphical tree of sub-dirs
+ `netstat -an | grep ESTABLISHED | awk '{print $5}' | awk -F: '{print $1}' | sort | uniq -c | awk '{ printf("%s\t%s\t",$2,$1); 
for (i = 0; i < $1; i++) {printf("*")}; print "" }'`  - Graph # of connections for each host
+ `history | awk '{a[$2]++}END{for(i in a){print a[i] " " i}}' | sort -rn | head`  - List most used commands

## 3. For loops
```bash
## Computer hardware overview v2
for host in host1 host2 host3; do 
  ssh $host "lshw -html" > $host_hardware.html 
done
```
```bash
## Setup ssh keys for all systems
ssh-keygen &&
for host in $(cat /path/to/hosts.txt); do
  ssh-copy-id "$host"
done
```
```bash
## Remote command execution
for host in $(cat /path/to/hosts.txt); do
  ssh user@"$host" 'sudo <some command> > /path/to/logs/<some command>.$(date +"%Y%m%d_$H%M%S")'
done
```
