# Vim Cheat Sheet

### Global                               
+ `:help keyword` - open help for keyword
+ `:o file` - open file                  
+ `:saveas file` - save file as          
+ `:close` - close current pane          
+ `:set nu` - show line numbers          

### Cursor Movement
+ `h` - move cursor left
+ `j` - move cursor down
+ `k` - move cursor up
+ `l` - move cursor right
+ `H` - move to top of screen
+ `L` - move to bottom of screen
+ `w` - move forward to start of a word
+ `e` - move forward to the end of a word
+ `b` - move backwards to start of a word
+ `0` - move to start of the line
+ `^` - move to first non-blank character of the line
+ `$` - move to the end of the line
+ `g_` - move to the last non-blank character of the line
+ `gg` - move to the first line of the document
+ `G` - move to the last line of the document
+ `5G` - move to line 5
+ `}` - move to next paragraph
+ `{` - move to previous paragraph
+ `Ctrl + d` - move down half a page
+ `Ctrl + u` - move up half a page

### Insert Mode
+ `i` - insert before cursor
+ `I` - insert at beginning of the line
+ `a` - append after the cursor
+ `A` - append at the end of the line
+ `o` - open a new line below the current line
+ `O` - open a new line above the current line
+ `Esc` - exit insert mode

### Editing
+ `r` - replace a single character
+ `R` - replace multiple characters
+ `u` - undo
+ `Ctrl + r` - redo
+ `.` - repeat last command

### Cut and Paste
+ `yy` - copy a line
+ `p` - paste after cursor
+ `P` - paste before cursor
+ `dd` - delete a line
+ `D` - delete to the end of the line
+ `x` - delete a single character

### Exiting
+ `:w` - save the file, don't exit
+ `:wq` - save and quit
+ `:q` - quit
+ `:q!`or`ZQ` - quit and discard unsaved changes

### Search and Replace
+ `/pattern` - search for pattern
+ `?pattern` - search backwards for pattern
+ `n` - repeat search in same direction
+ `N` - repeat search in opposite direction
+ `:s/old/new/g` - replace all old with new throughout file
