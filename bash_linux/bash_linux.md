Bash&Linux
==========

Mostly linux-specific stuff.

## Sections

-   [Text processing](#Text-processing)

-   [GDB magic](#GDB-magic)

-   [Misc](#Misc)

### Text processing

-   Print field 3 if it starts from **some_string**:

        $ awk '$3 ~ /^ *some_string/ {print $3}'

-   Get unique IPs connected to local port 2222:

        $ netstat -nap | grep 2222 | awk '{print $5}' | cut -d: -f1 | sort -u

-   Get a value of the **status** key in a json:

        $ echo '{"jobId":"1","status":"RUNNING"}'|python3 -c 'import sys, json; print(json.load(sys.stdin)[sys.argv[1]])' status

-   Get top5 CPU consumers:

        $ ps aux | sort -nrk 3,3 | head -n 5

-   Get environment of running process:

        $ tr '\0' '\n' < /proc/<pid>/environ

-   Print specific line:

        $ sed -n -e 192997p access.log

-   Add line numbers:

        $ cat access.log |awk '{ print NR,$(NF-1)}'|sort -k 2 > sorted.txt

-   Remove newlines:

        $ tr '\n' ' ' < test.txt

-   See connection number status:

        $ netstat -ant| awk '{print $6}' | sort | uniq -c | sort -n
        $ netstat -ant| awk '{print $5}' |cut -d ':' -f1 | sort | uniq -c | sort -n

-   Get top 10 residential memory consumers with RSS column in Mb:

        $ ps aux --sort +rss | tail -10|awk 'NR==1 {o=$0; a=match($0,$11);}; NR>1 {o=$0;$6=int(10*$5/1024)/10"M";}{ printf "%-8s %6s %-5s %-5s %9s %9s %-8s %-4s %-6s %-5s %s\n", $1, $2, $3, $4, $5, $6, $7, $8, $9, $10, substr(o, a);}'

### GDB magic

-   Dump nginx config from running process:

        # Set pid of nginx master process here
        pid=1234
        # generate gdb commands from the process's memory mappings using awk
        cat /proc/$pid/maps | awk '$6 !~ "^/" {split ($1,addrs,"-"); print "dump memory mem_" addrs[1] " 0x" addrs[1] " 0x" addrs[2] ;}END{print "quit"}' > gdb-commands
        # use gdb with the -x option to dump these memory regions to mem_* files
        gdb -p $pid -x gdb-commands
        # look for some (any) nginx.conf text
        grep worker_connections mem_*
        grep server_name mem_*


-   Get stdout of running process:

        $ gdb -p <PID_OF_CAT_PROCESS> /bin/cat
        (gdb) p close(1)
        $1 = 0
        (gdb) p creat("/tmp/foo3", 0600)
        $2 = 1
        (gdb) q

### Misc

-   Get inotify watches consumers:

        $ lsof -K | grep inotify | (less||more||pg)

    hotfix:

        $ echo 1048576 > /proc/sys/fs/inotify/max_user_watches

-   Bash for-loop cpp-style

        for ((x=; x<=; x++))
        {
          echo $x
        }

-   Ad-hoc network speed test:

        # myserver.local.net
        nc -v -v -l -n 2222 >/dev/null
        # client
        dd if=/dev/zero bs=1024K count=512 | nc -v -v -n myserver.local.net 2222 >/dev/null

-   Simulate packet loss with **iptables**:

        # for randomly dropping 10% of incoming packets:
        iptables -A INPUT -m statistic --mode random --probability 0.1 -j DROP
        # and for dropping 10% of outgoing packets:
        iptables -A OUTPUT -m statistic --mode random --probability 0.1 -j DROP

-   Get your local sshd host fingerprint:

        $ ssh-keygen -lf /etc/ssh/ssh_host_rsa_key.pub
        $ ssh-keygen -E md5 -lf /etc/ssh/ssh_host_ed25519_key.pub

-   Get your ssh pubkey fingerprint:

        $ ssh-keygen -l -E sha256 -f ~/.ssh/id_rsa.pub

-   Watch with pipe (memcached connections example):

        $ watch -n 1 'lsof -i :11211|wc -l'

-   Get a timestamp from dmesg for time **600711.395348**:

        $ ut=`cut -d' ' -f1 </proc/uptime` 
        $ ts=`date +%s` 
        $ date -d "70-1-1 + $ts sec - $ut sec + 600711.395348 sec" +"%F %T"

