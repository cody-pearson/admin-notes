# Screen Cheat Sheet

## 0. Customizing
+ *Startup File*: $HOME/.screenrc
	- user's home directory and /usr/local/etc/screenrc
	- Global: $SYSSCREENRC
	- User: $SCREENRC then $HOME/.screenrc
+ *Colon*
	- `Ctrl-a :`  - Allows you to enter .screenrc command lines. Useful for on-the-fly modification of key bindings, specific window creation and changing settings.

## 1. Starting 
+ `screen -dr`						- Reattach a session and if necessary detach it first. 
+ `screen -dR`						- Reattach a session and if necessary detach or even create it first. 
+ `screen -dRR`						- Reattach a session and if necessary detach or create it. Use first session if more are available. 
+ `screen -Dr`						- Reattach a session. If necessary detach and logout remotely first. 
+ `screen -DR`						- If a session is running, then reattach. If it was not running create it and notify the user. 
+ `screen -DRR` 					- Attach here and now. Whatever that means, just do it.
+ `screen -L`						- Tell screen to turn on automatic output logging for the windows.
+ `screen -r [pid.sessionname]`		- Resume a detached screen session.
+ `screen -R`						- Resume the first appropriate detached screen session.
+ `screen -t name`					- Set the title (name) for the default shell or specified program.
+ `screen -wipe [match]`			- List available screens but remove destroyed sessions instead of marking them as ‘dead’.

## 2. Default Key Bindings
+ `Ctrl-a "`										- Present a list of all windows for selection.
+ `Ctrl-a c`										- Create a new window with a shell and switch to that window.
+ `Ctrl-a C`										- Clear the screen.
+ `Ctrl-a d`										- Detach screen from this terminal.
+ `Ctrl-a i`										- Uses the message line to display some information about the current window.
+ `screen -ls` or `screen -list`					- Do not start screen, but instead print a list of session identification strings.
+ `Ctrl-a x`										- Call a screenlock program.
+ `Ctrl-a ?`										- Show key bindings.
+ `Ctrl-a Ctrl-\`									- Kill all windows and terminate screen.
+ `Ctrl-a *`										- Show the listing of attached displays.


## 3. Selecting a Window
+ `Ctrl-a <SPC>` or `Ctrl-a n`					 	- Switch to the next window.
+ `Ctrl-a p`					 					- Switch to the previous window.
+ `Ctrl-a Ctrl-a`									- Toggle to the window displayed previously.
+ `Ctrl-a '`										- Prompt for a window identifier and switch.
+ `Ctrl-a 0...9`									- Switch to the window with the number *n*.

## 4. Widow Display
+ `Ctrl-a S`						- Split the current region into two new *horizontal* ones.
+ `Ctrl-a |`						- Split the current region into two new *vertical* ones.
+ `Ctrl-a <Tab>`					- Switch the input focus to the next region.
+ `Ctrl-a Q`						- Delete all regions but the current one.
+ `Ctrl-a X`						- Kill the current region.
+ `Ctrl-a F`						- Change the window size to the size of the current region.

## 5. Window Settings
+ `Ctrl-a A`						- Set the name of the current window. If no name is specified, screen prompts for one.
+ `Ctrl-a k`						- Kill the current window.
+ `Ctrl-a w`						- Show a list of active windows.
	- The current window is marked with a ‘*’; the previous window is marked with a ‘-’; all the windows that are logged in are marked with a ‘$’; a background window that is being monitored and has had activity occur is marked with an ‘@’; a window which has output logging turned on is marked with ‘(L)’; windows in the zombie state are marked with ‘Z’.
+ `Ctrl-a W`						- Toggle the window width between 80 and 132 columns.

## 6. Logging
+ `Ctrl-a h`						- Writes out the currently displayed image to hardcopy.*n* in the default directory.
+ `Ctrl-a H`						- Begins/ends logging of the current window to the file screenlog.n in the default directory.

## 7.Screen Files Referenced
+ /etc/screenrc and /etc/etcscreenrc 
	- Examples in the screen distribution package for private and global initialization files. 
+ $SYSSCREENRC and /local/etc/screenrc
	- Screen initialization commands 
+ $SCREENRC, $HOME/.iscreenrc, $HOME/.screenrc
	- Read in after /local/etc/screenrc 
+ hardcopy.[0-9]
	- Screen images created by the hardcopy command 
+ screenlog.[0-9]
	- Output log files created by the log command 
+ /etc/utmp
	- Login records 
+ $LOCKPRG
	- Program for locking the terminal.
