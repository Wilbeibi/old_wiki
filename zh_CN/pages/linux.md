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
+ `Alt + .`: last argument of the previous command [ST: How can I recall arguments of previous command][recall]
+ `du -sh *` show size of each directory
+ extract content between quotes: `grep -o '".*"' somefile | sed 's/"//g'` [st][extract_quote]
+ rename suffix (all cpp files to cc) `for f in *.cpp; do mv "$%"`
+ `Ctrl-\` is QUIT signal
+ Sed use other delimiter other than "/" [Black magic][bm]
+ `:noh` to turn off vim search highlight until new search
+ `tmux source-file ~/.tmux.conf`
+ `wget download arbitrary files recursively`
  wget -r --no-parent --reject
+ 当文件太多，run out of inode的时候(watch df -i)， 可以 `ls -f | xargs -n 500 -P 20 rm`。 `ls -f` unsorted 列出文件， xargs 以批处理，每次处理500个，并发20个进程删除。
> Handling files or folders with spaces in the name
One problem with the above examples is that it does not correctly handle files or directories with a space in the name. This is because xargs by default will split on any white-space character. A quick solution to this is to tell find to delimit results with NUL (\0) characters (by supplying -print0 to find), and to tell xargs to split the input on NUL characters as well (-0).

> Remove backup files recursively even if they contain spaces
> find . -name "\*~" -print0 | xargs -0 rm
> Security note: filenames can often contain more than just spaces.
+ `less` injection, when we can access `less` command, we can use `v` and `!` to gain access to edit and shell. Add environment variables
  `export LESSSECURE=1`, we can turn off these features, only show output.

+ [Makefile Automatic variables](http://www.gnu.org/software/make/manual/html_node/Automatic-Variables.html#Automatic-Variables), use `make -p` to print all related variables

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
+ [create tty](http://unix.stackexchange.com/questions/157313/accidentally-deleted-dev-tty-how-to-bring-it-back-on-debian7)
```
mknod /dev/tty c 5 0
chmod 666 /dev/tty
chown root.root /dev/tty
```
### vim
+ search current word (`*`)
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
[wget]: http://stackoverflow.com/questions/273743/using-wget-to-recursively-fetch-a-directory-with-arbitrary-files-in-it
