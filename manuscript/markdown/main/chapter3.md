# Tooling Setup {#tooling-setup}

All the software tools I use for penetration and security testing are free and most are also open source (other than Windows of course). I always try hard to stick to this, as it makes:

1. Pitching tooling acquisition to managers and those that hold the purse strings easy
2. Learning how the tools work and contributing to the security ecosystem

## Kali Linux {#tooling-setup-kali-linux}

In the words of Offensive Security (Creators of Kali Linux), [Kali Linux](http://docs.kali.org/introduction/what-is-kali-linux) is an advanced Penetration Testing and Security Auditing Linux distribution. For those that are familiar with BackTrack, basically Kali is a new creation based on Debian rather than Ubuntu, with significant improvements over BackTrack.

Offensive Security Kali Linux is free and always will be. It’s also completely open (as it’s based on debian) to modification of it’s OS or programmes.  
FHS compliant. That means the file system complies to the Linux File-system Hierarchy Standard.  
Wireless device support is vast. Including USB devices.  
Forensics Mode. As with BackTrack 5, the Kali ISO also has an option to boot into the forensic mode. No drives are written to (including swap). No drives will be auto mounted upon insertion.

Some of the following recommendations have come from my [blog post](http://blog.binarymist.net/2014/03/29/up-and-running-with-kali-linux-and-friends/) that was originally used for a PenTest Magazine article.

When it comes to actually getting Kali on some hardware, there is a multitude of options available.

All externally listening services by default are disabled, but very easy to turn on if/when required. The idea being to reduce chances of detecting the presence of Kali.

Kali also provides a simple way to create your own ISO image from the latest source. You can include the packages you want and exclude the ones you don’t. You can customise the kernel. The options are virtually limitless.

The default desktop environment is Gnome, but Kali also provides an easy way to configure which desktop environment you use before building your custom ISO image.

The alternative options provided are: KDE, LXDE, XFCE, I3WM and MATE.

Kali has really embraced the Debian ethos of being able to be run on pretty well any hardware with extreme flexibility. This is great to see.

### What's Included in Kali Linux {#tooling-setup-kali-linux-whats-included-in-kali-linux}

More than 300 security programmes packaged with the operating system. Before installation you can view the tools included in the [Kali repository](http://git.kali.org/gitweb/), or once installed by issuing the following command:

{linenos=off}
    # prints complete list of installed packages.
    dpkg --get-selections | less

To find out a little more about the application:

`dpkg-query -l '*<some text you think may exist in the package name>*'`

Or if you know the package name you're after:

`dpkg -l [package name]`

Want more info still?

`man [package name]`

### Kali Linux Install {#tooling-setup-kali-linux-kali-linux-install}

Offensive Security (the creators of Kali Linux) provide many options for running their distribution. ISO, turn-key VMware image, [custom ARM images](https://www.offensive-security.com/kali-linux-vmware-arm-image-download/), Mobile images ([NetHunter](https://www.kali.org/kali-linux-nethunter/)).
I've used the ISO installed on a VM guest in this setup, as I had a capable host for running this VM and the Windows VM that I setup below. I didn't want the pre-generated SSH host key, which is why I chose the ISO over the turn-key image. If you install on physical hardware, you can just skip this section and move to the "[Tools I Use](#tooling-setup-kali-linux-tools-i-use-that-need-adding-to-kali-linux)" section.

The Offensive Security team which can be found on [IRC](http://docs.kali.org/community/kali-linux-irc-channel) are also very helpful and quick to respond to package requests, etc.

You should find most of what you need [here](http://docs.kali.org/category/installation). Just follow the links specific to your requirements.

%% 2015-05-19 Setup Kali 1.1.0a-amd64

The VMware and VirtualBox images were [here](https://www.offensive-security.com/kali-linux-vmware-arm-image-download/) if you want to go the turn-key way.

For this install I created a new VirtualBox VM based on Debian 64.  
Made a fixed disk of size 40GB and 4GB RAM. More RAM is helpful sometimes, but in most cases I've found 4GB to be enough.

The ISO can be downloaded from [here](https://www.kali.org/downloads/) via torrent and direct download. A little over 3GB.

you’re going to want to check the validity of the ISO (or other image if that's your preference) by verifying the SHA1 checksums. Now this is where the [instructions](http://docs.kali.org/introduction/download-official-kali-linux-images) can be a little confusing. You’ll need to make sure that the SHA1SUMS file that contains the specific checksum you’re going to use to verify the checksum of the image you downloaded, is in fact the authentic SHA1SUMS file. Instructions say on the Kali [downloads](https://www.kali.org/downloads/) page: “When you download an image, be sure to download the SHA1SUMS and SHA1SUMS.gpg files that are next to the downloaded image (i.e. in the same directory on the server).”. You’ve got to read between the lines a bit here. A little further down the page has the key to where these files are. It’s buried in a wget command. Plus you have to add another directory to find them. The location was [here](http://archive.kali.org/kali-images/). Now that you’ve got these two files downloaded in the same directory, verify the SHA1SUMS.gpg signature as follows:

{linenos=off}
    gpg --verify SHA1SUMS.gpg SHA1SUMS

Output:

{linenos=off}
    gpg: Signature made Thu 25 Jul 2013 08:05:16 NZST using RSA key ID 7D8D0BF6
    gpg: Good signature from "Kali Linux Repository <devel@kali.org>

If you get a warning about the key not being certified with a trusted signature, it's because you still need to import the kali key.

I imported the kali gpg key and did the checksums. Details [here](http://docs.kali.org/introduction/download-official-kali-linux-images). If you need any extra help with gpg, check out my [blog post](http://blog.binarymist.net/2015/01/31/gnupg-key-pair-with-sub-keys/) on the sugject.

Now verify the checksum of the image you downloaded with the checksum within the (authentic) SHA1SUMS file.

Compare the output of the following two commands. They should be the same.

{linenos=off}
    # Calculate the checksum of your downloaded image file.
    sha1sum [name of your downloaded image file]

    # Print the checksum from the SHA1SUMS file for your specific downloaded image file name.
    grep [name of your downloaded image file] SHA1SUMS

You can refer to [the hard-disk install](http://docs.kali.org/installation/kali-linux-hard-disk-install) for running through the OS installer if you need it.

Once installed, run:

{linenos=off}
    apt-get update
    apt-get dist-upgrade

Install Guest Additions.

As with BackTrack, the default user is “root” without the quotes. If your installing, make sure you use a decent password. Not a dictionary word or similar. It’s generally a good idea to use a mix of upper case, lower case characters, numbers and special characters and of a decent length. At the terminal, enter: `passwd` and follow the prompts.

### Tools I Use in Kali Linux requiring config, etc {#tooling-setup-kali-linux-tools-i-use-in-kali-linux-requiring-config-etc}

#### Metasploit {#tooling-setup-kali-linux-tools-i-use-in-kali-linux-config-etc-metasploit}

1. To start Metasploit, often I like to see which ports are open first:  
`ss -ant`
2. Then start postgresql if you require [database support](#additional-resources-using-the-database-and-workspaces-in-metasploit). Which is generally quite useful:  
`service postgresql start`  
Followed by:  
`ss -ant`  
if your interested in which ports are being opened.
3. Start the Metasploit service:  
`service metasploit start`  
4. Start the Metasploit Framework Console:  
`msfconsole`  
or possibly with a resource script:  
`msfconsole -r <your custom resource>.rc`

##### Useful metasploit [commands](https://www.offensive-security.com/metasploit-unleashed/msfconsole-commands/)

* `msf >` [help](https://www.offensive-security.com/metasploit-unleashed/msfconsole-commands/#help)
* `msf >` [show](https://www.offensive-security.com/metasploit-unleashed/msfconsole-commands/#show)
  * Valid options to add to show are: `all`, `encoders`, `nops`, `exploits`, `payloads`, `auxiliary`, `plugins`, `options`  
  * Additional module specific parameters are: `missing`, `advanced`, `evasion`, `targets`, `actions`  
* `msf > show options`
* `msf > info <module name>` [info](https://www.offensive-security.com/metasploit-unleashed/msfconsole-commands/#info)

##### metasploit meterpreter client commands

* [Meterpreter Client](https://en.wikibooks.org/wiki/Metasploit/MeterpreterClient)
* [Meterpreter Basics](https://www.offensive-security.com/metasploit-unleashed/meterpreter-basics/)

##### Using the database and workspaces in metasploit {#additional-resources-using-the-database-and-workspaces-in-metasploit}

* [Using databases](https://www.offensive-security.com/metasploit-unleashed/using-databases/)
* [Information gathering](http://resources.infosecinstitute.com/information-gathering-using-metasploit/)

#### BeEF {#tooling-setup-kali-linux-tools-i-use-in-kali-linux-config-etc-beef}

Check-out the recommended [configuration](https://github.com/beefproject/beef/wiki/Configuration).

Modify the following two files as required:

1. `/etc/beef-xss/config.yaml`
2. `/usr/share/beef-xss/extensions/metasploit/config.yaml`

If you need Metasploit integration in BeEF (in most cases you'll want this), set:  
`extension: metasploit: enable: true`  
in the `/etc/beef-xss/config.yaml` file.  
Also make sure:  
`enable`  
is set to `true` in `/usr/share/beef-xss/extensions/metasploit/config.yaml`

[Start Metasploit](#tooling-setup-kali-linux-tools-i-use-in-kali-linux-config-etc-metasploit).

When running Metasploit for BeEF, I often provide `msfconsole` with a Metasploit resource file specifically for BeEF (I call this `beef.rc` and put it in `~/`). This resource file will have the following at least in it:  
`load msgrpc ServerHost=127.0.0.1 Pass=abc123`  
Then once the Metasploit service is started, I'd start `msfconsole` like:  
`msfconsole -r beef.rc`  
If not using the resource file, then once `msfconsole` was running, in order to enable RPC communication for BeEF, I'd enter:  
`load msgrpc ServerHost=127.0.0.1 Pass=abc123`

Finally... starting BeEF. There are three ways. I find the first to be the most informative and interactive.

* Contrary to a blog post on the [beefproject](http://blog.beefproject.com/2014/06/kali-formerly-backtrack-linux-beef.html), I've found the most useful way to run BeEF to be from the `/usr/share/beef-xss/` directory  
`./beef`  
This way provides the most feedback. If the main `config.yaml` (That resides in `/etc/beef-xss/config.yaml`) contains:  
`extension: console: shell: enable: true`  
then we also get an interactive [console](https://github.com/beefproject/beef/wiki/BeEF-Console).
* The second way, running from the command line:  
`service beef-xss start`  
gives no feedback other than a return.
* The third way, running BeEF from the Kali menu:  
Menu: Exploitation Tools -> BeEF XSS Framework -> beef  
doesn't provide a lot of feedback as to what's loaded.

### Tools I Use That Need Adding to Kali Linux {#tooling-setup-kali-linux-tools-i-use-that-need-adding-to-kali-linux}

I now took a backup in case I needed to revert. With VirtualBox it's very easy to take a snap-shot that can be reverted to at any time. Snap-shots are excellent for returning to a known state between penetration tests. Testing is not really testing at all unless you can reproduce the same results each test. Starting from a known state is essential for this.

A> #### Adding Shortcuts to your Panel
A>
A> [Alt]+[right click]->[Add to Panel…]
A>
A> If your Kali install is on VirtualBox:
A>
A> [Windows]+[Alt]+[right click]->[Add to Panel…]

#### Terminator {#tooling-setup-kali-linux-tools-i-use-that-need-adding-to-kali-linux-terminator}

As I spend most of my life in a terminal, I want a good one. I've found Terminator does everything I need from a terminal. Briefly discussed [here](http://blog.binarymist.net/2013/01/19/a-decent-console-for-windows/) in my blog post. If you also like a decent terminal experience, then: `apt-get install terminator`

#### Discover Scripts {#tooling-setup-kali-linux-tools-i-use-that-need-adding-to-kali-linux-discover-scripts}

A set of shell scripts tied together in a CUI to aggregate Kali Linux information gathering tools & automate various pentesting tasks. Both passive and active options allowing you to dig up a lot of dirt on your target long before you start trying to penetrate them. So rather than getting familiar with all the recon tools, you can just get familiar with Discover Scripts.

1. `cd /opt/`
2. `git clone http://github.com/leebaird/discover.git`
3. `cd discover`
4. `./setup.sh`

#### SmbExec {#tooling-setup-kali-linux-tools-i-use-that-need-adding-to-kali-linux-smbexec}

To install:

1. 
    {line-numbers=off}
       cd /opt/
2. 
    {line-numbers=off}
       git clone https://github.com/pentestgeek/smbexec/smbexec.git
3. 
    {line-numbers=off}
       cd smbexec
4. 
    {line-numbers=off}
       ./install.sh
5. Choose number 1
6. 
    {line-numbers=off}
       cd /opt/smbexec/progs
7. 
    {line-numbers=off}
       ln -s /usr/bin/pth-winexe smbwinexe

You may also need to:

{linenos=off}
    ln -s /usr/bin/pth-smbclient smbexeclient
    # For me the smbexeclient already existed,
    # so I only needed to create the single symbolic link.

#### Veil Framework {#tooling-setup-kali-linux-tools-i-use-that-need-adding-to-kali-linux-veil-framework}

I decided to clone the Veil-Framework, as it has a good collection of very useful tools. Veil-Evasion is specifically useful for Anti Virus Evasion. The homepage is [here](https://www.veil-framework.com/).  
The Veil super project which has an install script to install all of the projects can be found [here](https://github.com/Veil-Framework/Veil).

There are install guides [here](https://www.veil-framework.com/guidesvideos/)

To install:

1. 
    {line-numbers=off}
       cd /opt/
2. 
    {line-numbers=off}
       git clone https://github.com/Veil-Framework/Veil.git
3. 
    {line-numbers=off}
       cd Veil
4. 
    {line-numbers=off} 
       ./Install.sh -c

    The dependency installs take a while. Python on Windows, Ruby and Go.
5. Select the defaults for the Python for Windows install.
6. When the install reaches the "Select Destination Directory", go with the default.
7. Click the "Yes" when asked if you want to overwrite the existing Python files.
8. Select the defaults for the rest of the Python install.
9. Next up is the pywin32 setup. Click "Next" to continue.
10. Leaving the default Phthon directory location -> click "Next" -> "Next" -> "Finish"
11. Next up is pycrypto. "Next" -> "Next" -> "Next" -> "Finish"

#### Password Lists {#tooling-setup-kali-linux-tools-i-use-that-need-adding-to-kali-linux-password-lists}

1. `mkdir ~/Desktop/password_list`
2. Download password lists:
  * [Rockyou](http://downloads.skullsecurity.org/passwords/rockyou.txt.bz2)
  * Web Search for "[crackstation-human-only.txt.gz](https://crackstation.net/buy-crackstation-wordlist-password-cracking-dictionary.htm)"
  * [m3g9tr0n_Passwords_WordList_CLEANED](http://blog.thireus.com/cracking-story-how-i-cracked-over-122-million-sha1-and-md5-hashed-passwords)
  * Take your pick of:
    1. https://downloads.skullsecurity.org/passwords/
    2. http://download.g0tmi1k.com/wordlists/large/
3. Decompress any that need it.

Also if you: `locate wordlists` you will find many already exist in Kali Linux.

#### Common User Passwords Profiler (cupp) {#tooling-setup-kali-linux-tools-i-use-that-need-adding-to-kali-linux-cupp}

Easy to use password profiling tool.

1. `cd /opt/`
2. `git clone https://github.com/Mebus/cupp.git`

#### Peepingtom {#tooling-setup-kali-linux-tools-i-use-that-need-adding-to-kali-linux-peepingtom}

1. `cd /opt/`
2. `wget http://thehackerplaybook.com/Download/peepingtom.zip`
3. `unzip peepingtom.zip`
4. `cd peepingtom`
5. `chmod +x *`

#### NMap Script {#tooling-setup-kali-linux-tools-i-use-that-need-adding-to-kali-linux-nmap-script}

Fast scanning and Smart identification.

https://github.com/hdm/scan-tools/blob/master/nse/banner-plus.nse

1. `cd /usr/share/nmap/scripts`
2. `wget https://raw.githubusercontent.com/hdm/scan-tools/master/nse/banner-plus.nse`

#### PowerSploit {#tooling-setup-kali-linux-tools-i-use-that-need-adding-to-kali-linux-powersploit}

PowerShell scripts for post exploitation.

1. `cd /opt/`
2. `git clone http://github.com/mattifestation/PowerSploit.git`
3. `cd PowerSploit`
4. `wget http://raw.github.com/obscuresec/random/master/StartListener.py`
5. `wget http://raw.github.com/darkoperator/powershell_scripts/master/ps_encoder.py`

#### BypassUAC {#tooling-setup-kali-linux-tools-i-use-that-need-adding-to-kali-linux-bypassuac}

Used to bypass UAC at post exploitation, allowing us to get the system account.

1. `cd /opt/`
2. `wget http://www.secmaniac.com/files/bypassuac.zip`
3. `unzip bypassuac.zip`
4. `cp bypassuac/bypassuac.rb /opt/metasploit/apps/pro/msf3/scripts/meterpreter/`
5. `mv bypassuac/uac/ /opt/metasploit/apps/pro/msf3/data/exploits/`

#### OWASP SecLists {#tooling-setup-kali-linux-tools-i-use-that-need-adding-to-kali-linux-owasp-seclists}

Collection of multiple types of lists used during security assessments.

1. `cd /opt/`
2. `git clone http://github.com/danielmiessler/SecLists.git`

#### Chromium {#tooling-setup-kali-linux-tools-i-use-that-need-adding-to-kali-linux-chromium}

As a web developer I use the chromium dev tools more frequently than FireFox dev tools, although both have their strengths.  
In order to run this under the root account, you’ll need to add the following parameter to `/etc/chromium/default` between the quotes for `CHROMIUM_FLAGS=””`

`--user-data-dir`

#### Chromium Extensions {#tooling-setup-kali-linux-tools-i-use-that-need-adding-to-kali-linux-chromium-extensions}

##### FoxyProxy Standard {#tooling-setup-kali-linux-tools-i-use-that-need-adding-to-kali-linux-chromium-extensions-foxyproxy-standard}

Really reduces friction with web proxy interception. FoxyProxy is a very handy add-on for both Chromium and FireFox. Although it seems to have more options for FireFox, or at least they are more easily accessible. It allows you to set-up a list of proxies and then switch between them as you need. When I run Chromium as a non root user I can’t change the proxy settings once the browser is running. I have to run the following command in order to set the proxy to my intermediary before run time like this:

`chromium-browser --temp-profile –proxy-server=localhost:3001`

Firefox is a little easier, but neither browsers allow you to build up lists of proxies and then switch them in mid flight. FoxyProxy provides a menu button, so with two clicks you can disable the add-on completely to revert to your previous settings, or select any or your predefined proxies. This is a real time saver.

##### Cookies

##### EditThisCookie

##### ScriptSafe

Because I like to know where my JavaScript is coming from.

##### SessionBuddy

For storage of browser sessions and easy hydration.

##### User Agent Switcher for Chrome

##### Web Developer

Because I'm a web developer and it has some really useful tools that provide visibility and insight.

#### IceWeasel (FireFox with different Licensing) add-ons {#tooling-setup-kali-linux-tools-i-use-that-need-adding-to-kali-linux-iceweasel-add-ons}

##### FoxyProxy Standard

As for [Chromium](#tooling-setup-kali-linux-tools-i-use-that-need-adding-to-kali-linux-chromium-extensions-foxyproxy-standard)

##### NoScript

Because I like to know where my JavaScript is coming from.

##### Web Developer

Because I'm a web developer and it has some really useful tools that provide visibility and insight.

##### HackBar

Somewhat useful for (en/de)coding (Base64, Hex, MD5, SHA-(1/256), etc), manipulating and splitting URLs

##### Advanced Cookie Manager

##### NoScript

Because I like to know where my JavaScript is coming from.

##### SQL Inject Me

Simple and often useful for running a quick vulnerability assessment. Open source software (GPLv3) from Security Compass Labs. SQL Inject Me is a component of the Exploit-Me suite. Allowing you to test all or any number of input fields on all or any of a pages forms. You just fill in the fields with valid data, then test with all the tools attacks or with the top that you’ve defined in the options menu. It then looks for database errors which are rendered into the returned HTML as a result of sending escape strings, so doesn’t cater for blind injection. You can also add remove escape strings and resulting error strings that SQL Inject Me should look for on response. The order in which each escape string can be tried can also be changed. All you need to know can be found [here](http://labs.securitycompass.com/exploit-me/sql-inject-me/sql-inject-me-faq/).

##### XSS Me

Simple and often useful for running a quick vulnerability assessment. Open source software (GPLv3) from Security Compass Labs. XSS Me is also a component of the Exploit-Me suite. This tool’s behaviour is very similar to SQL Inject Me (follows the Principle of Least Astonishment (POLA)) which makes using the tools very easy. Both these add-ons have next to no learning curve. The level of entry is very low and I think are exactly what web developers that make excuses for not testing their own security need. The other thing is that it helps developers understand how these attacks can be carried out. XSS Me currently only tests for reflected XSS. It doesn’t attempt to compromise the security of the target system. Both XSS Me and SQL Inject Me are reconnaissance tools, where the information is the vulnerabilities found. XSS Me doesn’t support stored XSS or user supplied data from sources such as cookies, links, or HTTP headers. How effective XSS Me is in finding vulnerabilities is also determined by the list of attack strings the tool has available. Out of the box the list of XSS attack strings are derived from RSnakes collection which were donated to OWASP who now maintains it as one of their [cheat-sheets](https://www.owasp.org/index.php/XSS_Filter_Evasion_Cheat_Sheet).. Multiple encodings are not yet supported, but are planned for the future. You can help to keep the collection up to date by submitting new attack strings.

##### [Tamper Data](https://addons.mozilla.org/en-US/firefox/addon/tamper-data/)

Is a light weight HTTP intercepting proxy as an Iceweasel add-on which allows you to view and modify headers and post parameters. Once you add it to your browser, open it up and add a new proxy. We will use this one for Burpsuite later.

Proxy Name: Burp 8080 # or what ever you like.  
Host or IP Address: `127.0.0.1`  
Port: `8080`

##### User Agent Switcher





%% #### OpenVAS
%% http://blog.binarymist.net/2014/03/29/up-and-running-with-kali-linux-and-friends/#openVAS
   

### Additional Hardware


#### TP-LINK TL-WN722N USB Wireless Adapter

It is often very helpful to have at least a couple of Wi-Fi adapters when you are performing certain wireless reconnaissance or attacks or even so you can maintain internet connectivity using your phone as a wireless hot-spot while you are on another network. Useful for researching while your connected to a targets wireless Access Point (AP). An easy way to do this is to use the laptops on-board wireless interface to connect to the phones wireless hot-spot and pass the USB Wi-Fi adapter straight to the guest.

There are many devices of varying specifications. I've found (along with others) that the TL-WN722N hits a good sweet spot of cross platform compatibility, price, size, easy to find and a few other considerations.

As I find it flexible to run pen testing set-ups as VMs, that is what the following addresses. Using VirtualBox 4.3.18_r96516 (but known to work on later releases also), USB devices have to be "passed through" to the guest. The following process explains what is involved.

The following is the process I found to set-up the pass-through on Kali 1.1.0
(same process for 2.0) guest, by-passing the Linux Mint 17.1 (Rebecca) Host (in my case).

##### Wi-Fi Adapter:

TP-LINK TL-WN722N Version 1.10

* chip-set: Atheros ar9271
* Vendor ID: 0cf3
* Product ID: 9271
* Module (driver): ath9k_htc

![](images/TL-WN722N.jpg)

##### Useful commands:

* `iwconfig`
* `ifconfig`
* `sudo lshw -C network`
* `iwlist scan`
* `lsusb`
* `dmesg | grep -e wlan -e ath9`
* contents of `/var/log/syslog`
* `lsmod`
* Release DHCP assigned IP. Similar to Windows ipconfig /release  
`dhclient -r [interface-name]``
* Renew DHCP assigned IP. Similar to Windows ipconfig /renew  
`dhclient [interface-name]`

##### Reconnaissance:

When you plug the Wi-Fi adapter into your laptop and run `lsusb`, you should see a line that looks like:

`ID 0cf3:9271 Atheros Communications, Inc. AR9271 802.11n`

The first four hex digits are the Vendor ID and the second four hex digits are the Product ID.

If you have a look from the bottom up of the `/var/log/syslog` file, you’ll see similar output to the following:

{title="/var/log/syslog", linenos=off, lang=bash}
    kernel: [ 98.212097] usb 2-2: USB disconnect, device number 3
    kernel: [ 102.654780] usb 1-1: new high-speed USB device number 2 using ehci_hcd
    kernel: [ 103.279004] usb 1-1: New USB device found, idVendor=0cf3, idProduct=7015
    kernel: [ 103.279014] usb 1-1: New USB device strings: Mfr=16, Product=32, SerialNumber=48
    kernel: [ 103.279020] usb 1-1: Product: UB95
    kernel: [ 103.279025] usb 1-1: Manufacturer: ATHEROS
    kernel: [ 103.279030] usb 1-1: SerialNumber: 12345
    kernel: [ 103.597849] usb 1-1: ath9k_htc: Transferred FW: htc_7010.fw, size: 72992
    kernel: [ 104.596310] ath9k_htc 1-1:1.0: ath9k_htc: Target is unresponsive
    kernel: [ 104.596328] Failed to initialize the device
    kernel: [ 104.605694] ath9k_htc: probe of 1-1:1.0 failed with error -22

##### Provide USB privileges to guest:

First of all you need to add the user that controls guest to the vboxusers group on the host so that VMs can control USB devices. logout/in of/to host.

##### Provide USB recognition to guest:

Install the particular VirtualBox Extension Pack on to the host. These packs can be found [here](https://www.virtualbox.org/ticket/9511?cversion=0&cnum_hist=2). If you have an older version of VirtualBox, you can find them [here](https://www.virtualbox.org/wiki/Download_Old_Builds_4_3_pre24). Do not forget to checksum the pack before you add the extension.

1. `apt-get update`
2. `apt-get upgrade`
3. `apt-get dist-upgrade`
4. `apt-get install linux-headers-$(uname -r)`
5. Shutdown Linux guest OS
6. Apply extension to VirtualBox in the host at: File -> Preferences -> Extensions

##### Blacklist Wi-Fi Module on Host:

Unload the `ath9k_htc` module to take effect immediately and blacklist it so that it does not load on boot. The module needs to be blacklisted on the host in order for the guest to be able to load it. Now we need to check to see if the module is loaded on the host with the following command:

`lsmod | grep -e ath`

We are looking for `ath9k_htc`. If it is visible in the output produced from previous command, unload it with the following command:

`modprobe -r ath9k_htc`

Now you will need to create a blacklist file in `/etc/modprobe.d/`. Create `/etc/modprobe.d/blacklist-ath9k.conf` and add the following text into it and save:

`blacklist ath9k_htc`

Now go into the settings of your VM -> USB -> and add a Device Filter. I name this tl-wn722n and add the Vendor and Product ID’s we discovered with `lsusb`. Make sure The “Enable USB 2.0 (EHCI) Controller” is enabled also.

![](images/USBDeviceFilter.png)

##### Upgrade Driver on Guest:

Start the VM.

Install the latest firmware-atheros package:

On the guest, check to see which version of firmware-atheros is installed:

`dpkg-query -l '*atheros*'`

Will probably be `0.44kali` whether you’re on Kali Linux 1.0.0 or 2.

`aptitude show firmware-atheros`

Will provide lots more information if you are interested. So now we need to remove this old package:

`apt-get remove --purge firmware-atheros`

Add the jessie-backports (that’s Debian 8.0) repository to your `/etc/apt/sources.list` in the following form:

`deb http://ftp.nz.debian.org/debian jessie-backports main contrib non-free`

Change the country prefix to your country if you like and follow it up with an update:

`apt-get update`

Then install the later package from the new repository we just added:

`apt-get install -t jessie-backports firmware-atheros`

Now if you run the `dpkg-query -l '*atheros*'` command again, your package should be on version `0.44~bp8+1`

##### Test: 

Plug your Wi-Fi adapter into your laptop.

In the Devices menu of your guest -> USB Devices, you should be able to select the “ATHEROS USB2.0 WLAN” adapter.

Run `dmesg | grep htc` and you should see something similar to the following printed:

{linenos=off, lang=bash}
    [ 4.648701] usb 2-1: ath9k_htc: Firmware htc_9271.fw requested
    [ 4.648805] usbcore: registered new interface driver ath9k_htc
    [ 4.649951] usb 2-1: firmware: direct-loading firmware htc_9271.fw
    [ 4.966479] usb 2-1: ath9k_htc: Transferred FW: htc_9271.fw, size: 50980
    [ 5.217395] ath9k_htc 2-1:1.0: ath9k_htc: HTC initialized with 33 credits
    [ 5.860808] ath9k_htc 2-1:1.0: ath9k_htc: FW Version: 1.3

You should now be able to select the phones wireless hot-spot you want to connect to in network manager.







## Windows

_Todo_