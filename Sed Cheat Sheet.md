# USEFUL ONE-LINE SCRIPTS FOR SED (Unix stream editor)

## 0. FILE SPACING:
+ `sed G`                   - double space a file
+ `sed '/^$/d;G'`           - double space a file which already has blank lines in it
+ `sed 'G;G'`               - triple space a file
+ `sed 'n;d'`               - undo double-spacing (assumes even-numbered lines are always blank)
+ `sed '/regex/{x;p;x;}'`   - insert a blank line above every line which matches "regex"
+ `sed '/regex/G'`          - insert a blank line below every line which matches "regex"
+ `sed '/regex/{x;p;x;G;}'` - insert a blank line above and below every line which matches "regex"

## 1. NUMBERING:
+ `sed = filename | sed 'N;s/\n/\t/'`                            - number each line of a file
+ `sed = filename | sed 'N; s/^/   /; s/ *\(.\{6,\}\)\n/\1  /'`  - number each line of a file (number on left, right-aligned)
+ `sed '/./=' filename | sed '/./N; s/\n/ /'`                    - number each line of file, but only print numbers if line is not blank
+ `sed -n '$='`                                                  - count lines (emulates "wc -l")

## 2. TEXT CONVERSION AND SUBSTITUTION:

### 2.1 IN UNIX ENVIRONMENT: convert DOS newlines (CR/LF) to Unix format.
+ `sed 's/.$//'`        - assumes that all lines end with CR/LF
+ `sed 's/^M$//'`       - in bash/tcsh, press Ctrl-V then Ctrl-M
+ `sed 's/\x0D$//'`     - works on ssed, gsed 3.02.80 or higher

### 2.1 IN UNIX ENVIRONMENT: convert Unix newlines (LF) to DOS format.
+ `sed "s/$/`echo -e \\\r`/"`      - command line under ksh
+ `sed 's/$'"/`echo \\\r`/"`       - command line under bash
+ `sed "s/$/`echo \\\r`/"`         - command line under zsh
+ `sed 's/$/\r/'`                  - gsed 3.02.80 or higher

### 2.2 IN DOS ENVIRONMENT: convert Unix newlines (LF) to DOS format.
+ `sed "s/$//"`                    - method 1
+ `sed -n p`                       - method 2

### 2.2 IN DOS ENVIRONMENT: convert DOS newlines (CR/LF) to Unix format.
> Can only be done with UnxUtils sed, version 4.0.7 or higher. The
> UnxUtils version can be identified by the custom "--text" switch
> which appears when you use the "--help" switch. Otherwise, changing
> DOS newlines to Unix newlines cannot be done with sed in a DOS
> environment. Use "tr" instead.
+ `sed "s/\r//" infile >outfile`   - UnxUtils sed v4.0.7 or higher
+ `tr -d \r <infile >outfile`      - GNU tr version 1.22 or higher

### 2.3 Text Manipulation
+ `sed 's/^[ \t]*//'`                                 - delete leading whitespace (spaces, tabs) from front of each line
+ `sed 's/[ \t]*$//'`                                 - delete trailing whitespace (spaces, tabs) from end of each line
+ `sed 's/^[ \t]*//;s/[ \t]*$//'`                     - delete BOTH leading and trailing whitespace from each line
+ `sed 's/^/     /'`                                  - insert 5 blank spaces at beginning of each line (make page offset)
+ `sed -e :a -e 's/^.\{1,78\}$/ &/;ta'`               - align all text flush right on a 79-column width
+ `sed 's/foo/bar/'`                                  - substitute "foo" with "bar" only 1st instance in a line
+ `sed 's/foo/bar/4'`                                 - substitute "foo" with "bar" only 4th instance in a line
+ `sed 's/foo/bar/g'`                                 - substitute "foo" with "bar" ALL instances in a line
+ `sed 's/\(.*\)foo\(.*foo\)/\1bar\2/'`               - substitute "foo" with "bar" the next-to-last case
+ `sed 's/\(.*\)foo/\1bar/'`                          - substitute "foo" with "bar" only the last case
+ `sed '/baz/s/foo/bar/g'`                            - substitute "foo" with "bar" ONLY for lines which contain "baz"
+ `sed '/baz/!s/foo/bar/g'`                           - substitute "foo" with "bar" EXCEPT for lines which contain "baz"
+ `sed 's/scarlet/red/g;s/ruby/red/g;s/puce/red/g'`   - change "scarlet" or "ruby" or "puce" to "red" \/ most seds
+ `gsed 's/scarlet\|ruby\|puce/red/g'`                - change "scarlet" or "ruby" or "puce" to "red" \/ GNU sed only
+ `sed '1!G;h;$!d'`                                   - reverse order of lines (emulates "tac") \/ method 1
+ `sed -n '1!G;h;$p'`                                 - reverse order of lines (emulates "tac") \/ method 2
+ `sed '/\n/!G;s/\(.\)\(.*\n\)/&\2\1/;//D;s/.//'`     - reverse each character on the line (emulates "rev")
+ `sed '$!N;s/\n/ /'`                                 - join pairs of lines side-by-side (like "paste")
+ `sed -e :a -e '/\\$/N; s/\\\n//; ta'`               - if a line ends with a backslash, append the next line to it
+ `sed -e :a -e '$!N;s/\n=/ /;ta' -e 'P;D'`           - if a line begins with an equal sign, replace it with a space and append it
+ `gsed ':a;s/\B[0-9]\{3\}\>/,&/;ta'`                    - add commas to numeric strings, changing "1234567" to "1,234,567" \/ GNU sed
+ `sed -e :a -e 's/\(.*[0-9]\)\([0-9]\{3\}\)/\1,\2/;ta'` - add commas to numeric strings, changing "1234567" to "1,234,567" \/ other seds
+ `gsed -r ':a;s/(^|[^0-9.])([0-9]+)([0-9]{3})/\1\2,\3/g;ta'` - add commas to numbers with decimal points and minus signs \/ GNU sed
+ `gsed '0~5G'`                                       - add a blank line every 5 lines (after lines 5, 10, 15, 20, etc.) \/ GNU sed only
+ `sed 'n;n;n;n;G;'`                                  - add a blank line every 5 lines (after lines 5, 10, 15, 20, etc.) \/ other seds

## 3. SELECTIVE PRINTING OF CERTAIN LINES:
+ `sed 10q`                                         - print first 10 lines of file (emulates behavior of "head")
+ `sed q`                                           - print first line of file (emulates "head -1")
+ `sed -e :a -e '$q;N;11,$D;ba'`                    - print the last 10 lines of a file (emulates "tail")
+ `sed '$!N;$!D'`                                   - print the last 2 lines of a file (emulates "tail -2")
+ `sed '$!d'`                                       - print the last line of a file (emulates "tail -1") \/ method 1
+ `sed -n '$p'`                                     - print the last line of a file (emulates "tail -1") \/ method 2
+ `sed -e '$!{h;d;}' -e x`                          - print the next-to-the-last line of a file \/ for 1-line files, print blank line
+ `sed -e '1{$q;}' -e '$!{h;d;}' -e x`              - print the next-to-the-last line of a file \/ for 1-line files, print the line
+ `sed -e '1{$d;}' -e '$!{h;d;}' -e x`              - print the next-to-the-last line of a file \/ for 1-line files, print nothing
+ `sed -n '/regexp/p'`                              - print only lines which match regular expression (emulates "grep") \/ method 1
+ `sed '/regexp/!d'`                                - print only lines which match regular expression (emulates "grep") \/ method 2
+ `sed -n '/regexp/!p'`                             - print only lines which do NOT match regexp (emulates "grep -v") \/ method 1
+ `sed '/regexp/d'`                                 - print only lines which do NOT match regexp (emulates "grep -v") \/ method 2
+ `sed -n '/regexp/{g;1!p;};h'`                     - print the line immediately before a regexp, but not the line containing the regexp
+ `sed -n '/regexp/{n;p;}'`                         - print the line immediately after a regexp, but not the line containing the regexp
+ `sed -n -e '/regexp/{=;x;1!p;g;$!N;p;D;}' -e h`   - print 1 line of context before and after regexp, with line number
+ `sed '/AAA/!d; /BBB/!d; /CCC/!d'`                 - grep for AAA and BBB and CCC (in any order)
+ `sed '/AAA.*BBB.*CCC/!d'`                                         - grep for AAA and BBB and CCC (in that order)
+ `sed -e '/AAA/b' -e '/BBB/b' -e '/CCC/b' -e d`                    - grep for AAA or BBB or CCC (emulates "egrep") \/ most seds
+ `gsed '/AAA\|BBB\|CCC/!d'`                                        - grep for AAA or BBB or CCC (emulates "egrep") \/ GNU sed only
+ `sed -e '/./{H;$!d;}' -e 'x;/AAA/!d;'`                            - print paragraph if it contains AAA (blank lines separate paragraphs)
+ `sed -e '/./{H;$!d;}' -e 'x;/AAA/!d;/BBB/!d;/CCC/!d'`             - print paragraph if it contains AAA and BBB and CCC (in any order)
+ `sed -e '/./{H;$!d;}' -e 'x;/AAA/b' -e '/BBB/b' -e '/CCC/b' -e d` - print paragraph if it contains AAA or BBB or CCC
+ `gsed '/./{H;$!d;};x;/AAA\|BBB\|CCC/b;d'`                         - print paragraph if it contains AAA or BBB or CCC \/ GNU sed only
+ `sed -n '/^.\{65\}/p'`                                            - print only lines of 65 characters or longer
+ `sed -n '/^.\{65\}/!p'`                           - print only lines of less than 65 characters \/ method 1
+ `sed '/^.\{65\}/d'`                               - print only lines of less than 65 characters \/ method 2
+ `sed -n '/regexp/,$p'`                            - print section of file from regular expression to end of file
+ `sed -n '8,12p'`                                  - print section of file based on line numbers (lines 8-12, inclusive) \/ method 1
+ `sed '8,12!d'`                                    - print section of file based on line numbers (lines 8-12, inclusive) \/ method 2
+ `sed -n '52p'`                                    - print line number 52 \/ method 1
+ `sed '52!d'`                                      - print line number 52 \/ method 2
+ `sed '52q;d'`                                     - print line number 52 \/ method 3, efficient on large files
+ `gsed -n '3~7p'`                                  - beginning at line 3, print every 7th line \/ GNU sed only
+ `sed -n '3,${p;n;n;n;n;n;n;}'`                    - beginning at line 3, print every 7th line \/ other seds
+ `sed -n '/Iowa/,/Montana/p'`                      - print section of file between two regular expressions (inclusive) \/ case sensitive

## 4. SELECTIVE DELETION OF CERTAIN LINES:
+ `sed '/Iowa/,/Montana/d'`                                    - print all of file EXCEPT section between 2 regular expressions
+ `sed '$!N; /^\(.*\)\n\1$/!P; D'`                             - delete duplicate, consecutive lines from a file (emulates "uniq")
+ `sed -n 'G; s/\n/&&/; /^\([ -~]*\n\).*\n\1/d; s/\n//; h; P'` - delete duplicate, nonconsecutive lines from a file
+ `sed '$!N; s/^\(.*\)\n\1$/\1/; t; D'`                        - delete all lines except duplicate lines (emulates "uniq -d")
+ `sed '1,10d'`                                                - delete the first 10 lines of a file
+ `sed '$d'`                                                   - delete the last line of a file
+ `sed 'N;$!P;$!D;$d'`                                         - delete the last 2 lines of a file
+ `sed -e :a -e '$d;N;2,10ba' -e 'P;D'`                        - delete the last 10 lines of a file \/ method 1
+ `sed -n -e :a -e '1,10!{P;N;D;};N;ba'`                       - delete the last 10 lines of a file \/ method 2
+ `gsed '0~8d'`                                                - delete every 8th line \/ GNU sed only
+ `sed 'n;n;n;n;n;n;n;d;'`                                     - delete every 8th line \/ other seds
+ `sed '/pattern/d'`                                           - delete lines matching pattern
+ `sed '/^$/d'`                                                - delete ALL blank lines from a file (same as "grep '.' ") \/ method 1
+ `sed '/./!d'`                                                - delete ALL blank lines from a file (same as "grep '.' ") \/ method 2
+ `sed '/./,/^$/!d'`                         - delete all CONSECUTIVE blank lines from file except the first \/ 0 blanks at top, 1 at EOF
+ `sed '/^$/N;/\n$/D'`                       - delete all CONSECUTIVE blank lines from file except the first \/ 1 blank at top, 0 at EOF
+ `sed '/^$/N;/\n$/N;//D'`                   - delete all CONSECUTIVE blank lines from file except the first 2
+ `sed '/./,$!d'`                            - delete all leading blank lines at top of file
+ `sed -e :a -e '/^\n*$/{$d;N;ba' -e '}'`    - delete all trailing blank lines at end of file \/ works on all seds
+ `sed -e :a -e '/^\n*$/N;/\n$/ba'`          - delete all trailing blank lines at end of file \/ ditto, except for gsed 3.02.*
+ `sed -n '/^$/{p;h;};/./{x;/./p;}'`         - delete the last line of each paragraph

## 5. SPECIAL APPLICATIONS:
```
# remove nroff overstrikes (char, backspace) from man pages. The 'echo'
command may need an -e switch if you use Unix System V or bash shell.
sed "s/.`echo \\\b`//g"    # double quotes required for Unix environment
sed 's/.^H//g'             # in bash/tcsh, press Ctrl-V and then Ctrl-H
sed 's/.\x08//g'           # hex expression for sed 1.5, GNU sed, ssed
```
+ `sed '/^$/q'`                                  - get Usenet/e-mail message header \/ deletes everything after first blank line
+ `sed '1,/^$/d'`                                - get Usenet/e-mail message body \/ deletes everything up to first blank line
+ `sed '/^Subject: */!d; s///;q'`                - get Subject header, but remove initial "Subject: " portion
+ `sed '/^Reply-To:/q; /^From:/h; /./d;g;q'`     - get return address header
```
# parse out the address proper. Pulls out the e-mail address by itself
# from the 1-line return address header (see preceding script)
sed 's/ *(.*)//; s/>.*//; s/.*[:<] *//'
```
+ `sed 's/^/> /'`                                                                         - quote a message
+ `sed 's/^> //'`                                                                         - unquote a message
+ `sed -e :a -e 's/<[^>]*>//g;/</N;//ba'`                                                 - remove most HTML tags
+ `sed '/./{H;d;};x;s/\n/={NL}=/g' file | sort | sed '1s/={NL}=//;s/={NL}=/\n/g'`         - sort paragraphs of file alphabetically
+ `gsed '/./{H;d};x;y/\n/\v/' file | sort | sed '1s/\v//;y/\v/\n/'`                       - sort paragraphs of file alphabetically
```
# zip up each .TXT file individually, deleting the source file and
# setting the name of each .ZIP file to the basename of the .TXT file
# (under DOS: the "dir /b" switch returns bare filenames in all caps).
echo @echo off >zipup.bat
dir /b *.txt | sed "s/^\(.*\)\.TXT/pkzip -mo \1 \1.TXT/" >>zipup.bat
```

## 6. TYPICAL USE 
Sed takes one or more editing commands and applies all of them, in sequence, to each line of input. 
After all the commands have been applied to the first input line, that line is output and a second
input line is taken for processing, and the cycle repeats. The preceding examples assume that input 
comes from the standard input device (i.e, the console, normally this will be piped input). One or
more filenames can be appended to the command line if the input does not come from stdin. 
Output is sent to stdout (the screen). Thus:
```
cat filename | sed '10q'        # uses piped input
sed '10q' filename              # same effect, avoids a useless "cat"
sed '10q' filename > newfile    # redirects output to disk
```

## 7. QUOTING SYNTAX
The preceding examples use single quotes ('...') instead of double quotes ("...") to enclose editing commands, since
sed is typically used on a Unix platform. Single quotes prevent the Unix shell from intrepreting the dollar sign ($) 
and backquotes (`...`), which are expanded by the shell if they are enclosed in double quotes. 
Users of the "csh" shell and derivatives will also need to quote the exclamation mark (!) with the backslash (i.e., \!) 
to properly run the examples listed above, even within single quotes. Versions of sed written for DOS invariably require
double quotes ("...") instead of single quotes to enclose editing commands.

## 8. USE OF '\t' IN SED SCRIPTS
For clarity in documentation, we have used the expression '\t' to indicate a tab character (0x09) in the scripts.
However, most versions of sed do not recognize the '\t' abbreviation, so when typing these scripts from the command line, 
you should press the TAB key instead. '\t' is supported as a regular expression metacharacter in awk, perl, and HHsed, 
sedmod, and GNU sed v3.02.80.

## 9. VERSIONS OF SED
Versions of sed do differ, and some slight syntax variation is to be expected. In particular, most do not support the
use of labels (:name) or branch instructions (b,t) within editing commands, except at the end of those commands. 
We have used the syntax which will be portable to most users of sed, even though the popular GNU versions of sed allow
a more succinct syntax. When the reader sees a fairly long command such as this:
```
sed -e '/AAA/b' -e '/BBB/b' -e '/CCC/b' -e d
```
it is heartening to know that GNU sed will let you reduce it to:
```
sed '/AAA/b;/BBB/b;/CCC/b;d'      # or even
sed '/AAA\|BBB\|CCC/b;d'
```
In addition, remember that while many versions of sed accept a command like "/one/ s/RE1/RE2/", 
some do NOT allow "/one/! s/RE1/RE2/", which contains space before the 's'. Omit the space when typing the command.

## 10. OPTIMIZING FOR SPEED
If execution speed needs to be increased (due to large input files or slow processors or hard disks), substitution will
be executed more quickly if the "find" expression is specified before giving the "s/.../.../" instruction. Thus:
```
sed 's/foo/bar/g' filename         # standard replace command
sed '/foo/ s/foo/bar/g' filename   # executes more quickly
sed '/foo/ s//bar/g' filename      # shorthand sed syntax
```
On line selection or deletion in which you only need to output lines from the first part of the file, a "quit" command (q) in 
the script will drastically reduce processing time for large files. Thus:
```
sed -n '45,50p' filename           # print line nos. 45-50 of a file
sed -n '51q;45,50p' filename       # same, but executes much faster
```
