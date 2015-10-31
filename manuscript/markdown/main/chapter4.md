# Process and Practises {#process-and-practises}

In this chapter I am going to detail a common work-flow of a penetration tester followed with some tried and tested practises to augment your agile development work-flow.

Coming at the security problem from a penetration testers point of view (read team) can be quite different than coming at it from a software developers point of view (blue team (if you are aware)).

* The penetration tester is trying to find all the faults in your system. This is not limited to the technology aspect either, as we address throughout this book.
* As a web developer, you are more focussed on delivering a solution that makes a problem less of a problem. Often not thinking so much about what could go wrong, but more focused on how to make it work.

In saying that, the two disciplines can work in harmony together. There is a real need for improving most software developers security related awareness, skills and knowledge, and as this book is focussed on the web developer, it is my intention to show you as a web developer, how to take the lessons learnt from the penetration testers perspective and apply it to your own work and life. That is right, security needs to be part of who you are. I have the advantage of working in both disciplines and also in some very successful Scrum teams often as Scrum Master. This makes it relatively easy to empathise to both sides and pull the best from both sides, then apply it to the other side. Thus attempting to bring both in to harmony.

## Penetration Testing {#process-and-practises-penetration-testing}

This is the process and the steps commonly taken by a penetration tester.

### Reconnaissance {#process-and-practises-penetration-testing-reconnaissance}

This is the act of information gathering. The quieter you can do this, the less likely you will be to raise suspicions or raise your clients defences.  
Here we want to gather as much information that will be potentially useful for taking into the following stages. Where we start to obtain more information about services & other software being used & their versions. Moving from passive to more active techniques. We need to learn as much as possible about the people involved within, related to and how they are related to the target organisation.  
This way we will be able to create effective attack strategies including non technical aspects such as physical security and pretexts for the people we want to exploit.

#### The Forms it Takes

Information gathering can be done in such a way that the target doesn't know you are doing it (passive) through to where the target should absolutely know you are doing it (active), but all to often the target still does not notice due to insufficient logging, monitoring, alerting (as mentioned in several of the following chapters) and someone actually taking notice as discussed in the People chapter around engagement.

#### Passive

This is when the information gathering can not be detected by the target. Information is gathered indirectly from the target. Usually from sources that have had direct relationship with the target. Social media web sites, 

The pen tester or attacker can not directly probe the target. They must use third party information. Sometimes this will be out of date, so confirmation needs to be sought. This is usually not to difficult, but it takes more time than if the passive constraint was lifted and the pen tester could probe directly.

If you think back to the diagram in the 300,00' view chapter under [Identify Risks](#starting-with-the-30000-foot-view-identify-risks-likelihood-and-impact), where you have "Your Organisation" and the indirect relationships to the "Competitor", you'll see that your competitor or attacker has what is known as these passive or third party relationships with you via your bank, accountants, domain and/or technical consultants, professional services, telcos, ISP and any other number of intermediaries like: social media, whois, DNS (and reverse) lookups and the ocean of information floating around the internet and even the physical cities which you can see if you are observant. Once you have the names of the technology workers at the target organisation, you can search technology specific forums to see what sort of questions they are asking or possibly blog posting on.

#### Semi-Active

Is gathering information directly from the target, but in a manner that looks non suspicious, as if it was any casual browser, passer by or just normal internet traffic. The `nmap -sV` example below almost fits into this form.

#### Active

Here we interact with the target directly, engaging in activities like:

* Snooping physical premises
* Port scanning the entire range `nmap -p- <target>` and some of the aggressive nmap scanning modes shown below
* Spidering public facing resources. Directories and files, often public without the administrators realising it. If they ran a spidering tool against their servers they would see all the publicly accessible resources.
* Banner grabbing and probing

##### Netcat

&nbsp;

No where near as configurable as a dedicated port scanner, but still scans ports is Netcat. Netcat is a network Swiss army knife. It can be used to host a web page, send files, poking at things to see what happens and so many other uses. Lets look at some uses.

{linenos=off, lang=bash}
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

This is an example of using nmap against a target with a SSH daemon and web server running...

As you can see with using the aggressive option `-A` the pen tester makes a lot of noise and if the logs are being casually inspected by a system administrator, the chance of noticing the probes with `nmap` written all over them is very likely.

{title="nmap command", linenos=off, lang=bash}
    # Attempt to detect target OS and services running on it.
    nmap -A <target>

{title="nmap result", linenos=off, lang=bash}
    nmap -A <target>
    
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

{title="target system logs", linenos=off, lang=bash}
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

{title="nmap command", linenos=off, lang=bash}
    nmap -sV --version-intensity 9 <target>

{title="nmap result", linenos=off, lang=bash}
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

{title="target system logs", linenos=off, lang=bash}
    <time> <target> sshd:  refused connect from <kali ip> (<kali ip>)
    <time> <target> <logger instance>:  [info] ::ffff:<kali ip> - - 
       <time> "GET / HTTP/1.0" 200 56328 "-" "-"
    <time> <target> sshd:  refused connect from <kali ip> (<kali ip>)

NMap and the scripting engine are a very powerful tool-set for gathering information. From passive to active. There are many scripts available and they are easy to work out what each is for by using the `--script-help` option.

#### Concealing NMap Source IP Address

An attacker will often attempt to conceal their source IP address during an NMap port scan with the likes of:

##### Decoy host `-D`

&nbsp;

What this does is make it appear to the target that the scans are coming from all the decoy hosts. You can optionally use ME as one of the decoys to represent your/the attackers address. Putting ME in the sixth position or later will will cause some common port scan detectors such as Scanlogd to not show your IP address at all. You can also use `RND` to generate a random non-reserved IP address. For more details check the man page.

You probably want to make sure that the hosts you use as fakes/decoys are actually up otherwise you may `SYN` flood your target(s). There will be no `RST` flag sent to the target being scanned from the decoy thus keeping the connection open. As nmap continues to send more requests to the target with the decoy IP address as the source, the target will maintain a growing list of open connections

A> The `RST` (TCP reset) flag used to be sent indicating that a session be terminated due to problems. In recent years the `RST` flag has been used to terminate sessions instead of `FIN`-> <-`ACK`, <-`FIN` `ACK`-> basically because it is faster and releases stack resources immediately.

With the decoy actually being up and receiving the <-`SYN` `ACK`-> it responds with the TCP reset and the target knows the connecting is finished and releases its resources.

It is also kind of obvious as to which IP address the attack is coming from if the decoy hosts are not actually up.

Useing too many decoys will slow your scan down and possibly make the results less accurate. Some ISPs may filter out your spoofed packets.

{linenos=off, lang=bash}
    # Make sure your decoys are up unless you want to DOS your target.
    nmap -D <decoyip1>,<decoyip2>,<decoyip3>,<decoyip4>,<decoyip5>,ME <target>

##### Idle scan `-sI`

&nbsp;

There are a few things you need to know in order to use this and be able to reason about what is going to happen. This is a side-channel attack which exploits predictable IP fragmentation ID sequence generation on the zombie (fake) host to glean information about the open ports on the target IDS systems will display the scan as coming from the zombie machine you specify (which must be up and meet certain criteria). Check the man page.

{linenos=off, lang=bash}
    nmap -sI 1.1.1.1:1234 <target>

#### Service Fingerprinting

Adding to what we've just seen above, the simplest way to attempt to deduce the details of the service running bound to a particular port is to just see what banner is returned, or for HTTP, the `Server` header field.

##### Depending on the Server field

&nbsp;

{title="Request", linenos=off, lang=bash}
    # Run netcat against your targets web server
    nc <target> 80
    # Now you need to issue the request.
    HEAD / HTTP/1.1
    # You will probably need to hit the enter key twice.

Now if the target is running Apache 2.2.3, then you may see something like the following:

{title="Response", linenos=off, lang=bash}
    HTTP/1.1 400 Bad Request
    Date: Thu, 29 Oct 2015 04:44:09 GMT
    Server: Apache/2.2.3 (CentOS)
    Connection: close
    Content-Type: text/html; charset=iso-8859-1

You can not rely on the Server field though. It could be obfuscated:

{title="Response", linenos=off, lang=bash}
    403 HTTP/1.1 Forbidden
    Date: Thu, 29 Oct 2015 04:44:09 GMT
    Server: Unknown-Webserver/1.0
    Connection: close
    Content-Type: text/html; charset=iso-8859-1

Or if your target is running an Express server, you will probably see something like:

{title="Response", linenos=off, lang=bash}
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

Now if we try some malformed requests:

{title="Request", linenos=off, lang=bash}
    nc <experss 4.0 server> 80
    GET / HTTP/1.1

{title="Response", linenos=off, lang=bash}
    HTTP/1.1 200 OK
    X-Powered-By: Express
    Content-Type: text/html; charset=utf-8
    Content-Length: 56328
    ETag: W/"dc08-0R4KyLlbTHepNS8qdLYCyQ"
    Date: Thu, 29 Oct 2015 05:03:46 GMT
    Connection: keep-alive
    
    # We get the page markup here.

{title="Request", linenos=off, lang=bash}
    nc <experss 4.0 server> 80
    GET / HTTP/1.0

{title="Response", linenos=off, lang=bash}
    HTTP/1.1 200 OK
    X-Powered-By: Express
    Content-Type: text/html; charset=utf-8
    Content-Length: 56328
    ETag: W/"dc08-0R4KyLlbTHepNS8qdLYCyQ"
    Date: Thu, 29 Oct 2015 05:04:20 GMT
    Connection: close
    
    # We get the page markup here.

&nbsp;

{title="Request", linenos=off, lang=bash}
    nc <apache 2.2.3 server> 80
    GET / HTTP/1.1

{title="Response", linenos=off, lang=bash}
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

{title="Request", linenos=off, lang=bash}
    nc <apache 2.2.3 server> 80
    GET / HTTP/1.0

{title="Response", linenos=off, lang=bash}
    HTTP/1.1 200 OK
    Date: Thu, 29 Oct 2015 05:02:03 GMT
    Server: Apache/2.2.3 (CentOS)
    Accept-Ranges: bytes
    Connection: close
    Content-Type: text/html; charset=UTF-8
    
    No Host: header seen.

Interesting isn't it? Every server type answers in a different way.

##### Non-existent protocol

&nbsp;

Now if we use a non-existent protocol:

{title="Request", linenos=off, lang=bash}
    nc <express 4.0 server> 80
    GET / CATSFORDINNER/1.0
    # Express ignores cats for dinner. No response


{title="Request", linenos=off, lang=bash}
    nc <apache 2.2.3 server> 80
    GET / CATSFORDINNER/1.0

Now we see Apache just can not get enough cats for dinner.

{title="Response", linenos=off, lang=bash}
    HTTP/1.1 200 OK
    Date: Thu, 29 Oct 2015 05:12:51 GMT
    Server: Apache/2.2.3 (CentOS)
    Accept-Ranges: bytes
    Connection: close
    Content-Type: text/html; charset=UTF-8
    
    No Host: header seen.

##### Other Services

&nbsp;

Let us look at SSH.

Using nmaps service detection `-sV` option has the smarts to work most of this out because nmap uses service specific probes. So relying on higher level tools are often quicker and more effective as the manual work has already been done and backed into the tooling.

{linenos=off, lang=bash}
    nmap -sV -p 22 <target>

{title="Results", linenos=off, lang=bash}
    Starting Nmap 6.40 ( http://nmap.org ) at 2015-10-29 19:46 NZDT
    Nmap scan report for <target> (<target ip>)
    Host is up (0.00049s latency).
    rDNS record for <target ip>: <target.domain>
    PORT    STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 6.7p1 Debian 3 (protocol 2.0)
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Now if the system administrator had of modified the `/etc/hosts.deny` to be `ALL: ALL` and the `/etc/hosts.allow` to only include the machines intended to access the ssh daemon, then the attacker would see the following instead:

{title="Results", linenos=off, lang=bash}
    Starting Nmap 6.47 ( http://nmap.org ) at 2015-10-29 19:59 NZDT
    Nmap scan report for <target> (<target ip>)
    Host is up (0.0021s latency).
    rDNS record for <target ip>: <target.domain>
    PORT    STATE SERVICE    VERSION
    22/tcp open  tcpwrapped

Not the information an attacker is looking for.

{linenos=off, lang=bash}
    nc -v <target> 22

{title="Results", linenos=off, lang=bash}
    Connection to <target> 22 port [tcp/*] succeeded!
    SSH-2.0-OpenSSH_6.7p1 Debian-3

And again if the system administrator had setup the hosts files like above:

{title="Results", linenos=off, lang=bash}
    DNS fwd/rev mismatch: <target> != <target.domain>
    <target> [<target ip>] 22 (?) open

Again not the information an attacker is looking for. We also discuss risks and countermeasures around SSH in the VPS chapter.

#### Web Application Firewall (WAF) Fingerprinting

The fact that a WAF is in place is often given away by simply inspecting the responses from the server side. a WAF may add a cookie, with a little searching you may discover what the WAF is from its cookie. Likewise with inspecting the HTTP headers, there are often giveaways. A session timing out very quickly. That can be seen when you telnet or netcat to a web application, but don't issue a request quickly enough.

##### NMap

has a couple of good scripts out of the box for WAF detection.

To view all of the currently available local nmap scripts `locate *.nse` will give you the full listing. To narrow down the listing to what we are actually looking for in this case:

`nmap --script-help "http-waf*"`

Will yield the following two scripts which are very useful:

1. `http-waf-detect.nse`  
Attempts to determine whether the web server is protected by a IDS, IPS or WAF
2. `http-waf-fingerprint.nse`  
Attempts to discover the presence of a WAF, its type and version.

##### [WAFW00F](https://github.com/sandrogauci/wafw00f)

is also an excellent tool included in Kali Linux. WAFW00F or `wafw00f` tests for a large number of known WAFs (26 last time I looked). Running is pretty much self explanatory. Just run it against the target host.

"_Sends a normal HTTP request and analyses the response; this identifies a number of WAF solutions_  
_If that is not successful, it sends a number of (potentially malicious) HTTP requests and uses simple logic to deduce which WAF it is_  
_If that is also not successful, it analyses the responses previously returned and uses another simple algorithm to guess if a WAF or security solution is actively responding to our attacks_"

#### DNS

##### Domain Information Groper (dig) 

&nbsp;

To perform DNS lookup:

{title="dig", linenos=off, lang=bash}
    dig <domain you are wanting info on>

To perform a reverse lookup on an IP address:

{title="dig reverse lookup", linenos=off, lang=bash}
    dig -x <ip address>

{title="dig for email servers", linenos=off, lang=bash}
    # mx (email servers) is the type of record to look for.
    # by default, dig will perform a lookup for an A record
    dig <domain you are wanting info on> mx

There are many other options you can use with dig, the output is clear and informative. There is also host and the deprecated nslookup which generally provides less information and uses its own internal libraries as opposed to the OS resolver libraries that dig uses.

##### dnsenum

Will do everything dig can do plus more.
There is no man page for dnsenum. Simply run dnsenum and you will be presented with its help.

In order to find all subdomains associated with the target domain, you can use a wordlist. A good collection can be found in Kali Linux by simply issuing the following command:

{linenos=off, lang=bash}
    locate wordlist

Recon-ng as discussed below also has some in `/usr/share/recon-ng/data/` that it uses for similar tasks. Choose your wordlist then apply it to dnsenum. The results found will only be as good as your wordlist. So choose wisely.  
`/usr/share/dirbuster/wordlists/directories.jbrofuzz` is not great, but it is not bad either

{linenos=off, lang=bash}
    # This is a noisy command, but it is still passive. The target is not being touched.
    # Be patient, it can take some time if you use a large wordlist.
    dnsenum <target domain> -f <your chosen wordlist>
    # dnsenum can also recurse on all the subdomains with the -r option

This will provide the same results as a simple `dnsenum <target domain>` and then start the bruteforce which lists the IP addresses with the domains and record types

{linenos=off, lang=bash}
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

`dnsrecon -d <target domain>`  
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
or  
just [Alt]+[F2] theharvester

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

Both passive & active options available.

#### Discover-scripts

Is an excellent Open Source Intelligence Tool (OSINT).

![](images/discover.png)

This is a collection of shell scripts to aggregate Kali Linux tools & automate various pentesting tasks. Both passive & active options which allow you to dig up a lot of dirt on your target long before you start trying to penetrate them. I have found domain and Person to be very useful.

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

So rather than getting familiar with all the recon tools at once, you get the benefit of many tools in one. You do not get quite the flexibility of using the tools by themselves though, but as time is often the constraining factor, discover is a good choice.

#### recon-ng {#process-and-practises-penetration-testing-reconnaissance-recon-ng}

Another tool in a similar vein to discover-scripts. Recon-ng has a similar feel to metasploit, but for web based reconnaissance. If you are familiar with the metasploit framework, you will notice most of the options are very similar. Recon-ng has a modular framework, allowing anyone that can write some Python to contribute to the collection of modules. This is a very powerful and easy to use recon tool.

Some of the sort of information you may get is:

1. Additional companies
2. Hosts you didn't know existed with their IP addresses, domain names, countries of origin, region, latitude, longitude and the module used to find the information.
3. Contacts, first and last names, email addresses, pgp key associations
4. Vulnerabilities, credentials
5. So many more.

It depends on which moduels you use as to what information you receive.

When you run recon-ng with no arguments, you will be presented with the titles of the collections of the included modules and you'll be dropped at a recon-ng prompt showing the current workspace you are in. It looks like:

{linenos=off, lang=bash}
    [recon-ng][default] > 

Type `help` and you will be presented with the recon-ng commands.

Prepend any of the commands with help or simply type the command by itself if you want to know more about the specific command.

For example type `show` and you will be presented with:

{linenos=off, lang=bash}
    Shows various framework items
    
    Usage: show [banner|companies|contacts|credentials|dashboard|domains|hosts|leaks|locations|modules|netblocks|options|ports|profiles|pushpins|schema|vulnerabilities]

The above outputs are actually table names that contain data you add (discussed soon).

Then type `show modules` and you will be presented with a listing of all the moduels in `/usr/share/recon-ng/modules/`:

{title="recon-ng modules", linenos=off, lang=bash}
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

As recon-ng uses workspaces, you can keep all your specific assignment data in its own workspace. Data is persisted to the file system `/user/.recon-ng/workspaces/<your new workspace name>/` as it's gathered. You can exit and restart recon-ng anytime without loosing the data you have already gathered.

{linenos=off, lang=bash}
    workspaces add <your new workspace name>

Usually before you `use` -> `run` modules, you will want to `add` initial records. The sort of records you may want to add can be seen by typing `show`.

{linenos=off, lang=bash}
    add domains <a target domain>
    add companies
    # Now you are prompted for some details like name and description

To see what records you have already `add`ed, type `show [item]` (where `item` is actually a table name listed from the output of the `show` command). For example: `show domains` or `show companies`

To delete an item you have added: `del [item] [rowid]`, for example:

{linenos=off, lang=bash}
    del domains 1 # To remove the first record 

You can also query the workspace database with the `query` command. Just type it and you will get some help and be running in no time. Many of the commands can be performed using recon-ng commands or by using the `query` command and building up SQL queries.

To use a specific module:

{linenos=off, lang=bash}
    use <one of the modules listed from show modules command>

To find out what options if any you need to set for your chosen module, type:

{linenos=off, lang=bash}
    set
    # This will print the Names of the options which you need to set if not already set and if it is a Required field, Current Value and Description.
    # The Current Value should be the second argument you provide to set.

If you need more information about the current module you have `use`ed, type:

{linenos=off, lang=bash}
    show info

You will be notified when you attempt to `run` if you need an API key. Details on where to find these are on the [recon-ng wiki](https://bitbucket.org/LaNMaSteR53/recon-ng/wiki/Usage%20Guide#!acquiring-api-keys).

Before you add any social media API keys, you will want to create throwaway social media accounts, because many of the social media sites you will perform recon on will show the visiting user. You want that visiting user to be your throwaway account.

{linenos=off, lang=bash}
    # To show your currently installed API keys:
    keys list
    # To add an API key:
    keys add [specific_key_listed_from_previous_command] <yourkey>

Once you are ready to `run` the module you have chosen with the `use` command, just type `run`. All the information will be gathered into the database which you can query or create reports from. Between each new module you `use` and `run` you can view what was found simply by watching the screen or:

{linenos=off, lang=bash}
    query SELECT * FROM [any of the tables listed with the show command]
    # For example:
    query SELECT * FROM contacts
    query SELECT * FROM hosts

or

{linenos=off, lang=bash}
    use reporting/html
    set CREATOR <you or your company>
    set CUSTOMER <your client/target>
    run
    # Now you will be told where your report lives.
    # It should be in the workspace you created which you can access at any time.

The report types you can use can be seen by typing:

{linenos=off, lang=bash}
    show modules reporting
    # These include html, json, csv, xml and others

`exit` to quit out of recon-ng. Any data gathered will be retained.

The commands may seem a bit heavy to start with, but they're very intuitive. Spend some time with recon-ng and it will end up being one of the first recon tools you turn to.

#### Useful New Zealand Web Resources

Finding details on people: [http://searchenginez.com/findpeople_newzealand.html](http://searchenginez.com/findpeople_newzealand.html)

Finding details on companies: [https://www.business.govt.nz/companies/](https://www.business.govt.nz/companies/)

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

##### Password Profiling

&nbsp;

Is where an attacker will start to collate information and often feed it into a tool or specific set of tools, or if they have lots of time which doesn't really happen, perform the same process manually. I find that the tools which have well thought out algorithms are the quickest and best way to create a short list of probable passwords to later attempt to access accounts via brute force attack

I discuss this further in the [People](#people-identify-risks-weak-password-strategies) chapter.

### Vulnerability Scanning / Discovery

_Todo_

Create headings for the best tools.
Complete one of the tool sections so it can be used in workshop.

_Todo_

### Vulnerability Searching {#process-and-practises-penetration-testing-vulnerability-searching}

_Todo_

Discuss how to find exploits from vulnerability scanning step.

_Todo_

The vulnerability advisories were mentioned in the [30,000 View](#vulnerability-advisories) chapter.


_Todo_

### Exploitation

During this stage, thinking about and recording countermeasures for the vulnerabilities you are able to exploit successfully is just as important as finding and recording the exploited vulnerabilities. That is why each of the following chapters contain the Countermeasures sections.

Beat your own systems up and watch logs. Get familiar with signatures of different tools and attacks, then you will know when you are actually under attack. You can take the same concept with active and semi-active reconnaissance. This way, you will be able to pre-empt your attackers exploitation.

We will go through the actual exploitation that an attacker or penetration tester would carry out in the following chapters Identify Risks sections.

_Todo_

Discuss isolating (potential) malware:  
firejail  
https://threatpost.com/firefox-37-to-include-new-onecrl-certificate-blocklist/111411/  
http://sourceforge.net/projects/firejail/  
http://linuxmint.tumblr.com/post/128281378277/firejail

linux containers


docker


What else?

### Documenting and Reporting

Just because this section is last in the Penetration Testing section, does not mean it is carried out last. A penetration tester or even an attacker will be recording information throughout the entire engagement. Especially during the Reconnaissance stage.

#### Dradis

%% "The Social Engineer's Playbook" pg 81

_Todo_

Discuss what Dradis provides and how it helps.

## Agile Development and Practices

A> Taking Back the Security Focus... but why?  
A> Because as technologists we are hired to create business value and reduce business costs. If you look at the [public statistics](http://www.meetup.com/OWASP-New-Zealand-Chapter-Christchurch/events/223462991/) on businesses loosing value due to being compromised regularly, the figures are staggering and they are growing exponentially due to the popularity of cyber-crime. That's why we care.



If I can encourage even one developer within each team to lead the charge on taking back the responsibility of creating solutions that will withstand the types of attacks that are being launched against our physical locations, people, infrastructure, products and way of life on a regular basis today, I think we may be able to move forward and make our industry a better place, a place we want to be part of. 

Primarily this process is about significantly reducing the cost of producing quality products.

The 10,000' View should be carried out each Sprint for each PBI as it is pulled into WIP. If the particular PBI has something of Physical, IoT, Mobile, People, Cloud, VPS, Network, Web Applications attributes, then you can apply the concepts and direction discussed in these chapters to the PBI you are working on.

It is not my intention to encourage you to think about nothing but security (in this book at least), because then you would not deliver a product, but rather for it to become part of who you are, so as part of your normal work-flow the security consciousness and habits you build will be firmly entrenched. Thus your deliverables will be no longer at the bottom of the tree where your attackers will pick from first.


_Todo_

Create hypothetical process that augments an agile workflow. Many of these concepts will come from the Web Applications chapter with pieces from the other chapters. Lean heavily on my own experience here of what has and has not worked well in the past and where I see things going.

&nbsp;





A> Let us look at some of the practices we can use as developers to lift the level of security within our software and how we can integrate them into our Scrum process framework.

### Architecture

There are no architects in the Scrum Team. Just Developers. Some of those developers have architect qualities. They like to step back often to see the bigger picture and understand the interactions between the components and the people using the software. Including every aspect to do with the end product, the people intending to use it and the risks.

Agile architecture does a little design up front in collaboration with the team and everyone that has a vested interest. Agile architecture is not siloed, because it is part of development, just as analysing requirements and testing. It is all done in parallel.

So, we do not really separate the discipline of architecture from a software developer that excels at doing what a traditional architect does as well as engineering. An architect is essentially an extreme version of the 'T' shaped developer. They have very broad knowledge and skill bases, with multiple deep specialities.  
Another defining factor that sets the architect apart is their ability to translate effectively between the most technical people on a project and the least technical. I like to think of them as the people that spend a lot of time riding the elevator of a high rise building. Most technical people toward the bottom of the building and least technical people at the top. The architect spends time on each level taking what he/she has learnt from all the other levels, then translating and applying it to the specific level.

&nbsp;

### Security Test-Driven Development (STDD) {#process-agile-development-and-practices-security-test-driven-development}

![](images/red_green_refactor.jpg)

There are three aspects I would like to focus on here. You can simply continue to use your existing automated test suites and frameworks. All you have to do is:

1. Add a **security focused test API** into the mix of your existing automated acceptance test suites.  
 Your chosen (language specific) BDD framework of choice, putting legs to your test conditions with automatable "[Given, When, Thens](http://blog.binarymist.net/2012/03/24/how-to-optimise-your-testing-effort/#planningTheTestEffort)".
2. If you are using NodeJS and have not already got your tests running as part of a CI build or pre-commit hook, check out the Consuming Free and Open Source [Tooling](#web-applications-countermeasures-consuming-free-and-open-source-tooling) section for some info on how to set this up
3. Add **security focused BDD/TDD/ATDD** tests.
 This is the same amount of work as any other automated TDD, but it has the huge benefit of bringing the finding of security faults from where it is very expensive to fix:  

    ![](images/CostOfChange.png)  

    to where it is the cheapest possible place to fix:

    ![](images/CostOfChange-WithSTDD.png)  

&nbsp;

1. Continuing No. 1 from above: [OWASP ZAP](https://www.owasp.org/index.php/OWASP_Zed_Attack_Proxy_Project) (which also comes [pre-installed on Kali Linux](http://blog.binarymist.net/2014/03/29/up-and-running-with-kali-linux-and-friends/#zap) ) is a particularly useful tool for SBDD and regression testing. Because it not only provides a manual tool similar to the likes of Burp Suite + with many other features. ZAP also has the ability to run as a HTTP proxy:
    1. You can run ZAP manually then through the menu Tools -> Options... -> API turn the HTTP API on
    2. Run ZAP from the command line using the -daemon flag
    
        {linenos=off}
            owasp-zap -daemon

    You can then access the API like this:
    
    {linenos=off,lang=bash}
       curl http://localhost:8080 # Providing ZAP is listening on port 8080

  
    This allows us to within our Behavioural, Acceptance tests, send requests programmatically directly to the ZAP HTTP API to do what ever we could do manually with the tool against the System Under Test (SUT).
 You can of course use Selenium 2 (WebDriver) also to drive browser tests and in parallel. I discussed this in ["Automating Specification by Example"](http://blog.binarymist.net/2014/02/22/automating-specification-by-example-for-net/#scope).

    The ZAP API can be accessed directly or by any of the following client implementations:

    * Node.JS (by way of [zaproxy](https://www.npmjs.com/package/zaproxy))
    * Python
    * PHP
    * Ruby
    * .Net [write-up](http://www.codeproject.com/Articles/708129/Automated-penetration-testing-in-the-Microsoft-sta), [source](https://github.com/gustavorhm/ZapPenTester). It is easy to see how the API is started and used [here](https://github.com/gustavorhm/ZapPenTester/blob/master/ZAPPenTester/Zap.cs).  
 There is also the [OWASP Secure TDD Project](https://www.owasp.org/index.php/OWASP_Secure_TDD_Project). A .Net solution. This project appears to either be abandoned or just very low activity. Feel free to offer to help though if you are a .Net developer. I am not sure I agree with one of the opening statements: "they need to cover all tests prior development". The approach that I would take would be to write some specification (test), execute it, (red) -> Write the smallest amount of code possible to make it pass (green) -> Add to the specification (test)(refactor). As you can see that is your red->green->refactor loop, with the smallest amount possible for each iteration.
    * Java. A couple of client projects useful for seeing how to use the ZAP API: [zap-webdriver](https://github.com/continuumsecurity/zap-webdriver), [bdd-security](https://github.com/continuumsecurity/bdd-security)

    For getting started with OWASP ZAPs API check the:

    * [regression testing](https://code.google.com/p/zaproxy/wiki/SecRegTests)
    * [API details](https://code.google.com/p/zaproxy/wiki/ApiDetails)

2. Continuing No. 2 from above: This is adding another aspect to your existing TDD/BDD thought process. Instead of the business waiting until go-live before contracting the experts to beat up your system. Then tell you **your security sucks**. We take a proactive approach and move a lot of the effort traditionally performed at go-live **up front**, where you yourself as the developer can test and fix. Thus saving embarrassment and the business a lot of money.  
BSIMM has some good [guidance on security testing](https://www.bsimm.com/online/ssdl/st/)

What is so good about STDD and SBDD, is that it roles up Specifications, Design, Implementation and Verification all into one process. Thus working toward delivering each increment that is truly ["Done"](http://blog.binarymist.net/2013/03/02/how-to-increase-software-developer-productivity/#definitionOfDone). Driving development with tests is not about testing right? It is about creating code that is testable. Testable code is inherently well designed and gives us the ability to reason about the state at any given time.  
OpenSSL Heartbleed and Apples Goto Fail could have been prevented if (S)TDD was used. Check out Mike Bland's [excellent study and POC](http://martinfowler.com/articles/testing-culture.html).

<!--- Other Resources: http://www.continuumsecurity.net/services.html#testing -->

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

Remember I mentioned in the People chapter in the ["Top Developer Motivators in Order"](#people-countermeasures-morale-productivity-and-engagement-killers-top-developer-motivators-in-order) section how developers love being champion of something? The role of security champion or any champion for that matter needs to be applied to the developer as a vacuum. People do not respond well to being pushed into anything. The best developers will just pick up the role, but many will not be as proactive. For the less proactive developers, it pays to create the role and as a Product Owner or manager approach them and ask them if they would like to take the responsibility as security champion. Once a developer has taken up this responsibility, they will usually do a pretty good job of infecting the rest of their team with their passion and knowledge.

### Pair Programming

Two pairs of eyes on the same code is proven to drastically reduce defects, and it does it at the cheapest possible place, as they are being created. Pair programming can be a very effective discipline, but not all the time and not for all people. There is a lot of resources on the topic. If you have not tried it, then you should, but it is probably counter productive to mandate that all the developers should do it all of the time. It is probably a good idea to encourage developers to pair on complex tasks. It is a tool, try it and use it wisely.

&nbsp;

### Code Review

If we can not get the simple things right like [Coding standards and conventions](http://blog.binarymist.net/2012/12/19/javascript-coding-standards-and-guidelines/) to help remove some of the "wild west" attitudes and behaviours, then how will we ever get the complicated things right? The whole team needs to abide by the standards, conventions and guidelines.

[Callback Hell Alternatives](http://blog.binarymist.net/2014/07/26/node-js-asynchronicity-and-callback-nesting/) for example.

Manual and Automated.

Check out the [BSIMM Code Review](https://www.bsimm.com/online/ssdl/cr/) for some good ideas.

#### Why? {#process-agile-development-and-practices-code-review-why}

Because when humans are in creative mode, we often struggle to see the defects in our own creation. That is why tightly knit teams with high morale (as discussed in the people chapter under the "Morale, Productivity and Engagement Killers" sections) are a force to be reckoned with. We are all watching each others backs. We are good at seeing faults in others and their creations. That is why we really do need each other. Never underestimate this blindness in us all and that we have a very powerful mitigation tool in the team. Use it.

#### Linting, Static Analysis

Start with the likes of JSHint. There are lots of other good tooling options.

Techniques and tools to assist with automating

* [https://wiki.mozilla.org/Security/B2G/JavaScript_code_analysis](https://wiki.mozilla.org/Security/B2G/JavaScript_code_analysis)
* [JSPrime](https://www.youtube.com/watch?v=Vk5SPGpqiLc) Security focused

#### Dynamic Analysis

Tooling is still immature here. We have got a way to go, but lets start getting our feet wet.

* [Titanium Code Processor](https://theoreticalideations.com/tag/titanium-code-processor/)
* [Slide Deck](https://speakerdeck.com/ariya/dynamic-code-analysis-for-javascript)
* [Jalangi](https://www.eecs.berkeley.edu/~gongliang13/jalangi_ff/)

&nbsp;

### Techniques for Asserting Discipline

JavaScript is an inherently flexible and undisciplined language. This quality is a double edged sword. It provides us with extreme power and also allows us to slaughter ourselves. Discipline is very much needed in order to stay safe and be able to reason about our applications as they grow larger.

I have been involved in many JavaScript project rescue missions where a development team can no longer understand what the monster they have created is doing, what state it is in and why? In almost all occasions, the developers on the team do not have a deep understanding of the language and often of any language. Because of this I always try and push the fact that JavaScript developers must do everything they can to understand the language. In most cases this means taking their learning home and getting intimate with JavaScripts beauty.

Because of the distinct lack of discipline within JavaScript (unlike most other languages) I try and bring as much understanding around techniques that can help impose the extra rigour where and when it is needed. The beautiful thing about JavaScript is that when you really need to do something unconventional, you can, but I like to **weigh up the trade-offs** of either approach.

#### Static Type Checking

In JavaScript we need as much help as we can to fail fast. Static type checking gives us this. It also feels like the step before DbC.

* [flow](http://flowtype.org/) looks to be a good option. Providing consumers with the option of introducing type checking progressively and/or to certain parts that make the most sense. Or rather missing parts that require the extra flexibility.


#### Design by Contract (DbC)

Enforcing preconditions, postconditions and invariants in our routines.
That is right, we can even employ this design principle in JavaScript. I wrote about DbC in a [previous post](http://blog.binarymist.net/2010/10/11/lsp-dbc-and-nets-support/#dbc) in regards to usage in .Net.  
In JavaScript, I believe the DbC principle is even more important as part of adding discipline and keeping us on the straight and narrow. I believe DbC is the principle that helps us achieve the [Liskov Substitution Principle](http://blog.binarymist.net/2010/10/11/lsp-dbc-and-nets-support/#lsp), which is the 'L' in the SOLID design mnemonic. These are the offerings I have noticed that provide support:

1. [ristretto-js](https://code.google.com/p/ristretto-js/w/list)
2. [contract-js on NPM](https://www.npmjs.com/package/contracts-js)
  * [contract.js at home](http://www.contractsjs.org/)
3. [contractual on NPM](https://www.npmjs.com/package/contractual)

In many cases you can implement your cross cutting code contracts using AOP. This gets it out of your code, so that it is not in your face.

### Forming Habits

_Todo_

%% Start out the way you want to end.



### Sharpen Your Skills

_Todo_

%% Look at the likes of NodeGoat and other vulnerable apps.
