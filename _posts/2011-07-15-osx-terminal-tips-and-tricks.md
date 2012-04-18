---
layout: post
title: OS X Terminal Tips and Tricks  
category: Code
tags: OSX CLI BASH  
---

These are some shortcuts and programs that make my life easier in the terminal on OS X.


# TERMINAL
`CTRL-V` then any character  
- will allow you to type special characters eg. tab in the terminal

`CTRL-X-E`  
- open Vim (actually whatever's stored in $EDITOR) with whatever's on the command line


# BASH
- `$?` = last error
- `$#` = # of commands passed to a script
- `$@` = all commands passed to a script

`filename=$(basename $fullfile)`  
- alternately, `filename=${fullfile##*/}`

`filename_no_final_extension=${filename%.*}`
 
`extension=${filename##*.}`  
- alternately, `extension=${filename/*.}`


# UTILITIES
`pushd` / `popd`  
- Same as `cd` but stores `$PWD` on the stack. Push and pop directories instead of always changing to full paths. *Use this more*.  
- `dirs` shows the stack

`ps -cax`  
- List each program by the program that called it - better for quitting GUI apps

`osascript -e 'tell application "Application" to quit'`  
- safely Quit a GUI program

`open -a 'Program' file_name`  
- Open a GUI program (with an optional file_name)

`pbcopy`/`pbpaste`   
- copy/paste to the OS X clipboard

`awk '{gsub(/\r/, "\n", $0); print $0}'`  
- convert Mac line endings to UNIX line endings  
- (used to use tr, but it doesn't handle unicode very well. awk isn't pretty, but it does it)

`tr '<' '\n<'`  
- find all < characters and put a newline before them (NOT ELEGANT IN OS X SED)  
- alternately `awk '{sub(/</, "\n<", $0); print $0}'`

`sort -k 2.3,5`  
- sort based on the second column, third character through the fifth character

`sort -M`  
- sort based on months (closest to a date sort on text sorts)

`ls -1 > ALLFILES; sort MANIFEST MANIFEST ALLFILES | uniq -u | xargs rm`  
- to remove everything in the file list ALLFILES that's not contained in MANIFEST  
- (you'd probably want to quote the results, because xargs isn't friendly with space characters in filenames)

`find . -name "REGEXP"`  
- find a file, based on a regular expression

`find . -iname "REGEXP" -exec COMMAND {} \;`  
- find a file, case-insensitive, then exec COMMAND on every matched file  
- {} is a substitute for the filename

`find . -type f -name ".*"`  
- find all hidden files (type d for directories)

`find / -type f -name *.zip -size +100M -exec rm -i {} \;`  
- remove all .zip files on the computer, larger than 100M, interactively

`ruby -ne 'puts $_'`  
- takes standard input, runs with $_ being current line until end-of-file (CTRL-D) character

`syslog | grep -i "error"`  
- dump the entire system log file, check for errors

`jot 10`  
- prints 1 2 3 4 5 6 7 8 9 10 with newlines between them.  
- use "-r" for random  
- two numbers starts at the first and does the second number of prints

`defaults write com.apple.dock pinning -string end`  
- dock in the lower-right corner

`plutil -convert xml1 PLIST_FILE`  
- convert a plist file into an XCode-readable format

# RESOURCES
- Good sort examples: <http://www.cyberciti.biz/faq/linux-unix-sort-date-data-using-sortcommand/>
- Good find examples: <http://www.thegeekstuff.com/2009/03/15-practical-linux-find-command-examples/>
- BASH Scripting guide: <http://tldp.org/LDP/abs/html/>
- OSX Server Command Line: <http://www.scribd.com/doc/15339783/Command-Line>
