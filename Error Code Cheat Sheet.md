# Error Codes & System Signals

## 1. Error Codes
### 1.1 General (Bash)
| Exit Code Number | Meaning                   | Comments                                                                         |
|:------------|:------------------------------:|:---------------------------------------------------------------------------------|
| 0           | Success                        |                                                                                  |
| 1           | Catchall for general errors    | Miscellaneous errors, such as "divide by zero" and other impermissible operations|
| 2           | Misuse of shell builtins       | Missing keyword or command, or permission problem                                |
| 126         | Command invoked cannot execute | Permission problem or command is not an executable                               |
| 127         |"command not found"             | Possible problem with $PATH or a typo                                            |
| 128         | Invalid argument to exit       | exit takes only integer args in the range 0 - 255                                |
| 130         | Script terminated by Control-C | Control-C is fatal error signal 2, (130 = 128 + 2, see above)                    |
| 255\*       | Exit status out of range       | exit takes only integer args in the range 0 - 255                                |

#### 1.1.1 Additional (from /usr/include/sysexits.h)
| Exit Code Number | Defined | Comments                              |
|:---------|:--------------:|:---------------------------------------|
| 2 or 64  | EX_USAGE       | command line usage error               |
| 3 or 65  | EX_DATAERR     | data format error                      |
| 4 or 66  | EX_NOINPUT     | cannot open input                      |
| 5 or 67  | EX_NOUSER      | addressee unknown                      |
| 6 or 68  | EX_NOHOST      | host name unknown                      |
| 7 or 69  | EX_UNAVAILABLE | service unavailable                    |
| 8 or 70  | EX_SOFTWARE    | internal software error                |
| 9 or 71  | EX_OSERR       | system error                           |
| 10 or 72 | EX_OSFILE      | critical OS file missing               |
| 11 or 73 | EX_CANTCREAT   | can't create (user) output file        |
| 12 or 74 | EX_IOERR       | input/output error                     |
| 13 or 75 | EX_TEMPFAIL    | temp failure; user is invited to retry |
| 14 or 76 | EX_PROTOCOL    | remote error in protocol               |
| 15 or 77 | EX_NOPERM      | permission denied                      |
| 16 or 78 | EX_CONFIG      | configuration error                    |

### 1.2 Diff
| Exit Code Number | Meaning                                    | 
|:------------|:-----------------------------------------------:|
| 0           | Files are identical                             |
| 1           | Files are different                             |
| 2           | Binaries are different & Genuine failure        |

### 1.3 Grep
| Exit Code Number | Meaning                                    | 
|:------------|:-----------------------------------------------:|
| 0           | Success (line selected)                         |
| 1           | No lines selected                               |
| 2           | Genuine error                                   |

### 1.4 Less
| Exit Code Number | Meaning                                    | 
|:------------|:-----------------------------------------------:|
| 0           | Success or failed to receive argument           |
| 1           | Genuine failure                                 |

### 1.5 Rsync
| Exit Code Number | Meaning                                    | 
|:------------|:-----------------------------------------------:|
| 0           | Success                                         |
| 1           | Syntax or usage error                           |
| 2           | Protocol incompatibility                        |
| 3           | Errors selecting input/output files, dirs       |
| 4           | Requested action not supported                  |
| 5           | Error starting client-server protocol           |
| 6           | Daemon unable to append to log-file             |
| 10          | Error in socket I/O                             |
| 11          | Error in file I/O                               |
| 12          | Error in rsync protocol data stream             |
| 13          | Errors with program diagnostics                 |
| 14          | Error in IPC code                               |
| 20          | Received SIGUSR1 or SIGINT                      |
| 21          | Some error returned by waitpid()                |
| 22          | Error allocating core memory buffers            |
| 23          | Partial transfer due to error                   |
| 24          | Partial transfer due to vanished source files   |
| 25          | The --max-delete limit stopped deletions        |
| 30          | Timeout in data send/receive                    |
| 35          | Timeout waiting for daemon connection           |

### 1.6 Sed
| Exit Code Number | Meaning                                                                    | 
|:------------|:-------------------------------------------------------------------------------:|
| 0           | Success                                                                         |
| 1           | Invalid command, invalid syntax, invalid regular expression                     |
| 2           | One or more of the input file specified on the command line could not be opened |
| 4           | An I/O error, or a serious processing error during runtime                      |

### 1.7 Tar
| Exit Code Number | Meaning                                    | 
|:------------|:-----------------------------------------------:|
| 0           | Success                                         |
| 1           | Some files differ                               |
| 2           | Fatal error                                     |

### 1.8 Wget
| Exit Code Number | Meaning                                    | 
|:------------|:-----------------------------------------------:|
| 0           | Success                                         |
| 1           | Generic error code                              |
| 2           | Parse error                                     |
| 3           | File I/O error                                  |
| 4           | Network error                                   |
| 5           | SSL verification failure                        |
| 6           | Username/password authentication failure        |
| 7           | Protocol errors                                 |
| 8           | Server issues an error response                 |
> With the exceptions of 0 and 1, the lower-numbered exit codes take precedence over higher-numbered ones, 
when multiple types of errors are encountered.

### 1.9 Yum check-update
| Exit Code Number | Meaning                                                                                   |
|:----------------:|:------------------------------------------------------------------------------------------|
| 0                | No packages are available for update                                                      |
| 1                | Error occurred                                                                            |
| 100              | Packages are available for an update. (Also returns a list of the packages to be updated) |

### 1.10 Etc
+ `man <command>` - Check all commands since they may have specific error codes not covered here.

## 2. System Signals
> The most common are SIGHUP, SIGINT, SIGQUIT, SIGKILL, SIGTERM, and SIGSTOP.

| Signal        | Value    | Comment                                     |
|:-------------:|:-------- |:--------------------------------------------|
| SIGHUP        | 1        | Hangup detected on controlling terminal     |
| SIGINT        | 2        | Interrupt from keyboard                     |
| SIGQUIT       | 3        | Quit from keyboard                          |
| SIGILL        | 4        | Illegal Instruction                         |
| SIGABRT       | 6        | Abort signal from abort(3)                  |
| SIGFPE        | 8        | Floating point exception                    |
| SIGKILL       | 9        | Kill signal                                 |
| SIGSEGV       | 11       | Invalid memory reference                    |
| SIGPIPE       | 13       | Broken pipe: write to pipe with no readers  |
| SIGALRM       | 14       | Timer signal from alarm(2)                  |
| SIGTERM       | 15       | Termination signal                          |
| SIGUSR1       | 30,10,16 | User-defined signal 1                       |
| SIGUSR2       | 31,12,17 | User-defined signal 2                       |
| SIGCHLD       | 20,17,18 | Child stopped or terminated                 |
| SIGCONT       | 19,18,25 | Continue if stopped                         |
| SIGSTOP       | 17,19,23 | Stop process                                |
| SIGTSTP       | 18,20,24 | Stop typed at tty                           |
| SIGTTIN       | 21,21,26 | TTY input for background process            |
| SIGTTOU       | 22,22,27 | TTY output for background process           |      
> SIGKILL and SIGSTOP cannot be caught, blocked, or ignored because those signals are handled at the kernel level.
