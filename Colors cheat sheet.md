# Terminal Colors

## 1. ANSI Colors
+ `RED="\033[0;31m"`
+ `LRED="\033[1;31m"`
+ `GREEN="\033[0;32m"`
+ `LGREEN="\033[1;32m"`
+ `BROWN="\033[0;33m"`
+ `YELLOW="\033[1;33m"`
+ `BLUE="\033[0;34m"`
+ `LBLUE="\033[1;34m"`
+ `PURPLE="\033[0;35m"`
+ `LPURPLE="\033[1;35m"`
+ `CYAN="\033[0;36m"`
+ `LCYAN="\033[1;36m"`
+ `NOCOLOR="\033[0m"`

## 2. TPUT Colors
+ `red=$(tput setaf 1)`
+ `green=$(tput setaf 2)`
+ `yellow=$(tput setaf 3)`
+ `blue=$(tput setaf 4)`
+ `purple=$(tput setaf 5)`
+ `cyan=$(tput setaf 6)`
+ `white=$(tput setaf 7)`
+ `bold=$(tput bold)`
+ `underline=$(tput smul)`
+ `reset=$(tput sgr0)`

## 3. Long Color Explanation
The count of colors available to tput is given by tput colors. 

To see the basic 8 colors (as used by setf in urxvt terminal and setaf in xterm terminal):
+ `$ printf '\e[%sm ' {30..37} 0; echo`           ### foreground
+ `$ printf '\e[%sm ' {40..47} 0; echo`           ### background

And usually named as this:

|Color    |   #define     |Value|  RGB        |
|:-------:|:-------------:|:---:|:-----------:|
| black   | COLOR_BLACK   |  0  | 0, 0, 0     |
| red     | COLOR_RED     |  1  | max,0,0     |
| green   | COLOR_GREEN   |  2  | 0,max,0     |
| yellow  | COLOR_YELLOW  |  3  | max,max,0   |
| blue    | COLOR_BLUE    |  4  | 0,0,max     |
| magenta | COLOR_MAGENTA |  5  | max,0,max   |
| cyan    | COLOR_CYAN    |  6  | 0,max,max   |
| white   | COLOR_WHITE   |  7  | max,max,max |

To see the extended 256 colors (as used by setaf in urxvt):
+ `$ printf '\e[48;5;%dm ' {0..255}; printf '\e[0m \n'`

If you want numbers and an ordered output:
```
#!/bin/bash
color(){
    for c; do
        printf '\e[48;5;%dm%03d' $c $c
    done
    printf '\e[0m \n'
}

IFS=$' \t\n'
color {0..15}
for ((i=0;i<6;i++)); do
    color $(seq $((i*36+16)) $((i*36+51)))
done
color {232..255}
```

The 16 million colors need quite a bit of code (some consoles can not show this).
The basics is:
+ `fb=3;r=255;g=1;b=1;printf '\e[0;%s8;2;%s;%s;%sm   ' "$fb" "$r" "$g" "$b"`

> fb is front/back or 3/4. 

A simple test of your console capacity to present so many colors is:
```
for r in {200..255..5}; do 
  fb=4;g=1;b=1;printf '\e[0;%s8;2;%s;%s;%sm   ' "$fb" "$r" "$g" "$b" 
done 
echo
```
It will present a red line with a very small change in tone from left to right. 
If that small change is visible, your console is capable of 16 million colors.

> Each r, g, and b is a value from 0 to 255 for RGB (Red,Green,Blue).

If your console type support this, this code will create a color table:
```
mode2header(){
    #### For 16 Million colors use \e[0;38;2;R;G;Bm each RGB is {0..255}
    printf '\e[mR\n' # reset the colors.
    printf '\n\e[m%59s\n' "Some samples of colors for r;g;b. Each one may be 000..255"
    printf '\e[m%59s\n'   "for the ansi option: \e[0;38;2;r;g;bm or \e[0;48;2;r;g;bm :"
}
mode2colors(){
    # foreground or background (only 3 or 4 are accepted)
    local fb="$1"
    [[ $fb != 3 ]] && fb=4
    local samples=(0 63 127 191 255)
    for         r in "${samples[@]}"; do
        for     g in "${samples[@]}"; do
            for b in "${samples[@]}"; do
                printf '\e[0;%s8;2;%s;%s;%sm%03d;%03d;%03d ' "$fb" "$r" "$g" "$b" "$r" "$g" "$b"
            done; printf '\e[m\n'
        done; printf '\e[m'
    done; printf '\e[mReset\n'
}
mode2header
mode2colors 3
mode2colors 4
```

To convert an hex color value to a (nearest) 0-255 color index:
```
fromhex(){
    hex=${1#"#"}
    r=$(printf '0x%0.2s' "$hex")
    g=$(printf '0x%0.2s' ${hex#??})
    b=$(printf '0x%0.2s' ${hex#????})
    printf '%03d' "$(( (r<75?0:(r-35)/40)*6*6 + 
                       (g<75?0:(g-35)/40)*6   +
                       (b<75?0:(b-35)/40)     + 16 ))"
}
```

Use it as:
```
$ fromhex 00fc7b
048
$ fromhex #00fc7b
048
```

To find the color number as used in HTML colors format:
```
#!/bin/dash
tohex(){
    dec=$(($1%256))   ### input must be a number in range 0-255.
    if [ "$dec" -lt "16" ]; then
        bas=$(( dec%16 ))
        mul=128
        [ "$bas" -eq "7" ] && mul=192
        [ "$bas" -eq "8" ] && bas=7
        [ "$bas" -gt "8" ] && mul=255
        a="$((  (bas&1)    *mul ))"
        b="$(( ((bas&2)>>1)*mul ))" 
        c="$(( ((bas&4)>>2)*mul ))"
        printf 'dec= %3s basic= #%02x%02x%02x\n' "$dec" "$a" "$b" "$c"
    elif [ "$dec" -gt 15 ] && [ "$dec" -lt 232 ]; then
        b=$(( (dec-16)%6  )); b=$(( b==0?0: b*40 + 55 ))
        g=$(( (dec-16)/6%6)); g=$(( g==0?0: g*40 + 55 ))
        r=$(( (dec-16)/36 )); r=$(( r==0?0: r*40 + 55 ))
        printf 'dec= %3s color= #%02x%02x%02x\n' "$dec" "$r" "$g" "$b"
    else
        gray=$(( (dec-232)*10+8 ))
        printf 'dec= %3s  gray= #%02x%02x%02x\n' "$dec" "$gray" "$gray" "$gray"
    fi
}

for i in $(seq 0 255); do
    tohex ${i}
done
```

Use it as ("basic" is the first 16 colors, "color" is the main group, "gray" is the last gray colors):
```
$ tohex 125                  ### A number in range 0-255
dec= 125 color= #af005f
$ tohex 6
dec=   6 basic= #008080
$ tohex 235
dec= 235  gray= #262626
```
