# 4. Process and Practises {#process-and-practises}

In this chapter I am going to detail a common work-flow of a penetration tester followed with some tried and tested practises to augment your agile development work-flow.

Coming at the security problem from a penetration testers point of view (red team) can be quite different than coming at it from a software developers point of view (blue team (if you are aware)).

* The penetration tester is trying to find all the faults in your system. This is not limited to the technology aspect either, as we address throughout this book.
* As a web developer, you are more focussed on delivering a solution that makes a problem less of a problem. Often not thinking so much about what could go wrong, but more focused on how to make it work.

In saying that, the two disciplines can work in harmony together. There is a real need for improving most software developers security related awareness, skills and knowledge, and as this book is focussed on the web developer, it is my intention to show you as a web developer, how to take the lessons learnt from the penetration testers perspective and apply some of them to your own work and life. That is right, security needs to be part of who you are. I have the advantage of working in both disciplines and also in some very successful Scrum teams often as Scrum Master. This makes it relatively easy to empathise to both sides and pull the best from both sides, then apply it to the other side. Thus attempting to bring both in to harmony.

## Penetration Testing {#process-and-practises-penetration-testing}

The following is the process and the steps commonly taken by a penetration tester.

### Reconnaissance {#process-and-practises-penetration-testing-reconnaissance}

This is the act of information gathering. The quieter you can do this, the less likely you will be to raise suspicions or raise your clients defences.  
Here we want to gather as much information that will be potentially useful for taking into the following stages. Where we start to obtain more information about services and other software being used and their versions. Moving from passive to more active techniques. We need to learn as much as possible about the people involved within, related to and how they are related to the target organisation.  
This way we will be able to create effective attack strategies including non technical aspects such as physical security and pretexts for the people we want to exploit.

#### Reconnaissance Forms {#process-and-practises-penetration-testing-reconnaissance-reconnaissance-forms}

Information gathering can be done in such a way that the target does not know you are doing it (passive) through to where the target should absolutely know you are doing it (active), but all to often the target still does not notice due to insufficient logging, monitoring, alerting (as discussed in several of the following chapters) and someone actually taking notice as discussed in the People chapter around engagement.

#### Passive

This is when the information gathering can not be detected by the target. Information is gathered indirectly from the target. Often from:

* Sources that have had direct relationship with the target
* Social media web sites

The pen tester or attacker can not directly probe the target. They must use third party information. Sometimes this will be out of date, so confirmation needs to be sought. This is usually not to difficult, but it takes more time than if the passive constraint was lifted and the pen tester could probe directly.

If you think back to the diagram in the 300,00' view chapter under [Identify Risks](#starting-with-the-30000-foot-view-identify-risks-likelihood-and-impact), where you have "Your Organisation" and the indirect relationships to the "Competitor", you'll see that your competitor or attacker has what is known as these passive or third party relationships with you via your bank, accountants, domain and/or technical consultants, professional services, telcos, ISP and any other number of intermediaries like: social media, whois, DNS (and reverse) lookups and the ocean of information floating around the internet and even the physical cities which you can see if you are observant. Once you have the names of the technology workers at the target organisation, you can search technology specific forums to see what sort of questions they are asking or possibly blog posting on.

#### Semi-Active

Is gathering information directly from the target, but in a manner that looks non suspicious, as if it was any casual browser, passer by or just normal internet traffic. The `nmap -sV` example below almost fits into this form.

#### Active

Here we interact with the target directly, engaging in activities like:

* Snooping physical premises
* Port scanning the entire range `nmap -p- <target>` and some of the aggressive nmap scanning modes shown below
* Spidering public facing resources. Directories and files, often public without the administrators realising it. If they ran a spidering tool against their servers they would see all the publicly accessible resources.
* Banner grabbing and probing, which we address below under the [Service Fingerprinting](#process-and-practises-penetration-testing-reconnaissance-service-fingerprinting) section.

##### Netcat

&nbsp;

No where near as configurable as a dedicated port scanner, but still scans ports is Netcat. Netcat is a network Swiss army knife. It can be used to:

* Host a web page
* Send files
* Poke at things to see what happens
* Listen for a reverse shell: `nc -l <listening port>`
* So many other uses

Lets look at some uses.

{linenos=off, lang=Bash}
    # -z is the argument to instruct for a port scan.
    # 1-1000 is the port range
    # -r randomises the order of port scans to make it a little less obvious
    # -w 1 instructs nc to wait 1 second for a response to each port.
    nc -v -r -w 1 -z <target> 1-1000

    # This won't conceal the attackers IP address.
    # For example: sshd logs will show a failed attempt from specific IP address.
    # This is kind of a blunt tool for port scanning.

##### NMap

&nbsp;

**Hardened Web Server**

This is an example of using nmap against a hardened target with a SSH daemon and web server running...

As you can see with using the aggressive option `-A` the pen tester makes a lot of noise and if the logs are being casually inspected by a system administrator, the chance of noticing the probes with `nmap` written all over them is very likely.

{title="nmap command", linenos=off, lang=Bash}
    # Attempt to detect hardened target OS and services running on it.
    nmap -A <target>

{title="nmap result", linenos=off, lang=Bash}
    Nmap scan report for <target> (<target ip>)
    Host is up (0.0014s latency).
    rDNS record for <target ip>: <target>
    Not shown: 997 closed ports
    PORT     STATE    SERVICE    VERSION
    23/tcp   filtered telnet
    22/tcp  open     tcpwrapped
    2000/tcp open     http       Node.js (Express middleware)
    |_http-title: title
    No exact OS matches for host
    (If you know what OS is running on it, see http://nmap.org/submit/ ).
    TCP/IP fingerprint:
    # Some more info that isn't really helpful
    
    Network Distance: 2 hops
    
    TRACEROUTE (using port 139/tcp)
    HOP RTT     ADDRESS
    1   2.58 ms <target router> (<target router ip>)
    2   1.79 ms <target> (<target ip>)
    
    OS and Service detection performed.
    Please report any incorrect results at http://nmap.org/submit/ .
    Nmap done: 1 IP address (1 host up) scanned in 35.18 seconds

{title="target system logs", linenos=off}
    <time> <target> sshd:  refused connect from <kali ip> (<kali ip>)
       <time> <target> <logger instance>:  [info] ::ffff:<kali ip> - - 
       [<time>] "OPTIONS / HTTP/1.1" 200 8 "-" 
       "Mozilla/5.0 (compatible; Nmap Scripting Engine; http://nmap.org/book/nse.html)"
    <time> <target> <logger instance>:  [info] ::ffff:<kali ip> - - 
       [<time>] "GET / HTTP/1.1" 200 56328 "-" 
       "Mozilla/5.0 (compatible; Nmap Scripting Engine; http://nmap.org/book/nse.html)"
    <time> <target> <logger instance>:  [info] ::ffff:<kali ip> - - 
       [<time>] "OPTIONS / HTTP/1.1" 200 8 "-" 
       "Mozilla/5.0 (compatible; Nmap Scripting Engine; http://nmap.org/book/nse.html)"
    <time> <target> <logger instance>:  [info] ::ffff:<kali ip> - - 
       [<time>] "GET /master.jsp HTTP/1.1" 404 23 "-" 
       "Mozilla/5.0 (compatible; Nmap Scripting Engine; http://nmap.org/book/nse.html)"
    <time> <target> <logger instance>:  [info] ::ffff:<kali ip> - - 
       [<time>] "GET /flumemaster.jsp HTTP/1.1" 404 28 "-" 
       "Mozilla/5.0 (compatible; Nmap Scripting Engine; http://nmap.org/book/nse.html)"
    <time> <target> <logger instance>:  [info] ::ffff:<kali ip> - - 
       [<time>] "GET /tasktracker.jsp HTTP/1.1" 404 28 "-" 
       "Mozilla/5.0 (compatible; Nmap Scripting Engine; http://nmap.org/book/nse.html)"
    <time> <target> <logger instance>:  [info] ::ffff:<kali ip> - - 
       [<time>] "GET /dfshealth.jsp HTTP/1.1" 404 26 "-" 
       "Mozilla/5.0 (compatible; Nmap Scripting Engine; http://nmap.org/book/nse.html)"
    <time> <target> <logger instance>:  [info] ::ffff:<kali ip> - - 
       [<time>] "GET /jobtracker.jsp HTTP/1.1" 404 27 "-" 
       "Mozilla/5.0 (compatible; Nmap Scripting Engine; http://nmap.org/book/nse.html)"
    <time> <target> <logger instance>:  [info] ::ffff:<kali ip> - - 
       [<time>] "GET /robots.txt HTTP/1.1" 404 23 "-" 
       "Mozilla/5.0 (compatible; Nmap Scripting Engine; http://nmap.org/book/nse.html)"
    <time> <target> <logger instance>:  [info] ::ffff:<kali ip> - - 
       [<time>] "GET /status.jsp HTTP/1.1" 404 23 "-" 
       "Mozilla/5.0 (compatible; Nmap Scripting Engine; http://nmap.org/book/nse.html)"
    <time> <target> <logger instance>:  [info] ::ffff:<kali ip> - - 
       [<time>] "GET /rs-status HTTP/1.1" 404 22 "-" 
       "Mozilla/5.0 (compatible; Nmap Scripting Engine; http://nmap.org/book/nse.html)"
    <time> <target> <logger instance>:  [info] ::ffff:<kali ip> - - 
       [<time>] "GET /.git/HEAD HTTP/1.1" 404 22 "-" 
       "Mozilla/5.0 (compatible; Nmap Scripting Engine; http://nmap.org/book/nse.html)"
    <time> <target> <logger instance>:  [info] ::ffff:<kali ip> - - 
       [<time>] "GET /browseDirectory.jsp HTTP/1.1" 404 32 "-" 
       "Mozilla/5.0 (compatible; Nmap Scripting Engine; http://nmap.org/book/nse.html)"
    <time> <target> <logger instance>:  [info] ::ffff:<kali ip> - - 
       [<time>] "OPTIONS / HTTP/1.1" 200 8 "-" 
       "Mozilla/5.0 (compatible; Nmap Scripting Engine; http://nmap.org/book/nse.html)"
    <time> <target> <logger instance>:  [info] ::ffff:<kali ip> - - 
       [<time>] "OPTIONS / HTTP/1.1" 200 8 "-" 
       "Mozilla/5.0 (compatible; Nmap Scripting Engine; http://nmap.org/book/nse.html)"
    <time> <target> <logger instance>:  [info] ::ffff:<kali ip> - - 
       [<time>] "OPTIONS / HTTP/1.1" 200 8 "-" 
       "Mozilla/5.0 (compatible; Nmap Scripting Engine; http://nmap.org/book/nse.html)"
    <time> <target> <logger instance>:  [info] ::ffff:<kali ip> - - 
       [<time>] "OPTIONS / HTTP/1.1" 200 8 "-" 
       "Mozilla/5.0 (compatible; Nmap Scripting Engine; http://nmap.org/book/nse.html)"
    <time> <target> <logger instance>:  [info] ::ffff:<kali ip> - - 
       [<time>] "OPTIONS / HTTP/1.1" 200 8 "-" 
       "Mozilla/5.0 (compatible; Nmap Scripting Engine; http://nmap.org/book/nse.html)"
    <time> <target> <logger instance>:  [info] ::ffff:<kali ip> - - 
       [<time>] "OPTIONS / HTTP/1.1" 200 8 "-" 
       "Mozilla/5.0 (compatible; Nmap Scripting Engine; http://nmap.org/book/nse.html)"
    <time> <target> <logger instance>:  [info] ::ffff:<kali ip> - - 
       [<time>] "OPTIONS / HTTP/1.1" 200 8 "-" 
       "Mozilla/5.0 (compatible; Nmap Scripting Engine; http://nmap.org/book/nse.html)"
    <time> <target> <logger instance>:  [info] ::ffff:<kali ip> - - 
       [<time>] "OPTIONS / HTTP/1.1" 200 8 "-" 
       "Mozilla/5.0 (compatible; Nmap Scripting Engine; http://nmap.org/book/nse.html)"
    <time> <target> sshd:  refused connect from <kali ip> (<kali ip>)

Now using the service detection option `-sV` the results provide almost as much information. Even with the intensity maxed out, the pen tester makes very little noise and if the logs are being even closely inspected, the chance of missing these probes is likely. The only part that really stands out at all is the `sshd` auth request failure. As there are only two of these lines, it's likely that a system administrator wouldn't think that much of it. Without the `sshd` entries, this would fit into the Semi-Active form mentioned above.

{title="nmap command", linenos=off, lang=Bash}
    nmap -sV --version-intensity 9 <target>

{title="nmap result", linenos=off}
    Nmap scan report for <target> (<target ip>)
    Host is up (0.0061s latency).
    rDNS record for <target ip>: <target>
    Not shown: 997 closed ports
    PORT     STATE    SERVICE    VERSION
    23/tcp   filtered telnet
    22/tcp  open     tcpwrapped
    2000/tcp open     http       Node.js (Express middleware)
    
    Service detection performed. Please report any incorrect results at http://nmap.org/submit/
    Nmap done: 1 IP address (1 host up) scanned in 17.63 seconds

{title="target system logs", linenos=off}
    <time> <target> sshd:  refused connect from <kali ip> (<kali ip>)
    <time> <target> <logger instance>:  [info] ::ffff:<kali ip> - - 
       <time> "GET / HTTP/1.0" 200 56328 "-" "-"
    <time> <target> sshd:  refused connect from <kali ip> (<kali ip>)

**Metasploitable 2**

This is an example of using nmap against an un-hardened target. I discuss in later chapters how to go about the hardening process. I have also provided a lot of information around hardening servers on my [blog](http://blog.binarymist.net/).

{title="nmap command", linenos=off, lang=Bash}
    # Attempt to detect un-hardened target OS and services running on it.
    nmap -A <target>

{title="nmap result", linenos=off}
    Starting Nmap 6.47 ( http://nmap.org ) at 2015-11-12 20:17 NZDT
    Nmap scan report for <target>
    Host is up (0.00067s latency).
    Not shown: 977 closed ports
    PORT     STATE SERVICE     VERSION
    21/tcp   open  ftp         vsftpd 2.3.4
    |_ftp-anon: Anonymous FTP login allowed (FTP code 230)
    22/tcp   open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
    | ssh-hostkey: 
    |   1024 60:0f:cf:e1:c0:5f:6a:74:d6:90:24:fa:c4:d5:6c:cd (DSA)
    |_  2048 56:56:24:0f:21:1d:de:a7:2b:ae:61:b1:24:3d:e8:f3 (RSA)
    23/tcp   open  telnet      Linux telnetd
    25/tcp   open  smtp        Postfix smtpd
    |_smtp-commands: metasploitable.localdomain, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN, 
    | ssl-cert: Subject: commonName=ubuntu804-base.localdomain/organizationName=OCOSA/stateOrProvinceName=There is no such thing outside US/countryName=XX
    | Not valid before: 2010-03-17T13:07:45+00:00
    |_Not valid after:  2010-04-16T14:07:45+00:00
    |_ssl-date: 2015-11-12T07:17:50+00:00; -2s from local time.
    53/tcp   open  domain      ISC BIND 9.4.2
    | dns-nsid: 
    |_  bind.version: 9.4.2
    80/tcp   open  http        Apache httpd 2.2.8 ((Ubuntu) DAV/2)
    |_http-methods: No Allow or Public header in OPTIONS response (status code 200)
    |_http-title: Metasploitable2 - Linux
    111/tcp  open  rpcbind     2 (RPC #100000)
    | rpcinfo: 
    |   program version   port/proto  service
    |   100000  2            111/tcp  rpcbind
    |   100000  2            111/udp  rpcbind
    |   100003  2,3,4       2049/tcp  nfs
    |   100003  2,3,4       2049/udp  nfs
    |   100005  1,2,3      37486/udp  mountd
    |   100005  1,2,3      52274/tcp  mountd
    |   100021  1,3,4      48376/tcp  nlockmgr
    |   100021  1,3,4      53826/udp  nlockmgr
    |   100024  1          36762/tcp  status
    |_  100024  1          45387/udp  status
    139/tcp  open  netbios-ssn Samba smbd 3.X (workgroup: WORKGROUP)
    445/tcp  open  netbios-ssn Samba smbd 3.X (workgroup: WORKGROUP)
    512/tcp  open  exec        netkit-rsh rexecd
    513/tcp  open  login?
    514/tcp  open  shell?
    1099/tcp open  java-rmi    Java RMI Registry
    1524/tcp open  shell       Metasploitable root shell
    2049/tcp open  nfs         2-4 (RPC #100003)
    | rpcinfo: 
    |   program version   port/proto  service
    |   100000  2            111/tcp  rpcbind
    |   100000  2            111/udp  rpcbind
    |   100003  2,3,4       2049/tcp  nfs
    |   100003  2,3,4       2049/udp  nfs
    |   100005  1,2,3      37486/udp  mountd
    |   100005  1,2,3      52274/tcp  mountd
    |   100021  1,3,4      48376/tcp  nlockmgr
    |   100021  1,3,4      53826/udp  nlockmgr
    |   100024  1          36762/tcp  status
    |_  100024  1          45387/udp  status
    2121/tcp open  ftp         ProFTPD 1.3.1
    3306/tcp open  mysql       MySQL 5.0.51a-3ubuntu5
    | mysql-info: 
    |   Protocol: 53
    |   Version: .0.51a-3ubuntu5
    |   Thread ID: 24
    |   Capabilities flags: 43564
    |   Some Capabilities: SupportsTransactions, Support41Auth, LongColumnFlag, SwitchToSSLAfterHandshake, Speaks41ProtocolNew, ConnectWithDatabase, SupportsCompression
    |   Status: Autocommit
    |_  Salt: mA;F+EZtW9V%ST-!]1{;
    5432/tcp open  postgresql  PostgreSQL DB 8.3.0 - 8.3.7
    5900/tcp open  vnc         VNC (protocol 3.3)
    | vnc-info: 
    |   Protocol version: 3.3
    |   Security types: 
    |_    Unknown security type (33554432)
    6000/tcp open  X11         (access denied)
    6667/tcp open  irc         Unreal ircd
    | irc-info: 
    |   server: irc.Metasploitable.LAN
    |   version: Unreal3.2.8.1. irc.Metasploitable.LAN 
    |   servers: 1
    |   users: 1
    |   lservers: 0
    |   lusers: 1
    |   uptime: 0 days, 2:03:56
    |   source host: 3C9CF97E.97684684.FFFA6D49.IP
    |_  source ident: nmap
    8009/tcp open  ajp13       Apache Jserv (Protocol v1.3)
    |_ajp-methods: Failed to get a valid response for the OPTION request
    8180/tcp open  http        Apache Tomcat/Coyote JSP engine 1.1
    |_http-favicon: Apache Tomcat
    |_http-methods: No Allow or Public header in OPTIONS response (status code 200)
    |_http-title: Apache Tomcat/5.5
    1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at http://www.insecure.org/cgi-bin/servicefp-submit.cgi :
    SF-Port514-TCP:V=6.47%I=7%D=11/12%Time=56443D13%P=x86_64-unknown-linux-gnu
    SF:%r(NULL,33,"\x01getnameinfo:\x20Temporary\x20failure\x20in\x20name\x20r
    SF:esolution\n");
    MAC Address: 08:00:27:24:2B:3F (Cadmus Computer Systems)
    Device type: general purpose
    Running: Linux 2.6.X
    OS CPE: cpe:/o:linux:linux_kernel:2.6
    OS details: Linux 2.6.9 - 2.6.33
    Network Distance: 1 hop
    Service Info: Hosts:  metasploitable.localdomain, localhost, irc.Metasploitable.LAN; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
    
    Host script results:
    |_nbstat: NetBIOS name: METASPLOITABLE, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
    | smb-os-discovery: 
    |   OS: Unix (Samba 3.0.20-Debian)
    |   NetBIOS computer name: 
    |   Workgroup: WORKGROUP
    |_  System time: 2015-11-12T02:17:49-05:00
    
    TRACEROUTE
    HOP RTT     ADDRESS
    1   0.67 ms <target>
    
    OS and Service detection performed. Please report any incorrect results at http://nmap.org/submit/ .
    Nmap done: 1 IP address (1 host up) scanned in 44.27 seconds

Now using the service detection option `-sV` on the un-hardened metasploitable 2, we get no where near the verbosity as with `-A` but still get a lot.

{title="nmap command", linenos=off}
    nmap -sV --version-intensity 9 <target>

{title="nmap result", linenos=off}
    Starting Nmap 6.47 ( http://nmap.org ) at 2015-11-12 19:38 NZDT
    Nmap scan report for <target>
    Host is up (0.000085s latency).
    Not shown: 977 closed ports
    PORT     STATE SERVICE     VERSION
    21/tcp   open  ftp         vsftpd 2.3.4
    22/tcp   open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
    23/tcp   open  telnet      Linux telnetd
    25/tcp   open  smtp        Postfix smtpd
    53/tcp   open  domain      ISC BIND 9.4.2
    80/tcp   open  http        Apache httpd 2.2.8 ((Ubuntu) DAV/2)
    111/tcp  open  rpcbind     2 (RPC #100000)
    139/tcp  open  netbios-ssn Samba smbd 3.X (workgroup: WORKGROUP)
    445/tcp  open  netbios-ssn Samba smbd 3.X (workgroup: WORKGROUP)
    512/tcp  open  exec        netkit-rsh rexecd
    513/tcp  open  login?
    514/tcp  open  shell?
    1099/tcp open  rmiregistry GNU Classpath grmiregistry
    1524/tcp open  shell       Metasploitable root shell
    2049/tcp open  nfs         2-4 (RPC #100003)
    2121/tcp open  ftp         ProFTPD 1.3.1
    3306/tcp open  mysql       MySQL 5.0.51a-3ubuntu5
    5432/tcp open  postgresql  PostgreSQL DB 8.3.0 - 8.3.7
    5900/tcp open  vnc         VNC (protocol 3.3)
    6000/tcp open  X11         (access denied)
    6667/tcp open  irc         Unreal ircd
    8009/tcp open  ajp13       Apache Jserv (Protocol v1.3)
    8180/tcp open  http        Apache Tomcat/Coyote JSP engine 1.1
    1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at http://www.insecure.org/cgi-bin/servicefp-submit.cgi :
    SF-Port514-TCP:V=6.47%I=9%D=11/12%Time=564433FA%P=x86_64-unknown-linux-gnu
    SF:%r(NULL,33,"\x01getnameinfo:\x20Temporary\x20failure\x20in\x20name\x20r
    SF:esolution\n");
    MAC Address: 08:00:27:24:2B:3F (Cadmus Computer Systems)
    Service Info: Hosts:  metasploitable.localdomain, localhost, irc.Metasploitable.LAN; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
    
    Service detection performed. Please report any incorrect results at http://nmap.org/submit/ .
    Nmap done: 1 IP address (1 host up) scanned in 11.47 seconds

&nbsp;

As you can see, unless you put a lot of work into hardening a system, it will be blurting out a lot of information, which is excellent news for attackers. Most of the WWW web servers produce information about themselves that look like something between these two systems.

NMap and the scripting engine are a very powerful tool-set for gathering information. From passive to active. There are many scripts available and they are easy to work out what each is for by using the `--script-help` option.

#### Concealing NMap Source IP Address {#process-and-practises-penetration-testing-reconnaissance-concealing-nmap-source-ip-address}

An attacker will often attempt to conceal their source IP address during an NMap port scan with the likes of:

##### Decoy host `-D`

&nbsp;

What this does is make it appear to the target that the scans are coming from all the decoy hosts. You can optionally use ME as one of the decoys to represent your/the attackers address. Putting ME in the sixth position or later will cause some common port scan detectors such as Scanlogd to not show your IP address at all. You can also use `RND` to generate a random non-reserved IP address. For more details check the man page.

You probably want to make sure that the hosts you use as fakes/decoys are actually up otherwise you may `SYN` flood your target(s). There will be no `RST` flag sent to the target being scanned from the decoy thus keeping the connection open. As nmap continues to send more requests to the target with the decoy IP address as the source, the target will maintain a growing list of open connections

A> The `RST` (TCP reset) flag used to be sent indicating that a session be terminated due to problems. In recent years the `RST` flag has been used to terminate sessions instead of `FIN`-> <-`ACK`, <-`FIN` `ACK`-> basically because it is faster and releases stack resources immediately.

With the decoy actually being up and receiving the <-`SYN` `ACK`-> it responds with the TCP reset and the target knows the connecting is finished and releases its resources.

It is also kind of obvious as to which IP address the attack is coming from if the decoy hosts are not actually up.

Using too many decoys will slow your scan down and possibly make the results less accurate. Some ISPs may filter out your spoofed packets.

{linenos=off, lang=Bash}
    # Make sure your decoys are up unless you want to DOS your target.
    nmap -D <decoyip1>,<decoyip2>,<decoyip3>,<decoyip4>,<decoyip5>,ME <target>

##### Idle scan `-sI`

&nbsp;

There are a few things you need to know in order to use the idle scan and be able to reason about what is going to happen. This is a side-channel attack which exploits predictable IP fragmentation ID sequence generation on the zombie (decoy) host to glean information about the open ports on the target. This is a really clever yet simple technique. Intrusion Detection Systems (IDSs) will display the scan as coming from the zombie machine you specify (which must be up and meet certain criteria). Check the [Additional Resources](#additional-resources-process) chapter for further details.

{linenos=off, lang=Bash}
    # 1.1.1.1:1234 is the IP address and port of the decoy machine.
    nmap -sI 1.1.1.1:1234 <target>

#### Service Fingerprinting {#process-and-practises-penetration-testing-reconnaissance-service-fingerprinting}

Adding to what we've just seen above, the simplest way to attempt to deduce the details of the service running bound to a particular port is to just see what banner is returned, or for HTTP, the `Server` header field.

Also feel free to try the same requests against the likes of the Metasploitable 2 VM. Its legitimate HTTP protocol is 1.0

##### Depending on the Server field

&nbsp;

{title="Request", linenos=off, lang=Bash}
    # Run netcat against your targets web server
    nc <target> 80
    # Now you need to issue the request.
    HEAD / HTTP/1.1
    # You will probably need to hit the enter key twice.

Now if the target is running Apache 2.2.3, then you may see something like the following:

{title="Response", linenos=off, lang=HTTP}
    HTTP/1.1 400 Bad Request
    Date: Thu, 29 Oct 2015 04:44:09 GMT
    Server: Apache/2.2.3 (CentOS)
    Connection: close
    Content-Type: text/html; charset=iso-8859-1

You can not rely on the Server field though. It could be obfuscated:

{title="Response", linenos=off, lang=HTTP}
    403 HTTP/1.1 Forbidden
    Date: Thu, 29 Oct 2015 04:44:09 GMT
    Server: Unknown-Webserver/1.0
    Connection: close
    Content-Type: text/html; charset=iso-8859-1

Or if your target is running an Express server, you will probably see something like:

{title="Response", linenos=off, lang=HTTP}
    HTTP/1.1 200 OK
    X-Powered-By: Express
    Content-Type: text/html; charset=utf-8
    Content-Length: 56328
    ETag: W/"dc08-0R4KyLlbTHepNS8qdLYCyQ"
    Date: Thu, 29 Oct 2015 04:31:20 GMT
    Connection: keep-alive

##### Ordering of Header Fields

&nbsp;

Every web server has its own specific ordering of header fields. This is usually more reliable at deducing the server type.

##### Malformed Requests

&nbsp;

Now if we try some malformed requests or requests of non existent resources:

{title="Request (not malformed)", linenos=off}
    # Express is using HTTP 1.1
    nc <experss 4.0 server> 80
    GET / HTTP/1.1

{title="Response", linenos=off, lang=HTTP}
    HTTP/1.1 200 OK
    X-Powered-By: Express
    Content-Type: text/html; charset=utf-8
    Content-Length: 56328
    ETag: W/"dc08-0R4KyLlbTHepNS8qdLYCyQ"
    Date: Thu, 29 Oct 2015 05:03:46 GMT
    Connection: keep-alive
    
    # We get the page markup here.

{title="Request (malformed)", linenos=off}
    nc <experss 4.0 server> 80
    GET / HTTP/3.0

We get a closed connection, but we still get the resource if there is one

{title="Response", linenos=off, lang=HTTP}
    HTTP/1.1 200 OK
    X-Powered-By: Express
    Content-Type: text/html; charset=utf-8
    Content-Length: 56328
    ETag: W/"dc08-0R4KyLlbTHepNS8qdLYCyQ"
    Date: Thu, 29 Oct 2015 05:04:20 GMT
    Connection: close
    
    # We get the page markup here.

&nbsp;

Now we try an Apache server. Different versions have different behaviour also.

{title="Request (not malformed)", linenos=off}
    nc <apache 2.2.3 server> 80
    GET / HTTP/1.0

{title="Response", linenos=off, lang=HTTP}
    HTTP/1.1 200 OK
    Date: Thu, 29 Oct 2015 05:02:03 GMT
    Server: Apache/2.2.3 (CentOS)
    Accept-Ranges: bytes
    Connection: close
    Content-Type: text/html; charset=UTF-8
    
    No Host: header seen.

{title="Request (malformed)", linenos=off}
    nc <apache 2.2.3 server> 80
    GET / HTTP/1.1

{title="Response", linenos=off, lang=HTTP}
    HTTP/1.1 400 Bad Request
    Date: Thu, 29 Oct 2015 05:01:51 GMT
    Server: Apache/2.2.3 (CentOS)
    Content-Length: 226
    Connection: close
    Content-Type: text/html; charset=iso-8859-1
    
    <!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
    <html><head>
    <title>400 Bad Request</title>
    </head><body>
    <h1>Bad Request</h1>
    <p>Your browser sent a request that this server could not understand.<br />
    </p>
    </body></html>

Interesting isn't it? Every server type answers in a different way.

##### Non-existent protocol

&nbsp;

Now if we use a non-existent protocol:

{title="Request", linenos=off}
    nc <express 4.0 server> 80
    GET / CATSFORDINNER/1.1
    # Express ignores cats for dinner. No response


{title="Request", linenos=off}
    nc <apache 2.2.3 server> 80
    GET / CATSFORDINNER/1.0

Now we see Apache is `200 OK` happy to have cats for dinner.

{title="Response", linenos=off}
    HTTP/1.1 200 OK
    Date: Thu, 29 Oct 2015 05:12:51 GMT
    Server: Apache/2.2.3 (CentOS)
    Accept-Ranges: bytes
    Connection: close
    Content-Type: text/html; charset=UTF-8
    
    No Host: header seen.

##### Other Services {#process-and-practises-penetration-testing-reconnaissance-service-fingerprinting-other-services}

&nbsp;

Let us look at SSH.

Using nmaps service detection `-sV` option has the smarts to work most of this out because nmap uses service specific probes. So relying on higher level tools are often quicker and more effective as the manual work has already been done and baked into the tooling.

{linenos=off}
    nmap -sV -p 22 <target>

{title="Results", linenos=off}
    Starting Nmap 6.40 ( http://nmap.org ) at 2015-10-29 19:46 NZDT
    Nmap scan report for <target> (<target ip>)
    Host is up (0.00049s latency).
    rDNS record for <target ip>: <target.domain>
    PORT    STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 6.7p1 Debian 3 (protocol 2.0)
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Most of the time identifying banners are part of the binary. If you want to get rid of them, you will need to modify the source and recompile. Doing this will not stop service identification though. With SSH as with most other services, the version info is baked into the protocol.

{linenos=off}
    nc -v <target> 22

{title="Results", linenos=off}
    Connection to <target> 22 port [tcp/*] succeeded!
    SSH-2.0-OpenSSH_6.7p1 Debian-3

In the VPS chapter we discuss further risks, countermeasures and hardening of the SSH daemon.

#### Web Application Firewall (WAF) Fingerprinting

The fact that a WAF is in place is often given away by simply inspecting the responses from the server side. a WAF may add a cookie, with a little searching you may discover what the WAF is from its cookie. Likewise with inspecting the HTTP headers, there are often giveaways. A session timing out very quickly. That can be seen when you telnet or netcat to a web application, but don't issue a request quickly enough.

##### NMap

has a couple of good scripts out of the box for WAF detection.

To view all of the currently available local nmap scripts:

{linenos=off}
    locate *.nse

will give you the full listing. To narrow down the listing to what we are actually looking for in this case:

{linenos=off}
    nmap --script-help "http-waf*"

Will yield the following two scripts which are very useful:

1. `http-waf-detect.nse`  
Attempts to determine whether the web server is protected by an IDS, IPS or WAF
2. `http-waf-fingerprint.nse`  
Attempts to discover the presence of a WAF, its type and version.

{title="Run both scripts", linenos=off}
    nmap -p80 --script "http-waf-*" <target>

##### [WAFW00F](https://github.com/sandrogauci/wafw00f)

is also an excellent tool included in Kali Linux. WAFW00F or `wafw00f` tests for a large number of known WAFs (26 last time I looked). Running is pretty much self explanatory. Just run it against the target host.

"_Sends a normal HTTP request and analyses the response; this identifies a number of WAF solutions_  
_If that is not successful, it sends a number of (potentially malicious) HTTP requests and uses simple logic to deduce which WAF it is_  
_If that is also not successful, it analyses the responses previously returned and uses another simple algorithm to guess if a WAF or security solution is actively responding to our attacks_"

#### DNS

##### Domain Information Groper (dig) 

&nbsp;

To perform DNS lookup:

{title="dig", linenos=off, lang=Bash}
    dig <domain you are wanting info on>

To perform a reverse lookup on an IP address:

{title="dig reverse lookup", linenos=off, lang=Bash}
    dig -x <ip address>

{title="dig for email servers", linenos=off, lang=Bash}
    # mx (email servers) is the type of record to look for.
    # by default, dig will perform a lookup for an A record
    dig <domain you are wanting info on> mx

There are many other options you can use with dig, the output is clear and informative. There is also the programmes "host" and the deprecated "nslookup" which generally provides less information and uses its own internal libraries as opposed to the OS resolver libraries that dig uses.

##### dnsenum

Will do everything dig can do plus more.
There is no man page for dnsenum. Simply run dnsenum and you will be presented with its help.

In order to find all subdomains associated with the target domain, you can use a wordlist. A good collection can be found in Kali Linux by simply issuing the following command:

{linenos=off}
    locate wordlist

Recon-ng as discussed below also has some in `/usr/share/recon-ng/data/` that it uses for similar tasks. Choose your wordlist then apply it to dnsenum. The results found will only be as good as your wordlist. So choose wisely.  
`/usr/share/dirbuster/wordlists/directories.jbrofuzz` is not great, but it is not bad either

{linenos=off, lang=Bash}
    # This is a noisy command, but it is still passive. The target is not being touched.
    # Be patient, it can take some time if you use a large wordlist.
    dnsenum <target domain> -f <your chosen wordlist>
    # We swapped the target domain for binarymist.net
    # and your chosen wordlist for the directories.jbrofuzz mentioned above.
    # dnsenum can also recurse on all the subdomains with the -r option

This will provide the same results as a simple `dnsenum <target domain>` and then start the bruteforce which lists the IP addresses with the domains and record types

{linenos=off}
    binarymist.net.             3174    IN   A       202.46.170.8
    binarymist.net.             3174    IN   A       202.46.170.8
    binarymist.net.             3174    IN   A       202.46.170.8
    blog.binarymist.net.        3600    IN   CNAME   binarymist.wordpress.com.
    binarymist.wordpress.com.   14400   IN   CNAME   lb.wordpress.com.
    lb.wordpress.com.           225     IN   A       192.0.78.13
    lb.wordpress.com.           225     IN   A       192.0.78.12
    Blog.binarymist.net.        3600    IN   CNAME   binarymist.wordpress.com.
    binarymist.wordpress.com.   14400   IN   CNAME   lb.wordpress.com.
    lb.wordpress.com.           225     IN   A       192.0.78.12
    lb.wordpress.com.           225     IN   A       192.0.78.13
    CODE.binarymist.net.        3600    IN   CNAME   bitbucket.org.
    bitbucket.org.              65798   IN   A       131.103.20.167
    bitbucket.org.              65798   IN   A       131.103.20.168
    code.binarymist.net.        3600    IN   CNAME   bitbucket.org.
    bitbucket.org.              65798   IN   A       131.103.20.168
    bitbucket.org.              65798   IN   A       131.103.20.167
    Code.binarymist.net.        3600    IN   CNAME   bitbucket.org.
    bitbucket.org.              65798   IN   A       131.103.20.167
    bitbucket.org.              65798   IN   A       131.103.20.168
    dashboard.binarymist.net.   3600    IN   A       202.46.170.7
    Dashboard.binarymist.net.   3600    IN   A       202.46.170.7
    www.binarymist.net.         3600    IN   A       202.46.170.8
    WWW.binarymist.net.         3600    IN   A       202.46.170.8
    Www.binarymist.net.         3600    IN   A       202.46.170.8

##### dnsrecon

Is another similar tool to dnsenum with a few different options. It is usually helpful to try several similar tools for the same or similar exercise as you will get different results and it is good not to depend on any single tool for a specific task. Do not become tool dependent.

{linenos=off}
    dnsrecon -d <target domain>

There are many other options. Simply run the tool with no arguments to get its help.

&nbsp;

A> The following three (I like to call multi-tools) tools I've found to be really useful and use before and during just about every penetration testing assignment.

#### theHarvester

Is an Open Source Intelligence Tool (OSINT)

Can be used via discover-scripts.

"_theHarvester is a tool for gathering e-mail accounts, subdomain names, virtual hosts, open ports/ banners, and employee names from different public sources (search engines, pgp key servers)._"

Also useful for finding out what an attacker can find out about you, your organisation or your client.

Installed [out of the box](http://tools.kali.org/information-gathering/theharvester) on Kali Linux.

If not run from discover-scripts:  
within Kali Linux you can go through the menus: Information Gathering -> OSINT Analysis -> theharvester  
or run from the terminal

{linenos=off, lang=Bash}
    theharvester -d <target domain> -b all -c -t
    # Just running theharvester without any options provides examples and details on the options.

The sources used are:

* google search engine
* googleCSE custom search engine. A custom search engine needs to be [created](https://cse.google.co.nz/cse/), then add your API key and CSE id to discovery/googleCSE.py
* google profiles specific
* googleplus: finds users that work in target organisation (uses google search)
* baidu search engine
* yahoo search engine
* bing search engine
* bingapi (API key needs to be added to the discovery/bingsearch.py file)
* -vhost: This is the Microsoft bing virtual hosts search feature. The idea is that you provide an IP address of a web server using the `ip:` operator and the search engine enumerates all of the host names it has in its database
* pgp.rediris.es
* linkedin via specific google search
* twitter accounts related to specific domain (uses google search)
* shodan (for internet connected devices). Their website states they search The Web, Refrigerators, Webcams, Power Plants, IoT, Buildings. To use, you need to register and put your API key in discovery/shodansearch.py

Both passive and active options available.

#### Discover-scripts {#process-and-practises-penetration-testing-reconnaissance-discover-scripts}

Is an excellent Open Source Intelligence Tool (OSINT).

![](images/discover.png)

Discover is a collection of shell scripts to aggregate Kali Linux tools & automate various penetration testing tasks. Both passive and active options which allow you to dig up a lot of dirt on your target long before you start trying to penetrate them. I have found domain and Person to be very useful.

To run, within Kali Linux you just need to run the `/opt/discover/discover.sh` script. This is one of the additional tools we [added](#tooling-setup-kali-linux-tools-i-use-that-need-adding-to-kali-linux-discover-scripts) to Kali.

For example  
Recon -> Domain -> Passive combines:

* goofile
* goog-mail
* goohost
* theHarvester
* Metasploit
* dnsrecon
* URLCrazy
* Whois
* and multiple websites.

Recon -> Domain -> Active combines:

* Nmap
* dnsrecon
* Fierce
* lbd
* Wafw00f
* traceroute
* Whatweb

So rather than getting familiar with all the recon tools at once, you get the benefit of many tools in one. You do not get quite the flexibility of using the tools by themselves though, but as time is often the constraining factor, discover can be a good trade-off.

#### recon-ng {#process-and-practises-penetration-testing-reconnaissance-recon-ng}

Another tool in a similar vein to discover-scripts. Recon-ng has a similar feel to metasploit, but for web based reconnaissance. If you are familiar with the metasploit framework, you will notice most of the options are very similar. Recon-ng has a modular framework, allowing anyone that can write some Python to contribute to the collection of modules. This is a very powerful and easy to use recon tool.

Some of the sort of information you may get is:

1. Additional companies
2. Hosts you did not know existed with their IP addresses, domain names, countries of origin, region, latitude, longitude and the module used to find the information.
3. Contacts, first and last names, email addresses, pgp key associations
4. Vulnerabilities, credentials
5. So many more.

It depends on which moduels you use as to what information you receive.

When you run `recon-ng` with no arguments, you will be presented with the titles of the collections of the included modules and you'll be dropped at a `recon-ng` prompt showing the current workspace you are in. It looks like:

{linenos=off, lang=Bash}
    [recon-ng][default] > 

Type:

{linenos=off}
    help

and you will be presented with the `recon-ng` commands.

Prepend any of the commands with help or simply type the command by itself if you want to know more about the specific command.

For example type `show` and you will be presented with:

{linenos=off}
    Shows various framework items
    
    Usage: show [banner|companies|contacts|credentials|dashboard|domains|hosts|leaks|locations|modules|netblocks|options|ports|profiles|pushpins|schema|vulnerabilities]

The above outputs are actually table names that contain data you add (discussed soon).

Then type:

{linenos=off}
    show modules

and you will be presented with a listing of all the modules in `/usr/share/recon-ng/modules/`:

{title="recon-ng modules", linenos=off}
    Discovery
    ---------
      discovery/info_disclosure/cache_snoop
      discovery/info_disclosure/interesting_files
  
    Exploitation
    ------------
      exploitation/injection/command_injector
      exploitation/injection/xpath_bruter
  
    Import
    ------
      import/csv_file
      import/list
  
    Recon
    -----
      recon/companies-contacts/facebook
      recon/companies-contacts/jigsaw/point_usage
      recon/companies-contacts/jigsaw/purchase_contact
      recon/companies-contacts/jigsaw/search_contacts
      recon/companies-contacts/linkedin_auth
      recon/companies-contacts/linkedin_crawl
      recon/companies-multi/whois_miner
      recon/contacts-contacts/mailtester
      recon/contacts-contacts/mangle
      recon/contacts-contacts/unmangle
      recon/contacts-credentials/hibp_breach
      recon/contacts-credentials/hibp_paste
      recon/contacts-credentials/pwnedlist
      recon/contacts-domains/migrate_contacts
      recon/contacts-profiles/fullcontact
      recon/credentials-credentials/adobe
      recon/credentials-credentials/bozocrack
      recon/credentials-credentials/hashes_org
      recon/credentials-credentials/leakdb
      recon/domains-contacts/pgp_search
      recon/domains-contacts/salesmaple
      recon/domains-contacts/whois_pocs
      recon/domains-credentials/pwnedlist/account_creds
      recon/domains-credentials/pwnedlist/api_usage
      recon/domains-credentials/pwnedlist/domain_creds
      recon/domains-credentials/pwnedlist/domain_ispwned
      recon/domains-credentials/pwnedlist/leak_lookup
      recon/domains-credentials/pwnedlist/leaks_dump
      recon/domains-domains/brute_suffix
      recon/domains-hosts/baidu_site
      recon/domains-hosts/bing_domain_api
      recon/domains-hosts/bing_domain_web
      recon/domains-hosts/brute_hosts
      recon/domains-hosts/builtwith
      recon/domains-hosts/google_site_api
      recon/domains-hosts/google_site_web
      recon/domains-hosts/netcraft
      recon/domains-hosts/shodan_hostname
      recon/domains-hosts/ssl_san
      recon/domains-hosts/vpnhunter
      recon/domains-hosts/yahoo_domain
      recon/domains-vulnerabilities/punkspider
      recon/domains-vulnerabilities/xssed
      recon/domains-vulnerabilities/xssposed
      recon/hosts-domains/migrate_hosts
      recon/hosts-hosts/bing_ip
      recon/hosts-hosts/ip_neighbor
      recon/hosts-hosts/ipinfodb
      recon/hosts-hosts/resolve
      recon/hosts-hosts/reverse_resolve
      recon/locations-locations/geocode
      recon/locations-locations/reverse_geocode
      recon/locations-pushpins/flickr
      recon/locations-pushpins/instagram
      recon/locations-pushpins/picasa
      recon/locations-pushpins/shodan
      recon/locations-pushpins/twitter
      recon/locations-pushpins/youtube
      recon/netblocks-companies/whois_orgs
      recon/netblocks-hosts/reverse_resolve
      recon/netblocks-hosts/shodan_net
      recon/netblocks-ports/census_2012
      recon/ports-hosts/migrate_ports
      recon/profiles-contacts/dev_diver
      recon/profiles-profiles/namechk
      recon/profiles-profiles/profiler
      recon/profiles-profiles/twitter
  
    Reporting
    ---------
      reporting/csv
      reporting/html
      reporting/json
      reporting/list
      reporting/pushpin
      reporting/xlsx
      reporting/xml

A> As `recon-ng` uses workspaces, you can keep all your specific assignment data in its own workspace.

Data is persisted to the file system `/<user>/.recon-ng/workspaces/<your new workspace name>/` as it's gathered. You can exit and restart `recon-ng` anytime without loosing the data you have already gathered. So lets see which workspaces we already have.

{linenos=off, lang=Bash}
    # Typing workspaces alone tells us which arguments workspaces expects.
    workspaces list
    # Should show us that we only have the default workspace.
    workspaces add <your new workspace name>

Usually before you `use` then `run` each desired module, you will want to `add` initial records. The sort of records you may want to add can be seen by typing:

{linenos=off, lang=Bash}
    show

{linenos=off, lang=Bash}
    add domains <a target domain>
    add companies
    # Now you are prompted for some details like name and description

To see what records you have already `add`ed, type `show [item]` (where `item` is actually a table name listed from the output of the `show` command). For example: `show domains` or `show companies`

To delete an item you have added: `del [item] [rowid]`, for example:

{linenos=off, lang=Bash}
    del domains 1 # To remove the first record 

You can also query the workspace database with the `query` command. Just type it and you will get some help and be running in no time. Many of the commands can be performed using `recon-ng` commands or by using the `query` command and building up SQL queries.

To use a specific module:

{linenos=off, lang=Bash}
    use <one of the modules listed from show modules command>

To find out what options if any you need to set for your chosen module, type:

{linenos=off, lang=Bash}
    set
    # This will print the Names of the options which you need to set if not
    # already set and if it is a Required field, Current Value and Description.
    # The Current Value should be the second argument you provide to set.

If you need more information about the current module you have `use`ed, type:

{linenos=off}
    show info

You will be notified when you attempt to `run` if you need an API key. Details on where to find these are on the [`recon-ng` wiki](https://bitbucket.org/LaNMaSteR53/recon-ng/wiki/Usage%20Guide#!acquiring-api-keys).

Before you add any social media API keys, you will want to create throwaway social media accounts, because many of the social media sites you will perform recon on will show the visiting user. You want that visiting user to be your throwaway account.

{linenos=off, lang=Bash}
    # To show your currently installed API keys:
    keys list
    # To add an API key:
    keys add [specific_key_listed_from_previous_command] <yourkey>

Once you are ready to `run` the module you have chosen with the `use` command, just type `run`. All the information will be gathered into the workspace specific database which you can query or create reports from. Between each new module you `use` and `run` you can view what was found simply by watching the screen or:

{linenos=off, lang=Bash}
    query SELECT * FROM [any of the tables listed with the show command]
    # For example:
    query SELECT * FROM contacts
    query SELECT * FROM hosts

or

{linenos=off, lang=Bash}
    use reporting/html
    set CREATOR <you or your company>
    set CUSTOMER <your client/target>
    run
    # Now you will be told where your report lives.
    # It should be in the workspace you created which you can access at any time.

The report types you can use can be seen by typing:

{linenos=off, lang=Bash}
    show modules reporting
    # These include html, json, csv, xml and others

`exit` to quit out of `recon-ng`. Any data gathered will be retained.

The commands may seem a bit heavy to start with, but they're very intuitive. Spend some time with `recon-ng` and it will end up being one of the first recon tools you turn to.

%% #### Maltego

%% I had high hopes for this tool, it did not deliver.

%% Is an Open Source Intelligence (OSINT) and forensics tool from Paterva. The core purpose of Maltego is to determine and map the relationships between:

%% * People
%% * Groups of people (social networks)
%% * Organisations
%% * Websites
%% * Internet infrastructure:
%%   * Domains
%%   * DNS names
%%   * Netblocks and their IP addresses
%% * Phrases
%% * Affiliations
%% * Documents and files

%% and provide visualisations of these relationships in a graphical formate for analysing the links and data mining. Maltego does this with its library of what it calls transforms. Aggregating freely available information from the internet and providing visibility to the usually hidden pieces via visible linkages up to three or four degrees of separation away.

%% **Editions**

%% * Community (free)(which Kali Linux happens to come with)  
%% Maximum of 12 results per scan  
%% Can not paste > 50 entities at once  
%% Runs a bit slower than the commercial edition because communications between client and server is not compressed. It is also not encrypted.
%% API keys expire every couple of days having to be activated again
%% * Commercial  
%% US$760 + US$320 per year there after if you want to renew.  
%% There are bulk discounts.  

%% To run, within Kali Linux you can go through the menus: Information Gathering -> OSINT Analysis -> maltego  
%% or  
%% just [Alt]+[F2] maltego

%% You have to setup an account with Paterva and log in via the client (the GUI).
%% Then You're given the options of which machine to run. Machines are basically what type of fingerprinting you want to run against the target.

%% In most cases the transforms used by the machines were not found. There was not a lot of information anywhere around how to include them. Maybe the community edition that I was using just does not include most of the transforms, which made the tool redundant for me. There were also far fewer results than using the likes of [recon-ng](#process-and-practises-penetration-testing-reconnaissance-recon-ng) for the same target.

#### Password Profiling

Is where an attacker will start to collate information and often feed it into a tool or specific set of tools, or if they have lots of time which doesn't really happen, perform the same process manually. I find that the tools which have well thought out algorithms are the quickest and best way to create a short list of probable passwords to later attempt to access accounts via brute force attack

I discuss this further in the [People](#people-identify-risks-weak-password-strategies) chapter.

### Vulnerability Scanning / Discovery

This is where an attacker directs their attention to obtaining (scanning for) weaknesses of their target. Weaknesses that they think they may be able to find exploits for in the next step. Of course these steps often overlap. The attacker may already have some exploits in mind that they think may work against the target, but they need to come back to this step to verify that there is actually a matching exploitable weakness.

A lot of the work the attacker does in the reconnaissance stage will reveal not only all sorts of useful information but vulnerabilities as well.

I'm not going to spend much time in this section looking for vulnerabilities, as we do this throughout the book, especially in the Identify Risks sections in most chapters. I will list a handful of tools though that I find very useful in this stage.

#### NMap

Along with its scripts and ability to extend provides a very powerful and easy to use tool for locating potential vulnerabilities. There are many good and easy to find resources around nmap, some of which we have already covered and others through the rest of the book.

#### Metasploit

msfconsole has many modules out of the box for poking and prodding at things (scanning for vulnerabilities). Once you have started the msfconsole dependencies, `postgresql` and `metasploit`, start `msfconsole`. Show the modules available:

{title="using modules", linenos=off, lang=Bash}
    show auxiliary
    # Will list all the auxiliary modules available.
    # which include many scanners.

    # Once you decide which one you want to use or look at:
    use [yourchosenmodule] # For example:
    use auxiliary/scanner/ssh/ssh_version

    # Show all available options with:
    show options

    # Set any mandatory or optional options with:
    set [theoption] # For example:
    set RHOSTS 203.97.26.48
    # Alternative target port?
    set RPORT 20

    # Lets run it:
    run


I also wrote about a few other vulnerability scanners on my [blog](http://blog.binarymist.net/2014/03/29/up-and-running-with-kali-linux-and-friends/#vulnerability-scanners) such as:

* OpenVAS
* OWASP ZAP, which we use in a few places in this book. ZAP is also an HTTP intercepting proxy with plenty of great built in features and is of course free and open source.
* SkipFish
* Web Application Attack and Audit Framework (w3af)
* Nikto

There are also many scanning tools installed out of the box in Kali Linux.

Worth keeping in mind, is that most of this scanning makes quite a bit of noise and has the potential for being noticed very easily if people are watching logs, a good suite of event notifications are set-up, IDSs are up to date and running.

### Vulnerability Searching {#process-and-practises-penetration-testing-vulnerability-searching}

The vulnerability advisories were mentioned in the [30,000 View](#vulnerability-advisories) chapter. I cover some here in a little detail.

We should now have a good idea of some of the targets weaknesses. It is time to find some exploits.

#### Security Focus BugTraq

Continues to be an excellent source of exploits based on known vulnerabilities and also vulnerabilities themselves. You can search by vendor or CVE.

![](images/SecurityFocusBugTraq.png)

#### Exploit Database

Which can be found at [github](https://github.com/offensive-security/exploit-database). This is from Offensive Security, the creators of Kali Linux. Which has a very easy to use web front-end

![](images/ExploitDd.png)

Linked from the Exploit Database is the Google Hacking Database which is very useful for finding devious goodies on the inter-webs.

and a CLI: searchsploit found in Kali Linux.  
Menu: Exploitation Tools -> Exploit Database -> searchsploit  
Or run from the command line: `searchsploit <search phrase>`

#### Metasploit

To find an exploit or any other type of module easily. Once you have `msfconsole` running:

{title="search for modules to exploit nodejs", linenos=off}
    search nodejs
    # Provided 21 modules targeting NodeJS.
    # 10 of which were exploits, 8 were payloads.

### Exploitation

During this stage, thinking about and recording countermeasures for the vulnerabilities you are able to exploit successfully is just as important as finding and recording the exploited vulnerabilities. That is why each of the following chapters contain the Countermeasures sections.

Beat your own systems up and watch logs. Get familiar with signatures of different tools and attacks, then you will know when you are actually under attack. You can take the same concept with active and semi-active reconnaissance. This way, you will be able to pre-empt your attackers exploitation.

We will go through the actual exploitation that an attacker or penetration tester would carry out in each of the following chapters Identify Risks sections.

#### Isolating, Testing Potential Malware

There are many ways to isolate potentially infectious malware, such as air gaping, virtualisation, Linux Containers (LXC and later LXD) to an extent and their derivatives. Then the purpose built tools such as Firejail and Qubes.

Air gaping can be somewhat impractical, plus with the likes of wifi, bluetooth, etc, air gaps are not always as effective as they used to be.

I'm going to make a few suggestions. This topic as most, goes very deep and broad and could be the content of an array of books. I'll attempt to give you an idea of some of the offerings that you could consider for this. Generally speaking, the order of offerings presented below runs from systems with some isolation first, down to systems purposely designed for containing infectious malware toward the bottom of this section.

##### linux containers (LXC)

Containers on a host share the same kernel and operating system. The rest of the operating system files can be unique per container. This makes it "possible" to isolate running applications within a container from other applications on the container host.

%% _Todo_ Discuss the level of isolation here if time permits.

Windows containers are also coming along, but they are also scoped to the windows operating system they run inside.

In terms of performance, containers beat VMs because they share a lot more, which also means that in terms of isolating malware, generally speaking VMs do a better job.

##### Docker

![](images/DockerHighLevel.png)
%% Image in Alerts account

Docker was open sourced in [March 2013](http://www.infoq.com/news/2013/03/Docker) and is focused on containing application environments. Docker was designed to "_pack, ship and run any application as a lightweight container_" as stated in their github [README.md](https://github.com/docker/docker). Docker containers, unlike virtual machines do not require a separate operating system.

Docker containers can be run on any x64-bit Linux kernel that supports [cgroups](https://en.wikipedia.org/wiki/Cgroups) ("_a Linux kernel feature that limits, accounts for and isolates the resource usage (CPU, memory, disk I/O, network, etc) of a collection of processes_").

Docker used LXC as the default execution environment before the release of its version 0.9 on March 13, 2014. After that Dockers own `libcontainer` library written in GoLang was used. 

It's good to see "Security Disclosure" directions and contact details as the second header on the Docker github page. This installs confidence.

I'm not going to go into Docker in depth here because by default it doesn't really provide enough isolation for the purpose of containing infectious malware. It's reason to exist is more focussed on assisting dev-ops.

##### Virtual Machines

With network adapters configured to provide the isolation you require. VMs whether:
* [Type-1, native or bare-metal](http://blog.binarymist.net/2012/01/23/bare-metal-hypervisor-setup-evaluation/) (Xen, VMware ESX(i), KVM, ProxMox, Archipel, etc) or
* Type-2 or Hosted (VMware Workstation, Server, Player, VirtualBox, etc)) have a greater degree of separation than containers and don't share their operating system with the host or other guests. Although as discussed further on, Type-2 does use the hosting operating systems services which dramatically increases the surface area for attack.

![](images/HypervisorTypesHighLevel.png)
%% Image in Alerts account

The guest (or VM) emulates a real physical machine, but requests for resources such as CPU, memory, disk storage, network, USB, etc are managed by a virtualisation layer controlled by the host system, which delegates its resources as it sees fit. 

Malware can still cross all of these boundaries to varying extents. It's these boundaries that you need to consider when attempting to isolate potentially infectious binaries.

##### [FireJail](https://firejail.wordpress.com/)

FireJail is a SUID (Set owner User ID upon execution) program that sandboxes specified running environments of untrusted processes (servers, graphical applications, user login sessions) using [Linux namespaces](https://lwn.net/Articles/531114/) and [seccomp-bpf](https://l3net.wordpress.com/2015/04/13/firejail-seccomp-guide/) (introduced into the Linux kernel in v3.5). Firejail "_allows a process and all its descendants to have their own private view of the globally shared kernel resources, such as the network stack, process table, mount table._"

"_Written in C with virtually no dependencies, the software runs on any Linux computer with a 3.x kernel version or newer. The sandbox is lightweight, the overhead is low. There are no complicated configuration files to edit, no socket connections open, no daemons running in the background. All security features are implemented directly in Linux kernel and available on any Linux computer._". The source code is on [github](https://github.com/netblue30/firejail). Pre-built DEB, AUR and RPM packages are available for [download](https://firejail.wordpress.com/download-2/). Gentoo is also supported.

Firejail has been modelled on the security sandbox that Google Chrome uses.

Firejail "_includes security profiles for a large number of Linux programs_". Additional profiles can be created for programs not included. The profiles can be fine-tuned to suite your needs. Firejail can even run LXC, Docker and OpenVZ containers.

To start a sandbox, just prefix your command with `firejail` like so:

{title="As seen on firejail website", linenos=off, lang=Bash}
    $ firejail firefox                       # starting Mozilla Firefox
    $ firejail transmission-gtk              # starting Transmission BitTorrent 
    $ firejail vlc                           # starting VideoLAN Client
    $ sudo firejail /etc/init.d/nginx start  # starting nginx web server

This can make it really simple to sandbox what-ever you want to test. prefixing your programs like this and creating menu items or modifying existing desktop menu items in this way makes in very convenient to run your programs in the firejail sandbox.

FireJail also provides a separate graphical tool (Firetools). The firejail [web site](https://firejail.wordpress.com/) has good documentation to expand on this.

##### [Qubes](https://www.qubes-os.org/)

Before we look at Qubes, I'd just like to address Tails also, as they're often talked about in the same sentence. Tails is a live system (usually loaded from a DVD, USB stick, or SD card). It leaves no traces on the computer you are loading it onto unless you explicitly ask it to. All connections to the internet are forced to go through the Tor network. Tails whole aim is to provide anonymity for the user. Tails provides many options around anonymity, even forgetting user passwords, which you can specify on boot if you desire. I use Tails on a daily basis on some jobs, but it's target qualities are amnesia and anonymity.

Qubes will not keep you anonymous without customisation. Its primary goal is to keep infectious malware contained and isolated. Technically Qubes is not a Linux distribution, it's closer to being a Xen distro if anything.

Qubes is a standalone operating system which uses Xen to create it's isolated containers (AKA security domains, zones). A fairly standard case would be for a user to have a small number of domains such as: "work", "personal", "banking". Typically a user will use around five domains. You could use more if your levels of paranoia were greater or had other reasons.

Qubes also contains system domains such as a Networking domain (or VM) and USB domains, as it considers these untrusted. Even the USB stacks and drivers are sand-boxed in their own unprivileged VM (currently experimental). A storage domain has also been [considered](https://github.com/QubesOS/qubes-secpack/blob/master/QSBs/qsb-020-2015.txt#L97).

![](images/qubes-arch-diagram-1.png)

In the above image, the "Storage Domain" is actually a USB domain.

It provides proper GUI-level (one of the main goals) isolation. Along with security as one of the primary goals of the GUI virtualisation subsystem, performance was also priority so the virtualised applications feel as if they were executed natively. The GUI infrastructure introduces only about 2500 lines of C code (LOC) into the privileged domain (Dom0), thus keeping the attack surface small.

As opposed to the mainstream desktop operating sytems which:

"_are based on monolithic kernels usually containing tens of millions of lines of code. Most of this code is reachable from untrusted applications via all sorts of APIs, making the attack surface on the kernel huge._". All the networking, USB, etc drivers are also hosted in the kernel.  
Or  
Type-2 (hosted) hypervisors which suffer from similar weaknesses plus some of their own because they run inside of the hosting operating system as processes and/or kernel modules. This means they're trusting the underlying operating systems services for networking, USB, graphics, Human Interface Devices (HIDs) like keyboards and mice. Which means they are inheriting any buggy utilised resources in the underlying operating system. 

Fedora is currently used for the default template for AppVMs. A cut down Fedora and Debian templates are also supported by Invisible Things Lab (the creators of Qubes). There are also community supported templates built around Whonix (providing anonymity), Ubuntu and Archlinux. Windows 7 can be run in a VM, [support for Windows 8+ is in development](https://www.qubes-os.org/doc/windows-appvms/).  
Qubes VMs are light weight so as to be able to easily run many at once, requiring little memory and optimised to start almost instantly. Each VM is automatically assigned a private static IP.

In order to expose any services within a VM to the hosts interface, port forwarding needs to be configured in the host. Qubes provides advanced networking configurations.

There is no direct communication between VMs unless you explicitly set it up. In saying that, Qubes provides secure inter-VM clip-board and file sharing.

### Documenting and Reporting

Just because this section is last in the Penetration Testing section, does not mean it is carried out last. A penetration tester or even an attacker will be recording information throughout the entire engagement. Especially during the Reconnaissance stage.

There are many tools that can help us to capture, organise and provide physical representations of how the data is related. The below are excellent tools for this task. There are also many others. Many of which are [included](http://tools.kali.org/reporting-tools) in Kali Linux.

#### [Dradis](http://dradisframework.org/)

As software engineers often use wikis for storing and collaborating on information that is meant for the teams benefit, Dradis is a similar type of web application that stores all the information of what has been done and what is left to be done on an engagement for info-sec teams. It's self contained and can be run from the likes of a laptop where ever you're working from.

Like good wikis, Dradis provides the ability to add attachments and create reports. Dradis is included in Kali Linux and the source code can be accessed from the dradisframework [repository](https://github.com/dradis/dradisframework) of dradis which is on github. There is a collection of security tools that Dradis [integrates with](https://github.com/dradis/dradisframework#some-of-the-features) and creating connectors to additional tools is stated to be easy.

#### CaseFile

Created by the same organisation as Maltego (which falls into the reconnaissance category), CaseFile has similar characteristics but is targeted toward information that is manually gathered and entered off-line. It doesn't use transforms like Maltego and it's about a third of the price, unless you're using the community edition which is free. Both Maltego and CaseFile community editions are packaged in Kali Linux.

CaseFile is useful for mapping the relationships between the types of information you enter. Providing the ability to view the ascending and descending relationships to many levels of depth. Out of the box, it comes with its own entity types for you to store your information and you can create custom types as well.

Your CSV, XLS and XLSX formatted datasets can also be used and CaseFile will produce visualisations of how the data is related to each other.

## Agile Development and Practices {#process-and-practises-agile-development-and-practices}

A> Taking Back the Security Focus... but why?  
A> Because as technologists we are hired to create business value and reduce business costs. If you look at the [public statistics](http://www.meetup.com/OWASP-New-Zealand-Chapter-Christchurch/events/223462991/) on businesses loosing value due to being compromised regularly, the figures are staggering and they are growing exponentially due to the popularity of cyber-crime. That's why we care.



If I can encourage even one developer within each team to lead the charge on taking back the responsibility of creating solutions that will withstand the types of attacks that are being launched against our physical locations, people, infrastructure, products and way of life on a regular basis today, I think we may be able to move forward and make our industry a better place, a place we want to be part of. 

Primarily this process is about significantly reducing the cost of producing quality products.

The 10,000' View should be carried out each Sprint for each PBI as it is pulled into WIP. If the particular PBI has something of Physical, IoT, Mobile, People, Cloud, VPS, Network, Web Applications attributes, then you can apply the concepts and direction discussed in these chapters to the PBI you are working on.

It is not my intention to encourage you to think about nothing but security (in this book at least), because then you would not deliver a product, but rather for it to become part of who you are, so as part of your normal work-flow the security consciousness and habits you build will be firmly entrenched. Thus your deliverables will be no longer at the bottom of the tree where your attackers will pick from first.

I've compiled a collection of some of the most powerful practises at lifting quality with the least money spent. Use these to augment your agile work-flow. 

&nbsp;

A> Let us look at some of the practices we can use as developers to lift the level of security within our software and how we can integrate them into our Scrum process framework.

### Architecture

There are no architects in the Scrum Team. Just Developers. Some of those developers have architect qualities. They like to step back often to see the bigger picture and understand the interactions between the components and the people using the software. Including every aspect to do with the end product, the people intending to use it and the risks.

Agile architecture does a little design up front in collaboration with the team and everyone that has a vested interest. Agile architecture is not siloed, because it is part of development, just as analysing requirements and testing. It is all done in parallel.

So, we do not really separate the discipline of architecture from a software developer that excels at doing what a traditional architect does as well as engineering. An architect is essentially an extreme version of the 'T' shaped developer. They have very broad knowledge and skill bases, with multiple deep specialities.  
Another defining factor that sets the architect apart is their ability to translate effectively between the most technical people on a project and the least technical. I like to think of them as the people that spend a lot of time riding the elevator of a high rise building. Most technical people toward the bottom of the building and least technical people at the top. The architect spends time on each level taking what he/she has learnt from all the other levels, then translating and applying it to the specific level.

&nbsp;

### Cheapest Place to Deal with Defects

There has been many studies around the costs of finding and fixing defects early as opposed to cowboy coding, planning to fix defects once the product is delivered, or not planning at all.

The following table shows the average cost of fixing defects based on when they were introduced vs when they are detected. Putting the practises in the right order can reduce costs by up to 100 times.

![](images/AverageCostOfFixingDefects.png)

Steve McConnel goes on to say: The data in the above table "_shows that, for example, an architecture defect that costs $1000 to fix when the architecture is being created can cost $15,000 to fix during system test._" The below figure illustrates the same phenomenon.

![](images/AverageCostOfFixingDefectsDiagram.png)

"_The cost to fix a defect rises dramatically as the time from when it's introduced to when it's detected increases. This remains true whether the project is highly sequential (doing 100 percent of requirements and design up front) or highly iterative (doing 5 percent of requirements and design up front)._"

> Code Complete - Steven C. McConnel

### Evil Test Conditions {#process-and-practises-agile-development-and-practices-evil-test-conditions}

Many will be familiar with the test condition workshop that is often used within a Scrum Team immediately before the developers pulling a PBI into work in progress (WIP) start work on it. I'm not going to go into the exercise here, as it is non security related and I have already discussed the test condition workshop many times [on-line](http://blog.binarymist.net/2012/03/24/how-to-optimise-your-testing-effort/#planningTheTestEffort). I am going to provide a slight bent on the exercise though. Just as you would create the usual test conditions:

![](images/TestCondition.png)

Also consider creating evil test conditions. Often developers do not have the mind set for this, that is why we need that "person(s) with security specialisations" within the team, as discussed right at the beginning of the 30,000' View chapter.

![](images/TestConditionEvil.png)

### Security Focussed TDD

Once you have your evil test conditions defined, as usual once your normal test conditions are ready, you start the Test Driven Development (TDD) cycling, but now augmenting those usual test conditions with the evil ones.

![](images/red_green_refactor.jpg)

Adding **security focused TDD/BDD/ATDD** tests. This is the same amount of work as any other developer test driven development, but it has the same huge benefit of bringing the finding of not just faults, but security faults, from where it is very expensive to fix:  

![](images/CostOfChange.png)  

to where it is the cheapest possible place to fix. Putting legs to your test conditions with automatable security focussed "[Given, When, Thens](http://blog.binarymist.net/2012/03/24/how-to-optimise-your-testing-effort/#planningTheTestEffort)":

![](images/CostOfChange-WithSTDD.png)  

Now instead of the business waiting until go-live before contracting the experts to beat up your system, then telling you **your security sucks**, we take a proactive approach (as the self managing team that we are) and move a lot of the effort traditionally performed at go-live, to **up front**, where you yourself as the developer can test and fix. Thus saving embarrassment and the business a lot of money. This is what being a professional developer is all about. Taking the initiative and leading from the front.

What is so good about STDD and SBDD, is that it roles up Specifications, Design, Implementation and Verification all into one process. Thus working toward delivering each increment that is truly ["Done"](http://blog.binarymist.net/2013/03/02/how-to-increase-software-developer-productivity/#definitionOfDone). Driving development with tests is not about testing! It is about creating code that is testable. Testable code is inherently well designed and gives us the ability to reason about the state at any given time.  
OpenSSL Heartbleed and Apples Goto Fail could have been prevented if (S)TDD was used. Check out Mike Bland's [excellent study and POC](http://martinfowler.com/articles/testing-culture.html).

BSIMM has some good [guidance on security testing](https://www.bsimm.com/online/ssdl/st/) also.

<!--- Other Resources: http://www.continuumsecurity.net/services.html#testing -->

### Security Regression Testing {#process-agile-development-and-practices-security-regression-testing}

If you are using NodeJS and have not already got your tests running as part of a CI build or pre-commit hook, check out the Consuming Free and Open Source Tooling section in [Fascicle 1](https://leanpub.com/holistic-infosec-for-web-developers-fascicle1-vps-network-cloud-webapplications) for some info on how to set this up.

Now as I see it, this is one of the biggest wins you can take for minimum expenditure. You can simply continue to use your existing automated test suites and frameworks. All you have to do is add a **security focused test API** into the mix. You can continue to use Your chosen (language specific) TDD/BDD framework of choice. Just proxy your existing tests through the security API. Then simply tell the API to attack your application, targeting all the known vulnerabilities that the API knows about. The API now understands your applications external facing structure due to the fact that it has proxied all your requests and responses. The security API already exists. All you have to do is use it.

I created a [proof of concept (POC)](https://github.com/binarymist/NodeGoat/wiki/Security-Regression-Testing-with-Zap-API) using the existing NodeGoat purposely vulnerable web application written in NodeJS and used the RESTful API of OWASP Zap to proxy the selenium tests, then launch it's own tests. The "own tests" are already part of Zap. It's all done for you for free. We'll discuss soon how to set-up the NodeGoat Web Application along with the Zap REST API.

I demonstrate this to many of my clients and in my training classes.

[OWASP ZAP](https://www.owasp.org/index.php/OWASP_Zed_Attack_Proxy_Project) (which also comes [pre-installed on Kali Linux](http://blog.binarymist.net/2014/03/29/up-and-running-with-kali-linux-and-friends/#zap) ) is a particularly useful tool for security regression testing. It not only provides a manual tool similar to the likes of Burp Suite + with many other features.

The ZAP API can be accessed directly or by any of the following client implementations:

* Node.JS (by way of [zaproxy](https://www.npmjs.com/package/zaproxy)). You can see this all in [my code](https://github.com/binarymist/NodeGoat/tree/master/test/security). The source for zaproxy is in the Zap core project "[zaproxy](https://github.com/zaproxy/zaproxy/tree/develop/nodejs/api/zapv2)"
* Python
* PHP
* Ruby
* .Net
  1. ZapPenTester: [write-up](http://www.codeproject.com/Articles/708129/Automated-penetration-testing-in-the-Microsoft-sta) on codeproject and the [source](https://github.com/gustavorhm/ZapPenTester) (ZapPenTester) on the github gustavorhm account. It is easy to see how the API is started and used from the [Zap.cs](https://github.com/gustavorhm/ZapPenTester/blob/master/ZAPPenTester/Zap.cs) file.
  2. There is also the Zap supported [zap-api-dotnet](https://github.com/zaproxy/zap-api-dotnet).

There is also the [OWASP Secure TDD Project](https://www.owasp.org/index.php/OWASP_Secure_TDD_Project). A .Net solution. This project appears to either be abandoned or just very low activity. Feel free to offer to help though if you are a .Net developer.

There are links to the API clients from the zaproxy wiki on [github](https://github.com/zaproxy/zaproxy/wiki/ApiDetails)

#### Zap REST API Regression Testing NodeGoat

My set-up was done with fetching the NodeGoat source code onto a physical machine with Zap on a Kali Linux VirtualBox guest on the same physical machine.

There are also details on getting Zap running in a docker container on the [Zap wiki](https://github.com/zaproxy/zaproxy/wiki/Docker)


##### NodeGoat Set-up on your local machine {#process-agile-development-and-practices-security-regression-testing-nodegoat-set-up-on-your-local-machine}

The following is also addressed in the [Kali Linux Install](#tooling-setup-kali-linux-kali-linux-install) section of the Tooling Setup chapter.

In VirtualBox you will need a Host-only Network added. By default this will be called `vboxnet0` in your VirtualBox settings. an `ifconfig` on the host will reveal a new network interface called `vboxnet0`. This allows guests and host to communicate with each other without anyone outside of the host network interface being able to see the communications. In this example we set the Adapter IP address to `192.168.56.1`. That address will be assigned to your host as an additional network interface.

With this step taken, you will also need to make sure the `hostName` property in the `config/env/all.js` is set to the same IP address, as this IP address is used in the regression test(s) to inform the selenium web driver where our NodeGoat is listening from. In addition to that, requests are proxied through the Zap API to this same address.

In this example we enable the DHCP server on the Host-only Network by just checking the check-box. The DHCP address will be `192.168.56.2`

If you have a firewall running, You will need to allow TCP in on interface `vboxnet0`. From: `192.168.56.0/24` To: `192.168.56.1` on port `4000` (NodeGoat) and `35729` (LiveReload)

##### Zap Running on a local VirtualBox guest

On the guest machine in the VirtualBox Network settings, set one of the Adapter tabs so that the network adapter is attached to Host-only Adapter. Once the networking is restarted on this guest, it will have a network interface bound to something like `192.168.56.20` (specified by the `192.168.56.0/24` range decided above when you set-up the `vboxnet0` interface).

The Zap local proxy will need to be bound to the same IP address and port that the guest Host-only adapter is bound to in order to have requests it receives sent via the Host-only adapter to the host that NodeGoat is listening on. `192.168.56.1` in this example. By default the Zap local proxy will be set to `127.0.0.1`. Change it to the VMs externally visible network interface. So you can set the Zap local proxy and port to `192.168.56.20` and `8080` for this example via the Zap GUI:  
Tools -> Options -> Local proxy.  
The other way to do it is via the `~/ZAP/config.xml` file in the following section:

{linenos=off}
    <proxy>
       <ip>192.168.56.20</ip>
       <port>8080</port>        
    </proxy>

Update the `zapHostName` and `zapPort` properties in file `config/env/test.js` to reflect the host name (or IP address) and port that the Zap API is listening on within your virtual guest. If you want to debug the security regression tests, add the same properties to the `config/env/development.js` file.

Start Zap the usual way. Zap can and probably should be scripted to start automatically on each test or suite run, also reset the database so you have a known state if you are planning on using the API in your development team. Zap can be terminated via its API and is usually good practice to do so on each test or suite run. If you don't have a UI:

{linenos=off, lang=Bash}
    owasp-zap -daemon

Now you should be able to browse the Zap API from either machine at `http://192.168.56.20:8080/`.

##### Start the Security Regression test(s) from your local machine

For each test run, this is the usual set of steps:

{linenos=off, lang=Bash}
    # In one terminal create clean state in datastore:
    grunt db-reset
    # Then start NodeGoat:
    npm start

{linenos=off, lang=Bash}
    # In another terminal start the security regression test(s):
    grunt testsecurity

By default the XSS vulnerabilities exist in the `/profile` route. By running `grunt testsecurity`, the `test/security/profile-test.js` will be run and you should be informed of a failed test with the following output:

{title="Zap Found Vulnerabilities", linenos=off, lang=Bash}
    me@myBox in Source/NodeGoat git:(workshop) ✗  grunt testsecurity
    Running "env:test" (env) task

    Running "mochaTest:security" (mochaTest) task


      profile regression test suite
    Scan 0 is 26% complete with 10 alerts.
    Scan 0 is 53% complete with 10 alerts.
    Scan 0 is 100% complete with 10 alerts.
    We are finishing scan 0. Please see the report for further details.
    About to write report.
    Scan 0 is 100% complete with 10 alerts.
    Scan 0 is 100% complete with 10 alerts.
    Writing report to Source/NodeGoat/test/security/report_2015-11-24-20-57.html

    Search the generated report for "/profile" to see the 7 vulnerabilities that exceed the user defined threshold of: 3
        1) Should not exceed the decided threshold of vulnerabilities known to Zap


      0 passing (28s)
      1 failing

      1) profile regression test suite Should not exceed the decided threshold of vulnerabilities known to Zap:
         Uncaught AssertionError: expected '10' to be below or equal3
          at Assertion.fail (node_modules/should/lib/assertion.js:180:17)
          at Assertion.prop.value (node_modules/should/lib/assertion.js:65:17)
          at onCompletion (test/security/profile-test.js:124:38)
          at node_modules/async/lib/async.js:721:13
          at node_modules/async/lib/async.js:52:16
          at node_modules/async/lib/async.js:269:32
          at node_modules/async/lib/async.js:44:16
          at node_modules/async/lib/async.js:718:17
          at node_modules/async/lib/async.js:167:37
          at test/security/profile-test.js:268:41


    Warning: Task "mochaTest:security" failed. Used --force, continuing.

    Done, but with warnings.

Once you:

1. Fix the XSS vulnerabilities in the `/profile` route, explained in the NodeGoat [Tutorial Guide](http://nodegoat.herokuapp.com/tutorial/a3#source-code-example)
2. Restart Zap
3. Reset the datastore
4. Restart NodeGoat
5. Rerun the security regression test(s)

You should be informed of a successful test with the following output:

{title="Fixed Vulnerabilities", linenos=off, lang=Bash}
    me@myBox in Source/NodeGoat git:(workshop) ✗  grunt testsecurity
    Running "env:test" (env) task
    
    Running "mochaTest:security" (mochaTest) task
    
    
      profile regression test suite
    Scan 0 is 26% complete with 3 alerts.
    Scan 0 is 66% complete with 3 alerts.
    Scan 0 is 100% complete with 3 alerts.
    We are finishing scan 0. Please see the report for further details.
    About to write report.
    Scan 0 is 100% complete with 3 alerts.
    Scan 0 is 100% complete with 3 alerts.
    Writing report to Source/NodeGoat/test/security/report_2015-11-24-20-52.html
    
        ✓ Should not exceed the decided threshold of vulnerabilities known to Zap (21106ms)
    
    
      1 passing (28s)
    
    
    Done, without errors.

&nbsp;

### Hand-crafted Penetration Testing

Sometimes known as "gorilla testing".

As previously discussed, this can be costly when performed late in a projects life cycle, so by automating as much as possible, including high level scans which help to identify starting points to attack, it leaves the developers to do what they do best...

Get creative.

There is no reason why developers can not take a good chunk of the manual penetration testing effort on as part of their daily development practices. In fact in most teams I have lead, this has been exactly how we have worked. The gorilla testing needs to be performed in parallel with the PBIs in the Scrum Backlog as developers pull them into Work I Progress (WIP).

BSIMM again has some [good guidance](https://www.bsimm.com/online/deployment/pt/) on hands on penetration testing.

### Establish a Security Champion

Some developers have an inquisitiveness about how their work can be exploited, so it is important to have at least a none zero number of developers with a security focus within each team to:

1. Take the lead on the security front
2. Mentor, infect their passion and pass on their knowledge to their co-workers

I mention in the People chapter in the ["Top Developer Motivators in Order"](#people-countermeasures-morale-productivity-and-engagement-killers-top-developer-motivators-in-order) section how developers love being champion of something? The role of security champion or any champion for that matter needs to be applied to the developer as a vacuum. People do not respond well to being pushed into anything. The best developers will just pick up the role, but many will not be as proactive. For the less proactive developers, it pays to create the role and as a Product Owner or manager approach them and ask them if they would like to take the responsibility as security champion. Once a developer has taken up this responsibility, they will usually do a pretty good job of infecting the rest of their team with their passion and knowledge.

### Pair Programming {#process-agile-development-and-practices-pair-programming}

Two pairs of eyes on the same code is proven to drastically reduce defects, and it does it at the cheapest possible place, as they are being created. Pair programming can be a very effective discipline, but not all the time and not for all people. There is a lot of resources on the topic. If you have not tried it, then you should, but it is probably counter productive to mandate that all the developers should do it all of the time. It is probably a good idea to encourage developers to pair on complex tasks. It is a tool, try it and use it wisely.

### Code Review

If we can not get the simple things right like [Coding standards and conventions](http://blog.binarymist.net/2012/12/19/javascript-coding-standards-and-guidelines/) to help remove some of the "wild west" attitudes and behaviours, then how will we ever get the complicated things right? The whole team doesn't necessary have to agree on everything, but needs to be in alignment with and abide by the standards, conventions and guidelines.

Having a pair review your code against the standards you have created and offer [alternative](http://blog.binarymist.net/2014/07/26/node-js-asynchronicity-and-callback-nesting/) approaches and techniques for not only standards violations but for possible semantic modifications.

It's often a good idea to automate as much of your coding standards as possible with static analysis and other related tooling. Ideally using your source control commit hooks to trigger the review process. This is discussed in more detail in the "Consuming Free and Open Source" sections in the Web Applications chapter.

Automating is not going to catch everything though. Real person pair reviews should not be neglected. Team reviews should be carried out in moderation. I've found them to be less effective due to the fact that we loose face when our defects are highlighted in front of the entire team.

The [BSIMM Code Review](https://www.bsimm.com/online/ssdl/cr/) resource has some good ideas worth exploring.

#### Why? {#process-agile-development-and-practices-code-review-why}

Because when humans are in creative mode, we often struggle to see the defects in our own creation. That is why tightly knit teams with high morale (as discussed in the people chapter under the "Morale, Productivity and Engagement Killers" sections) are a force to be reckoned with. We are all watching each others backs. We are good at seeing faults in others and their creations. That is why we really do need each other. Never underestimate this blindness in us all and that we have a very powerful mitigation tool in the team. Use it.

#### Linting, Static Analysis

Start with the likes of JSHint for general purpose JavaScript linting. There are lots of other good tooling options.

Techniques and tools to assist with automating

* [https://wiki.mozilla.org/Security/B2G/JavaScript_code_analysis](https://wiki.mozilla.org/Security/B2G/JavaScript_code_analysis)
  * [DOMXSSScanner](https://github.com/yaph/domxssscanner). Does what the name suggests.
  * [JSPrime](https://www.youtube.com/watch?v=Vk5SPGpqiLc). Security focused. Not currently maintained at this point in time. The owners are keen for someone to pick it up.
  * [JSWebTools](http://www.jswebtools.org/). A collection of research papers and JS related security tools.

#### Dynamic Analysis

Tooling is still immature here. We have got a way to go, but lets start getting our feet wet. Here are a few resources to dip your toes:

* [Titanium Code Processor](https://theoreticalideations.com/tag/titanium-code-processor/) For [Titanium Mobile](https://github.com/appcelerator/titanium_mobile) (native mobile applications using JavaScript) projects. You can find the source code at the appcelerator account on [github](https://github.com/appcelerator/titanium-code-processor) 
* [Slide Deck](https://speakerdeck.com/ariya/dynamic-code-analysis-for-javascript) by Ariya Hidayat.
* [Jalangi](https://www.eecs.berkeley.edu/~gongliang13/jalangi_ff/)

### Techniques for Asserting Discipline

JavaScript is an inherently flexible and undisciplined language. This quality is a double edged sword. It provides us with extreme power and also allows us to slaughter ourselves. Discipline is very much needed in order to stay safe and be able to reason about our applications as they grow larger.

I have been involved in many JavaScript project rescue missions where a development team can no longer understand what the monster they have created is doing, what state it is in and why? In almost all occasions, the developers on the team do not have a deep understanding of the language and often of any language. Because of this I always try and push the fact that JavaScript developers must do everything they can to understand the language. In most cases this means taking their learning home and getting intimate with JavaScripts beauty.

Because of the distinct lack of discipline within JavaScript (unlike most other languages) I try and bring as much understanding around techniques that can help impose the extra rigour where and when it is needed. The beautiful thing about JavaScript is that when you really need to do something unconventional, you can, but I like to **weigh up the trade-offs** of either approach.

#### Static Type Checking

In JavaScript we need as much help as we can to fail fast. Static type checking gives us this. It also feels like the step before [DbC](http://blog.binarymist.net/2010/10/11/lsp-dbc-and-nets-support/).

* [Flow](http://flowtype.org/) looks to be a good option. Providing consumers with the ability of introducing type checking progressively and/or to certain parts that make the most sense. Or rather missing parts that require the extra flexibility.

Here is an example from the flow website.

{title="", linenos=off, lang=JavaScript}
    /* @flow */
    function foo(x) {
      return x * 10;
    }
    foo('Hello, world!');

{title="Run flow", linenos=off, lang=bash}
    $> flow
    hello.js:5:5,19: string
    This type is incompatible with
      hello.js:3:10,15: number
    
{title="Refactoring to DbC", linenos=off, lang=JavaScript}
    function foo(x: string, y: number): string {
      return x.length * y;
    }
    foo('Hello', 42);

{title="Run flow", linenos=off, lang=bash}
    $> flow
    hello.js:3:10,21: number
    This type is incompatible with
      hello.js:2:37,42: string

#### Design by Contract (DbC)

Enforcing preconditions, postconditions and invariants in our routines.
We can even employ this design principle in JavaScript. I wrote about DbC in a [previous post](http://blog.binarymist.net/2010/10/11/lsp-dbc-and-nets-support/#dbc) in regards to usage in .Net.  
In JavaScript, I believe employing the DbC principle is even more important as part of adding discipline and keeping us on the straight and narrow. I believe DbC is the principle that helps us achieve the [Liskov Substitution Principle](http://blog.binarymist.net/2010/10/11/lsp-dbc-and-nets-support/#lsp), which is the 'L' in the SOLID design mnemonic. These are the offerings I have noticed that provide support additionally to the flow just mentioned above:

2. [contract-js on NPM](https://www.npmjs.com/package/contracts-js) is our current leader in the DbC space.
  * [contract.js at home](http://www.contractsjs.org/)
3. [contractual on NPM](https://www.npmjs.com/package/contractual)
4. [ristretto-js](https://code.google.com/p/ristretto-js/w/list). Doesn't look to be currently maintained.

In many cases you can implement your cross cutting code contracts using AOP. This gets it out of your code, so that it is not in your face.

### Essentials for Creating and Maintaining a High Performance Development Team

#### How and Why Many Software Development Shops Fail

What I've seen a lot of, is organisations hiring code monkeys rather than professionals. Either they hire:

* The cheapest talent they can get their hands on. Now they want the best, but how much they have to pay the developer is the most important factor to them.

Or:

* The person that completes feature implementations as fast as possible (sometimes known or thought of as rock stars). Often young developers without a large amount of experience, which causes the more Professional Developers to slow down a bit and think tasks through a little more.  
Now, both approaches are short sighted. They hire code monkeys rather than professionals. Code monkeys write code fast and incur technical debt that is hidden at first, but over time slows the Development Team down until it can barely move.

##### The Scenario {#process-agile-development-and-practices-essentials-for-creating-and-maintaining-a-high-performance-development-team-how-and-why-many-software-development-shops-fail-the-scenario}

Code Monkey finishes his task much faster than Professional Developer.

Code Monkey is solely focused on completing his task as fast as possible. He cuts some code and declares the task done. Professional Developer thinks the problem through, does a little research to satisfy himself that his proposed approach  is in fact the most appropriate approach for the problem. He organises a [test condition workshop](http://blog.binarymist.net/2012/03/24/how-to-optimise-your-testing-effort/#testConditionWorkshop) which solidifies requirements and drives out design defects via active stake holder participation. He drives his low level design with TDD. Makes sure he follows the [coding standards](http://blog.binarymist.net/2013/03/02/how-to-increase-software-developer-productivity/#codingStandardsAndGuidelines), thus making future maintenance to his code easier, as it is much [easier to read](http://blog.binarymist.net/2009/12/24/keeping-encapsulation-on-ones-mind/). Asks for a pair to [review](http://blog.binarymist.net/2012/03/24/how-to-optimise-your-testing-effort/#pairReview) his code or perhaps requests a fellow team member to sit with him and [pair programme](#process-agile-development-and-practices-pair-programming) for a bit on some complex areas of the code base. Makes sure his code is being run in the [continuous integration](http://blog.binarymist.net/2012/03/24/how-to-optimise-your-testing-effort/#continuousIntegration) suite, that his [acceptance tests](http://www.slideshare.net/kimcarter75098/moving-to-tdd-bdd) (which have been driving his feature) are passing and the [security regression tests](#process-agile-development-and-practices-security-regression-testing) are not regressing. Checks that his work complies with the [Definition of Done](http://blog.binarymist.net/2013/03/02/how-to-increase-software-developer-productivity/#definitionOfDone). You do have a Definition of Done right?

Now what the Product Owner or software development manager often fails to understand is that it is the slow (Professional) developer that is creating code that can be maintained and extended at a sustainable pace. Professional Developer is investing time and effort into creating a better quality of code than the developer (Code Monkey) that appears to be producing code faster. The Product Owner and/or manager do not necessarily see this, in which case Code Monkey clearly looks to be the superior developer right (the rock star)? What also often happens is that Code Monkey rides on Professional Developers quality and adds his lower quality code on top, thus making Code Monkey look like a god.

The Product Owner sees output immediately by Code Monkey that "appears" to be working very fast. He does not see the quality being created by Professional Developer that "appears" to be working slower.

Time goes by. Sprint Review roles around. The stake holders love the new features that have been implemented and now want some additions and refinements. They ask the Product Owner to add some more User Stories into the Product Backlog. The Development Team pull these stories into a Sprint and start work. New functionality is added on top of the code that Professional Developer wrote previously. New functionality is added on top of the code that Code Monkey wrote previously.

Sprint Review roles around again and the stake holders are happy with the new features that have been added on top of Professional Developers code. Of course they have no idea that the underlying code was crated by Professional Developer. Now the stake holders have been using the software that had the new features added on top of Code Monkeys lower quality code and they are starting to notice other areas of the application that are no longer behaving the way they are supposed to. This continues to happen and the stake holders are oblivious to the fact that it is due to the code that Code Monkey is writing. They still think he is a rock star because he appears to pump out code so fast.

So… while Professional Developer seems to be slowing The Team down and clearly Code Monkey is simply amazing because he delivers his features so much faster. The actual truth is exactly the opposite. Professional Developer is creating [SOLID](http://blog.binarymist.net/2012/12/01/moving-to-tdd/#solidPrinciples) code and running at a pace that is sustainable (a key principle of the agile manifesto).

The code that Professional Developer wrote is easier to modify and extend as its design is superior, due to being well thought out and driven by tests. His code satisfies the acceptance tests, so everyone knows it meets the living specification. It is faster to add changes to his code because it is easier to read and there are less surprises. If any other team member changes Professional Developers code which makes it no longer conform to the specification, the acceptance Tests around his code fail, thus providing instant feedback to the developer making the change.

It is the practices (formed by habit) of Professional Developer that:

1. Provide the entire Development Team assurity that the software satisfies the requirements of the specification at all times
2. Allow The Development Team to run at a sustainable pace
3. Provide confidence in ongoing future estimations due to less surprises
4. Produce code that everyone wants to work with
5. Produce less error prone software that does what it says it will do on the box.

#### Scrum Teams can Fail Too

Velocity of the Development Team starts high then declines. Often it is hard for people (including the Product Owner) to pin-point why this is happening. The Scrum Team may have started out delivering at a consistently high cadence. They appeared to be really on fire.

The code base is small but growing fast. As it starts to get larger, the Development Team starts to feel the weight of a lot of code that has been hacked together in a rush. This causes the teams ability to release software fast to wane. A Scrum Team can get to this point quite fast, as they are a high performance team. When you get to this point, almost every change to the code base is hard. Make one change and something else fails (the whac-a-mole effect).

1. Routines are hundreds of lines long. Developers have to understand hundreds of lines of code in order to make a small change.
2. Names are not as meaningful as they should be.
3. Routines have multiple levels of abstraction, so multiple levels of code need to be understood to make a single change.
4. Inheritance is over used/abused, thus creating unnecessarily tight coupling.
5. There are many aspects of the code that have become terrible to work with.

##### How Does This Happen?

How does the Product Owner know that the quality of code being created is not good? The Product Owner is not generally a developer so does not know and even if he is, he is not in the code day in, day out. Also it is not generally the most important concern of the Product Owner, rather getting new functionality out the door is, so this is what the Development Team are rewarded for. When they pass a Sprint, the Product Owner is happy and praises the Development Team. The Product Owner has no idea that the quality of code is not as good as it needs to be to sustain a code base that is easy to extend.

So the Development Team does what ever it needs right now, to make sure they deliver right now (the current Sprint). Quality becomes secondary, because no one is rewarding them for it. This is a lack of professionalism on the Development Teams part. Bear in mind though, that each developer is competing against every other to appear as though they have produced the most. After all, that is what they are being rewarded for. Often what this means is they are working too fast and not thinking enough about what they are doing, thus the quality of the code-base is deteriorating, like in the example above with Code Monkey.

I have personally seen this on close to 100% of all non Scrum projects I have been involved with. Scrum Teams are sometimes better off because they have other practices in place that ensure quality remains high, but these practices are not prescribed by Scrum.

##### So... What do We Do?

We not only reward the Development Team for delivering features fast but we also reward them for the sort of practices that Professional Developer (from our example [above](#process-agile-development-and-practices-essentials-for-creating-and-maintaining-a-high-performance-development-team-how-and-why-many-software-development-shops-fail-the-scenario) performs.

##### How do We Do This

We add the practices that Professional Developer from [above](#process-agile-development-and-practices-essentials-for-creating-and-maintaining-a-high-performance-development-team-how-and-why-many-software-development-shops-fail-the-scenario) performs to our [Definition of Done](http://blog.binarymist.net/2013/03/02/how-to-increase-software-developer-productivity/#definitionOfDone).

The Product Owner runs through the Acceptance Criteria which should be included on every Product Backlog item (preferably during the Sprint) indicating acceptance. Also running through the Definition of Done… querying the Development Team that each point has in fact been done for the Backlog item in question. This of course should be done by the developers themselves first. This provides the Team with confidence that the Sprint Backlog item is actually complete. Essentially, the work is not Done, until all the Acceptance Criteria points and Definition of Done points are checked off. This way the Development Team is being rewarded for delivering fast and also delivering high quality features that do what the stake holders expect them to do. No nasty surprises.

On top of what our solo Professional Developer did [above](#process-agile-development-and-practices-essentials-for-creating-and-maintaining-a-high-performance-development-team-how-and-why-many-software-development-shops-fail-the-scenario), we should:

1. Measure test speed and reward fast running tests
2. Measure cyclomatic Complexity
3. Run static code analysis and use productivity enhancing tools. This is not cheating, it is allowing the developers to work faster and create cleaner code. This can even be set-up as pre-commit hooks etc on source control. Discussed in more detail in the "Consuming Free and Open Source" sections in the Web Applications chapter.
4. Code reviews need to be based on the coding standards and guidelines
5. Encourage developers to commit regularly, thus their code is being run against the entire test suite, providing confidence that their code plays nicely with everyone elses code. Commit frequency can be measured
6. The Development team should shame developers when they break the [CI build](http://blog.binarymist.net/2014/02/22/automating-specification-by-example-for-net). Report on how long builds stay broken for and shame when the duration is longer than an agreed on time.
7. Most of these practices can be added to the Definition of Done, this way Developers can and should be rewarded for doing these activities. Even better, you can automate most of these practices.

### Forming Habits and Sharpening Skills

Something I've learnt as part of my career progression and applied it to multiple areas of my life is starting out as you plan on finishing or progressing. What I mean by that is that the habits you establish early in your career will steer you in the direction of those habits.

There have been a collection of people that I would call mentors that I've taken some advice from and that helped to firm up some of my beliefs in terms of forming habits. Most of these have been from reading books, blog posts and listening to technology podcasts.

I really liked what Moxie Marlinspike said on the topic of [career advice](http://www.thoughtcrime.org/blog/career-advice/). Distilled down: Essentially we become what we do for a job. It's just habits. What ever you do all the time will shape who you become as a person. If you want to become outstanding at something, you need to put the effort into learning everything you possibly can about it and pushing yourself relentlessly. Of course sacrifices must be made somewhere in order to do this and you need to count the costs first.

You've probably heard the saying "No pain, no gain"? What doesn't kill you will make you stronger. Making a habit of constantly pushing yourself will pay off in terms of becoming very good at what ever it is you're doing.

Steer your career by reading the right books. The right books being those written by our software greats. Steve McConnel, Bob Martin and the others that have striven for excellence, and people that understand what goes into making a great crafts person.  
The book: "So Good They Can't Ignore You" by Cal Newport goes through a collection of interviews and analyses of some of the most successful people. The over arching theme is that excellence is a by-product of forming the right habits and pushing yourself relentlessly. Especially in your early years.

Listen to educational technology podcasts when you have down time, travelling etc. Cram that information into your head.

Be ready to admit mistakes and seek out positions where you are the worst developer and you feel uncomfortable. Being around the best will lift your abilities quickly if you're willing to push your self and learn from those that are better than you. I've seen this in many professions. The lower performer if they want to lift their game can do so quickly in a pressured environment like this if they are given the space to.

Forming habits such as  
* TDDing your code
* Making your code as readable as possible. Favouring read time convenience over write time. Your code will be read many times more than it is written. By making it as easy as possible to read (not making your code too clever), you'll weed out many bugs that have slipped through the gaps because developers struggled to understand what your code actually does. Yes you may be able to write something in less lines of code and/or it may be more performant, but at the cost of alienating other developers and harbouring defects.
* Establishing Continuous Integration and Deployment helps to weed out defects early, thus saving big costs.

Exercises such as purposely vulnerable targets (applications, networks, social engineering, etc) with the aim of challenging and lifting your current skill-set are invaluable.
