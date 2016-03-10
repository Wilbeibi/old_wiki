# Wilbeibi's Linux garden

## shell
+ [ps problem1][ps1]
+ [directory shortcut][cdpath]
+ [redirect program output to null][redirect]
+ archive/extract
+ [find file then cd to that directory in Linux][findcd]
  To include symbol links, add `-L`
+ dd to generate random data `dd if=/dev/urandom of=/rfs/nfs/file1 bs=10M count=1`
+ Install Python package to customized directory
+ tcpdump capture all traffic. use `-s` to set snarf
+ filter "Permission denied" or "No such file or directory" in find result `find / -type f -user bandit7 -group bandit6 -size 33c 2>&1 | grep -v "denied\|No"`
+ `$_`: last argument of the previous command [ST: How can I recall arguments of previous command][recall]
+ `du -sh *` show size of each directory
+ extract content between quotes: `grep -o '".*"' somefile | sed 's/"//g'` [st][extract_quote]
+ rename suffix (all cpp files to cc) `for f in *.cpp; do mv "$%"`
+ `Ctrl-\` is QUIT signal
+ Sed use other delimiter other than "/" [Black magic][bm]
+ `:noh` to turn off vim search highlight until new search
+ `tmux source-file ~/.tmux.conf`


## Miscellaneous
### Mount and /etc/fstab
+ They are two ways to mount a disk, the obvious difference is that if we restart
the machine, you need to mount it again. If we write it in /etc/fstab, the system
will mount it for us.
+ detailed info about each parameter are [here][mount]
### Check whether a executable file with gcc's -g flag
+ `file exec_name` if is not stripped, means it contains -g info. [More][strip]
### Makefile
+ only tab, no 4 spaces, no trailing spaces too.
### Bash variables
+ http://stackoverflow.com/questions/5163144/what-are-the-special-dollar-sign-shell-variables

## References
1. [List of Linux guide][guide]
2. [List of Linux howto][howto]
3. [Linux insides][insides]

[guide]: http://www.tldp.org/guides.html
[howto]: http://tldp.org/HOWTO/HOWTO-INDEX/howtos.html
[mount]: https://www.freebsd.org/doc/handbook/mount-unmount.html
[insides]: https://0xax.gitbooks.io/linux-insides/content/index.html
[ps1]: http://unix.stackexchange.com/questions/104821/how-to-stop-a-background-process
[cdpath]: http://unix.stackexchange.com/questions/1469/bash-directory-shortcuts
[redirect]: http://www.cyberciti.biz/faq/how-to-redirect-output-and-errors-to-devnull/
[findcd]: http://stackoverflow.com/questions/3458461/find-file-then-cd-to-that-directory-in-linux
[strip]: http://unix.stackexchange.com/questions/2969/what-are-stripped-and-not-stripped-executables-in-unix
[recall]: http://stackoverflow.com/questions/3371294/how-can-i-recall-the-argument-of-the-previous-bash-command
[extract_quote]: http://unix.stackexchange.com/questions/137030/how-do-i-extract-the-content-of-quoted-strings-from-the-output-of-a-command
[bm]: http://unix.stackexchange.com/questions/39800/how-to-replace-a-string-with-a-string-containing-slash-with-sed
