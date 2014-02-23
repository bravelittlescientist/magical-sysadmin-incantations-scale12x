# Incantations

Credit for this magic goes to George Robinson. http://www.socallinuxexpo.org/scale12x/speakers/george-robinson 

### Old-Timey Incantations

Unzip and Unpack (to live dangerously, pipe a wget to tar)

    gunzip < archive.tar.gz | tar -tvf -

Type in nonsense! (Stop with ^d)

    echo <<EOF

Start program and send to background...

    nohup md5sum bigfile.tgz 2>&1 &

Then manage!

    jobs, fg, ^Z, bg, nice

jobs & vi are in a default install, screen and dtach and disown are not.

### Network Info

Show all open TCP ports and what pid they are directing to (i.e., what
open ports are listening to).

    netstat -plant

Did they put me on the right vlan? Firehose incoming:

    tcpdump -vvv -nn -i eth0

Do I have link?
    
    ethtool eth0

ethtool rejection? 192.51.100.0/24 bogon address.

    ifconfig eth0 192.51.100.0 netmask 255.255.255.0

Flakey network? Check mtu size. Check if bonded via

    cat /proc/net/bonding/bond0

Force bond to an interface

    /sbin/ifenslave -c bond0 eth1

### tcpdump - intermediate magic

tcpdump firehouse with ascii & hex, timestamp, very very verbose, numbers for IP & ports, with line buffer, size, all packets, and count.

    tcpdump -tttt -vvv -nn -XX -l -s0 -c5 -A tcp src 8.8.8.8 and dst port 80 | grep something

### **lsof** - advanced magic

Everything is a file! LiSt Open Files

    lsof | grep

Grep for directories, users, files, pids, or (deleted)

    lsof -p <pid>

    lsof -N <nfs share>

    lsof -D <directory>

### **strace** - wizard, level 1

* -f = follow forks
* -e = <system call>
* -t = time stamp
* -r = relative timing
* -c = count (handy stats)

strace and save output

    strace -o output <command>

strace a running process
    
    strace -p <pid>

What is that program anyway? use **file**, **lsof**, and **strings** for clues.

### **locate**

Find files on a system, but sometimes you are looking for more info. 

Find files modified 10 to 5 days ago

    find /var/log -type f -mtime -10 mtime +5

Now, do something with those files

    find /home/me -type f -name *jpg* -mtime -10 -mtime +5 -print0 | xargs -r0 -P$(nproc) -n10 md5sum

### **grep**

* -v = grab non-matching lines
* -c = count
* -i = ignore case
* -l = list files that match
* -L = list files that do not match
* -r = recurse directories
* -H = print file name
* -h = hide file name
* -n = line number
* -C1 = show 1 line before and after match
* grep 'this\|that'

To do and, just pipe to grep again! Instead of egrep.

    sudo grep --color -linR dhcp /var/log/*

    sudo grep --color -i dhcp /var/log/messages

### **vi** and you

* Vim for color coding
* :set list = show hidden characters
* :set nu = line numbers
* :%s/^bad.*end/good\tend/g = subsitutions
* :n, n, . = next file
* :w, :w!, :wq! is bad!, :q! is good, ZZ (write and quit), view (read-only alias to vi)
* Handling end-of-lines
    *:%s/^V^M//g
    * :%s/^v^M/\r/g
    * :%s/\r/\r/g
    * `dos2unix`
* http://vim-adventures.com

### A little AWKward

Print first, last column

    cat <filename> | awk '{print $1, $NF}'

TODO Rest of awk commands are really long...will wait for slides so I can copy-pasta...

### **sed** and **tr**

Delete leading and trailing whitespace
    
    sed 's/^[ \t]*//;s/[ \t]*$//'

Yet another way to convert dos to unix

    tr -d \r <infile >outfile

### **tail**, **watch**, **wc**

To follow a file, **tail -f** or **less +F**. If you use less instead of tail, you can move around.

Most recently touched files:

    ls -altF | head

Update number of lines in a file (e.g., how is a file system growing?)

    watch -d -n10 wc -l /var/log/messages

TODO: Last one had a typo...will get from slides

### **sort** and **uniq**

Handy scriptlets:

    du -sk * | sort -rn | head
    
    du -sh * | sort -rh | head
    
    grep something <filename> | sort | uniq | wc -l
    
    grep something <filename> | sort -u | wc -l
    
    grep something <filename> | sort | uniq -c

### **crontab**

Placeholder: Awesome crontab template!

### The **ssh** you may have missed

Hold keys in memory.

    eval `ssh-agent`; ssh-add

Getting out of an ssh hot mess

    ~.

Love **scp**

TODO: Switch to *terminator*

Setup your remote host to accept ssh on ports 80 and 443. Only open ssh to right port knock.

### Shell grab bag

* mailx -s "subject" me@spam.com
* Learn sar: **sar**, **sar -r**, **sar -d**, **sar -f**
* ls `cat list.txt` = evaluate expression and ls on that
* !$, !!, $?, $1, $*, $NF
* **export HISTCONTROL=ignoreboth** then **<space>command password** -> will not be added to shell history
* history -d <num>
* reset = reset terminal, not computer
* chown -R <user>:<group>
* ls -d /* = just list directories
* ln -s <file> <link>
* sudo !! = sudo last command
* chmod -reference <ref> <target> = copy permissions to another file
* kill -HUP = safer than **kill -1** because **kill 1** kills everything!!!
* clear or ^l (lower L)

### Cool little commands

* wc -l
* bc
* ^wrong^right
* cd -, cd ~
* curl
* (cd /tmp && ls) -> will be evaluated in parenths
* env
* \command (unaliases)

### Uncommon extras to have

* fping = fast ping, **fping -g** generates a list
* mtr = traceroute and ping
* nmon = system performance
* pv = pipe viewer
* pee = send a pipe two directions
* nc (netcat) = shenanigans
* links, elinks, or lynx = handy browser
* ifstat
* iftop
* iperf = performance (speedtest)
* tip or minicom
* meld = visual diff!!! yum
* tcping
* hping

### Mandatory Line Noise

TODO copy pasta

### General Wisdom

* Do not wait for the network guy to do it.
* Everything is a file, never forget.
* Know how to use job and vi, available on all basic systems.
* Learn C, compile, and run.
* Bonus points for assembly.
* Electronics understanding, transistor logic gates, how things go wrong.
* Save early, save often. Save the backups, save the world.
* Copy pasta will save you. Dev first, then production.
* Scripting necessary for repetitive tasks. 
* Version everything you touch.
* Document as you go.
* "Temporary solution" is an oxymoron.
* Do not use passwordless keys!!!
* Make computers work for us not the other way around
