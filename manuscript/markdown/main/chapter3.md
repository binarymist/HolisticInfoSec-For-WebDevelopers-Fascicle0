# 3. Tooling Setup {#tooling-setup}

All the software tools I use for penetration and security testing are free, and most of them are open source. I am always diligent in focusing on the free and open source tools, as it makes it easier:

1. To pitch tooling acquisition to managers, and those that hold the purse strings.
2. To learn how the tools work and contribute to the security ecosystem.

## Kali Linux {#tooling-setup-kali-linux}

In the words of Offensive Security (Creators of Kali Linux), [Kali Linux](http://docs.kali.org/introduction/what-is-kali-linux) is an advanced Penetration Testing and Security Auditing Linux distribution. For those that are familiar with BackTrack, basically Kali is a newer creation based on Debian rather than Ubuntu, with significant improvements over BackTrack.

Offensive Security Kali Linux is free and always will be. It is also completely open (as it is based on Debian) to modification of its OS or programmes.  
Kali is also FHS compliant in that the file system complies to the Linux File-system Hierarchy Standard.  
Wireless device support is vast, including wireless USB devices.  
As with BackTrack 5, the Kali ISO also has an option to boot into forensics mode. No drives are written to (including swap) and no drives will be auto mounted upon insertion.

Some of the following recommendations have come from my [blog post](http://blog.binarymist.net/2014/03/29/up-and-running-with-kali-linux-and-friends/) originally used in a PenTest Magazine article.

When it comes to actually installing Kali on hardware, there are a multitude of options available.

All externally listening services are disabled by default, but very easy to turn on if/when required. The goal is to reduce chances of detecting the presence of Kali.

Kali also provides a simple way to create your own ISO image from the latest source. You can include the packages you want and exclude the ones you don't, you can also customise the kernel. The options are virtually limitless.

The default desktop environment in Kali 2.0 is Gnome3, but Kali also provides an easy way to configure which desktop environment you use.

The alternative options provided are: KDE, LXDE, XFCE, I3WM and MATE.

Kali has really embraced the Debian ethos of being able to be run on pretty well any hardware with extreme flexibility. This is great to see.

### What's Included in Kali Linux {#tooling-setup-kali-linux-whats-included-in-kali-linux}

There are more than 300 security programmes packaged with the operating system. Before installation you can view the tools included in the [Kali repository](http://git.kali.org/gitweb/), or once installed, by issuing the following command:

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

Offensive Security (the creators of Kali Linux) provide many options for running their distribution including ISO, turn-key [VMware or VirtualBox](https://www.offensive-security.com/kali-linux-vmware-virtualbox-image-download/) image, [custom ARM images](https://www.offensive-security.com/kali-linux-vmware-arm-image-download/), and Mobile images ([NetHunter](https://www.kali.org/kali-linux-nethunter/)).
I have used the ISO installed on a VM guest for this set-up, as I had a capable host for running this VM side by side with my preferred Windows VM. I did not want the pre-generated SSH host key, which is why I chose the ISO over the turn-key image. Learn more about the dangers of using an install with the pre-generated SSH host key via Nilesh Kapoor's talk at OWASP NZ Day 2016 on [Host Hardening](https://www.owasp.org/index.php/OWASP_New_Zealand_Day_2016#tab=Presentation_Schedule). Slide 12 details how to regenerate the SSH keychain. Note: If you install on physical hardware, you can skip this section and move to the "[Tools I Use](#tooling-setup-kali-linux-tools-i-use-that-need-adding-to-kali-linux)" section.

The Offensive Security team, which can be found on [IRC](http://docs.kali.org/community/kali-linux-irc-channel), are also very helpful and quick to respond to package requests, etc.

You should find most of what you need at [kali category installation](http://docs.kali.org/category/installation), just follow the links specific to your requirements.

For this install I created a new VirtualBox VM based on Debian 64.  
I created a dynamically allocated 45GB VDI disk with 4GB RAM. More RAM is helpful sometimes, but in most cases I have found 4GB to be enough. You can also increase the size of the disk if you want to, 50GB will give you a little more breathing room, especially if you have additional tools or password lists. With this install, I ended up using a total of 24.2 GB.

The following are just my preferences, you can play with these as you like.  
Once the machine is created, I go into the settings and change the location of Snapshots to be stored with the VM, which is running on its own SSD.  
Then I change the Shared Clipboard and Drag'n'Drop to Bidirectional.  
Change Processors to use as many cores as you can without affecting the rest of your system and any other VMs running on it.  
Increase the Video Memory to at least 64MB.  
I usually like to add a shared folder so its easier to copy files from host <-> guest. Do not forget to add your user to the vboxsf group that VirtualBox Guest Additions Installer creates below.  

Setup network host adapters: this is also discussed in the [NodeGoat Set-up on your local machine](#process-agile-development-and-practices-security-regression-testing-nodegoat-set-up-on-your-local-machine) section in the Process and Practises chapter:

Host-only Adapter allows:

1. The VM to talk to the host and other VMs connected to the host.
2. The host to talk to guest VMs.  
(often useful for the likes of demoing OWASP Zap Api where you may have Zap running on Kali and a vulnerable web app running on another VM or your physical hardware).  

NAT networking allows the VM to talk to the internet via the host's network interface.

Both Host-only and NAT networking are invisible beyond the host's network edge.

On your physical host, you can set-up your NAT adapter as follows:

![](images/PhysicalNatNetwork.png)

{pagebreak}

and your Host-only adapter like:

![](images/PhysicalHostOnlyAdapterNetwork.png)

![](images/PhysicalHostOnlyDHCPNetwork.png)

{pagebreak}

On your Kali Linux VM, set up your NAT adapter like this:

![](images/GuestNATAdapter.png)

{pagebreak}

and your Host-only adapter like:

![](images/GuestHostOnlyAdapter.png)

Then, any time you need to be on the same network segment as the physical host, just switch the Host-only Adapter to Bridged Adapter. You may need to set the interface in the "Name" drop-down box to wlan0 or what ever the primary interface is that your physical host is using to connect to the physical network. You may need to update your `/etc/network/interfaces` here also to the default that the install provided.

Alternatively, make the switch to Bridged Adapter on Adapter 1. This way you don't need to change the `/etc/network/interfaces` at all. In VirtualBox 5.0.4 I couldn't get an internet connection through any of the VirtualBox adapters, no matter how I configured them. It seems that VirtualBox may be a little buggy in terms of NAT and NAT Network. If you run into this problem, and you decide to set-up the external wireless adapter as discussed in the [Additional Hardware](#tooling-setup-kali-linux-additional-hardware) section below, you can bypass the VirtualBox adapters all together. This worked for me.

%% This looked good, but it didn't work: https://www.pythian.com/blog/test-lab-using-virtualbox-nat-networking/

The ISO can be downloaded from [kali downloads](https://www.kali.org/downloads/) via torrent and direct download and is a little over 3GB in size.

You will want to check the validity of the ISO (or other image if that is your preference) by verifying the SHA1 checksums. Details are directly under the downloads section and should be enough to get you going.

There are additional details if you want to read them at [http://docs.kali.org/introduction/download-official-kali-linux-images](http://docs.kali.org/introduction/download-official-kali-linux-images), but essentially there are several ways to validate that you have the official Kali Linux installer and not some tampered-with binary. The details at the above link provide a bit more explanation around the validation steps, and importing the Kali GPG public key. At the bottom of the page is the command for cryptographically validating the downloaded binary once you have validated the downloaded `SHA1SUMS` file with the `SHA1SUMS.pgp` key that you also downloaded. You will likely just have to change the actual `.iso` name in the command to that of the `.iso` you downloaded.

You can refer to the hard-disk install ([http://docs.kali.org/installation/kali-linux-hard-disk-install](http://docs.kali.org/installation/kali-linux-hard-disk-install)) for running through the OS installer if you need it. For Partitioning disks, I choose "Guided - use entire disk and set up LVM" for future flexibility.

Once installed, run the following, and regularly thereafter. On the first dist-upgrade, this can take hours depending on how many updates there are:

{linenos=off}
    apt-get update
    apt-get dist-upgrade

[Install Guest Additions:](http://docs.kali.org/general-use/kali-linux-virtual-box-guest).

{linenos=off}
    apt-get update
    apt-get install -y virtualbox-guest-x11
    reboot

In Kali 2016.1 (Sana) the default user is no longer root. If you're installing, make sure you use a decent password, not a dictionary word or similar. It is generally a good idea to use a mixture of upper case, lower case characters, numbers and special characters, and of a decent length. You will get the option to set your password and create a root user, or just use sudo during the install. If you want to change the password later, at the terminal, enter: `passwd` and follow the prompts. Many of the commands used in this book will require root privileges. Prefixing with `sudo` will elevate your privileges to those of root.

I usually set-up the `/etc/network/interfaces` file here for the host-only adapter, we will need this working for the Process and Practises chapter at a minimum, again this is partly discussed in the [NodeGoat Set-up on your local machine](#process-agile-development-and-practices-security-regression-testing-nodegoat-set-up-on-your-local-machine) section in the Process and Practises chapter. Backup your `/etc/network/interfaces` before making changes, and add the following to the bottom of the interfaces:

{linenos=off}
    #Host-only adapter
    allow-hotplug eth1
    iface eth1 inet static
    address 192.168.56.20
    netmask 255.255.255.0
    gateway 192.168.56.1

Once the VirtualBox Guest Additions Installer has run and added the `vboxsf` group, you will need to add your user to that:

{linenos=off}
    sudo adduser $(whoami) vboxsf

I prefer the Mate desktop environment over the default. To set this up, run the following:

{linenos=off}
    apt-get install mate-core mate-desktop-environment-extra mate-desktop-environment-extras

Then, at the login screen, just click the gear icon and change to Mate.

### Tools I Use in Kali Linux requiring config, etc {#tooling-setup-kali-linux-tools-i-use-in-kali-linux-requiring-config-etc}

#### Metasploit {#tooling-setup-kali-linux-tools-i-use-in-kali-linux-config-etc-metasploit}

The Metasploit Community / Pro package is no longer shipping in Kali. Instead, the metasploit-framework package is available. If you need the Community or Pro
edition, you will have to register your personal details with Rapid7 in order to be supplied with a license. There are more details at the kali.org site.

1. To start Metasploit, often I like to see which ports are open first. At the command prompt you can type:  
`ss -ant`
2. Then start postgresql if you require [database support](#additional-resources-using-the-database-and-workspaces-in-metasploit), which is generally quite useful:  
`service postgresql start`  
followed by:  
`ss -ant`  
if you are interested in which ports have been opened. PostgreSQL should be listening on port 5432.  
You can also check the status of the service with:  
`service postgresql status`  
3. Initialise the Metasploit Framework database  
`msfdb init`  
This will create databases: `msf` and `msf_test`, and create a configuration file in  
`/usr/share/metasploit-framework/config/database.yml`  
4. To start Metasploit, just run:  
`msfconsole`  
or possibly with a resource script:  
`msfconsole -r <your custom resource>.rc`  
We used to start the Metasploit service with:  
`service metasploit start`  
but now there is no `metasploit` service as such.

##### Useful metasploit [commands](https://www.offensive-security.com/metasploit-unleashed/msfconsole-commands/)

* `msf >` [help](https://www.offensive-security.com/metasploit-unleashed/msfconsole-commands/#help)
* `msf >` [show](https://www.offensive-security.com/metasploit-unleashed/msfconsole-commands/#show)
  * Valid options to add to show are: `all`, `encoders`, `nops`, `exploits`, `payloads`, `auxiliary`, `plugins`, `options`  
  * Additional module specific parameters are: `missing`, `advanced`, `evasion`, `targets`, `actions`  
* `msf > show options`
* `msf > info <module name>` [info](https://www.offensive-security.com/metasploit-unleashed/msfconsole-commands/#info)

##### metasploit meterpreter client commands

* Meterpreter Client  
[https://en.wikibooks.org/wiki/Metasploit/MeterpreterClient](https://en.wikibooks.org/wiki/Metasploit/MeterpreterClient)
* Meterpreter Basics  
[https://www.offensive-security.com/metasploit-unleashed/meterpreter-basics/](https://www.offensive-security.com/metasploit-unleashed/meterpreter-basics/)

##### Using the database and workspaces in metasploit {#additional-resources-using-the-database-and-workspaces-in-metasploit}

* [Using databases](https://www.offensive-security.com/metasploit-unleashed/using-databases/)
* [Information gathering](http://resources.infosecinstitute.com/information-gathering-using-metasploit/)

#### BeEF {#tooling-setup-kali-linux-tools-i-use-in-kali-linux-config-etc-beef}

Check-out the recommended [configuration](https://github.com/beefproject/beef/wiki/Configuration).

Modify the following two files as required:

1. `/etc/beef-xss/config.yaml`
2. `/usr/share/beef-xss/extensions/metasploit/config.yaml`

If you need Metasploit integration in BeEF (in most cases you will want this), set:  
`extension: metasploit: enable: true`  
in the `/etc/beef-xss/config.yaml` file.  
Also make sure  
`enable`  
is set to `true` in `/usr/share/beef-xss/extensions/metasploit/config.yaml`

When running Metasploit for BeEF, I often provide `msfconsole` with a Metasploit resource file specifically for BeEF (I call this `beef.rc` and put it in `~/`). This resource file will have the following in it at a minimum:

`load msgrpc ServerHost=127.0.0.1 Pass=abc123`

Start postgresql, then metasploit like:

`msfconsole -r beef.rc`

If not using the resource file, once `msfconsole` is running, in order to enable RPC communication for BeEF, I enter:

`load msgrpc ServerHost=127.0.0.1 Pass=abc123`

Finally... starting BeEF. There are at least three ways to do so. I find that the first is the most informative and interactive. Also, do not forget to run beef as root.

* Contrary to a blog post on the [beefproject](http://blog.beefproject.com/2014/06/kali-formerly-backtrack-linux-beef.html), I've found the most useful way to run BeEF to be from the `/usr/share/beef-xss/` directory  
`./beef`  
This way provides the most feedback. If the main `config.yaml`  
(resides in `/etc/beef-xss/config.yaml`) contains:  
`extension: console: shell: enable: true`  
then we also get an interactive [console](https://github.com/beefproject/beef/wiki/BeEF-Console), but no web UI.
* Second, running from the command line:  
`service beef-xss start`  
gives no other feedback.
* Third, running BeEF from the Kali menu:  
Menu: Exploitation Tools -> BeEF XSS Framework -> beef  
doesn't provide a lot of feedback as to what is loaded, and that is only if you are running as root.

#### Updating BurpSuite

If you run Burp and it tells you that "A new version of Burp is available.", follow these directions:

1. Get the new version (it is a `.jar` file) from [https://portswigger.net/burp/download.html](https://portswigger.net/burp/download.html)
2. From `~/Downloads`
3. `mv burpsuite_[version].jar /usr/bin`
4. From `/usr/bin`
5. `mv burpsuite burpsuite-old`
6. `mv burpsuite_[version].jar burpsuite`
7. `chmod 755 burpsuite`
8. `chown root:root burpsuite`
9. Test Burp

### Tools I Use That Need Adding to Kali Linux {#tooling-setup-kali-linux-tools-i-use-that-need-adding-to-kali-linux}

I now take a backup in case I need to revert. With VirtualBox it is very easy to take a snap-shot that can be reverted to at any time. Snap-shots are excellent for returning to a known state between penetration tests. Testing is not really testing at all unless you can reproduce the same results during each test. Starting from a known state is essential for this.

#### Terminator {#tooling-setup-kali-linux-tools-i-use-that-need-adding-to-kali-linux-terminator}

As I spend most of my life in terminals, I want a good one. I have found that Terminator does everything I need from a terminal. Briefly discussed on my [blog](http://blog.binarymist.net/2013/01/19/a-decent-console-for-windows/). If you also like a decent terminal experience, then:  
`apt-get install terminator`

#### [Discover Scripts](#process-and-practises-penetration-testing-reconnaissance-discover-scripts) {#tooling-setup-kali-linux-tools-i-use-that-need-adding-to-kali-linux-discover-scripts}

These are a set of shell scripts tied together in a CUI to aggregate Kali Linux information gathering tools and automate various penetration testing tasks. Both passive and active options allow you to dig up a lot of dirt on your target, long before you start trying to penetrate them. So, rather than getting familiar with all the reconnaissance tools, you can just become familiar with Discover Scripts.

1. `git clone https://github.com/leebaird/discover.git /opt/discover/`
2. `cd /opt/discover/ && ./update.sh`

#### [SmbExec](https://github.com/pentestgeek/smbexec) {#tooling-setup-kali-linux-tools-i-use-that-need-adding-to-kali-linux-smbexec}

SmbExec connects to the Windows Domain Controller and, using the Windows Shadow Copy (AKA Volume Snapshot Service (VSS)), grabs the ntds.dit file, which is the heart of Active Directory, including user accounts.

The set-up process looks like this in Kali 2016.1

1. `git clone https://github.com/pentestgeek/smbexec.git /opt/smbexec`
2. `cd /opt/smbexec && ./install.sh`
3. Select "1 - Debian/Ubuntu and derivatives"
4. [Enter] to agree to the install location of `/opt`  
You may also need to:
5. cd /opt/smbexec/progs
6. ln -s /usr/bin/pth-winexe smbwinexe  
# in Kali 1.1. the following link was not needed as smbexeclient already existed.
7. ln -s /usr/bin/pth-smbclient smbexeclient

%% Had discussion with Jonny via email about the links.
%% This is also broken currently. See https://github.com/pentestgeek/smbexec/issues/129

#### [Gitrob](https://github.com/michenriksen/gitrob)

Gitrob is an OSINT reconnaissance tool for obtaining information from your target's github account(s), or more.

1. `git clone https://github.com/michenriksen/gitrob.git /opt/gitrob`
2. `gem install bundler`
3. `apt-get install libpq-dev`
3. `service postgresql start`
4. `su postgres`
5. `createuser -s  gitrob  --pwprompt` # Do not use a password with [@](https://github.com/michenriksen/gitrob/issues/63) in it. 
6. `createdb -O gitrob gitrob`
7. `exit`
8. `cd /opt/gitrob/bin`
9. `gem install gitrob`

You'll need to acquire a [github API token](https://github.com/settings/tokens), because gitrob derives its OSINT from the [github API](https://developer.github.com/v3/)

Now, you configure gitrob with the information we have just discussed. Running the following will prompt you for the details:

`gitrob configure`  
This will write to ~/.gitrobrc

To run:

`gitrob [repo1, repo2, etc] [usernames, ...]`  
or see the documentation for more details

%% Errors installing. Submitted issue here: https://github.com/michenriksen/gitrob/issues/62
%% Error running. Didn't like my password: https://github.com/michenriksen/gitrob/issues/63

#### [CMSmap](https://github.com/Dionach/CMSmap)

CMSmap is a python open source CMS scanner that automates the process of detecting security flaws of the most popular Content Management Systems (CMSs).  
Currently supports: WordPress, Joomla and Drupal.

`git clone https://github.com/Dionach/CMSmap.git /opt/CMSmap`

#### [Veil Framework](https://www.veil-framework.com/) {#tooling-setup-kali-linux-tools-i-use-that-need-adding-to-kali-linux-veil-framework}

I have decided to clone the Veil-Framework, as it has a good collection of very useful tools. Veil-Evasion is specifically useful for antimalware evasion. The Veil super project also has an install script to install all Veil projects, found at the [Veil](https://github.com/Veil-Framework/Veil) repository for the Veil-Framework account on github.

There are install guides here:  
[https://www.veil-framework.com/guidesvideos/](https://www.veil-framework.com/guidesvideos/)

To install:

1. 
    {line-numbers=off}
       git clone https://github.com/Veil-Framework/Veil.git /opt/Veil
2. 
    {line-numbers=off}
       cd /opt/Veil && ./Install.sh -c  
  The dependency installs take a while, these include Python on Windows, Ruby and Go
3. Select the defaults for the Python for Windows install.
4. When the install reaches the "Select Destination Directory", go with the default.
5. Click the "Yes" when asked if you want to overwrite the existing Python files.
6. Select the defaults for the rest of the Python install.
7. Next up is the pywin32 set-up. Click "Next" to continue.
8. Use the default Python directory location -> click "Next" -> "Next" -> "Finish".
9. Next up is pycrypto. "Next" -> "Next" -> "Next" -> "Finish".
10. Choose your language.
11. Accept the Ruby License Agreement.
12. "Install" -> "Yes" to Folder Exists -> "Finish".
13. Finally, golang.

To run Veil-Evasion:  
`cd /opt/Veil/Veil-Evasion/ && ./Veil-Evasion.py`

#### Password Lists {#tooling-setup-kali-linux-tools-i-use-that-need-adding-to-kali-linux-password-lists}

1. `mkdir ~/Desktop/password_list`
2. Download password lists:
  * Rockyou [https://downloads.skullsecurity.org/passwords/rockyou.txt.bz2](https://downloads.skullsecurity.org/passwords/rockyou.txt.bz2) 139.9 MB decompressed, 14344391 passwords. These became available from a social game and advertising website in 2009
  * Crackstation: If you have the room, you can download the full list, otherwise the human-only is a lot smaller, 716.4 MB decompressed, including 63941069 passwords
  [https://crackstation.net/buy-crackstation-wordlist-password-cracking-dictionary.htm](https://crackstation.net/buy-crackstation-wordlist-password-cracking-dictionary.htm)
  * m3g9tr0n_Passwords_WordList_CLEANED  
  [http://blog.thireus.com/cracking-story-how-i-cracked-over-122-million-sha1-and-md5-hashed-passwords](http://blog.thireus.com/cracking-story-how-i-cracked-over-122-million-sha1-and-md5-hashed-passwords) 913.8 MB decompressed. 83653572 passwords
  * Take your pick of:
    1. [https://downloads.skullsecurity.org/passwords/](https://downloads.skullsecurity.org/passwords/)
    2. [http://download.g0tmi1k.com/wordlists/large/](http://download.g0tmi1k.com/wordlists/large/)
3. Decompress any that need it.

There are many other good lists out there. Also if you:  
`locate wordlists`  
on your Kali Linux install, you will find many already exist in Kali Linux.

#### Common User Passwords Profiler (cupp) {#tooling-setup-kali-linux-tools-i-use-that-need-adding-to-kali-linux-cupp}

Cupp is an easy to use password profiling tool.

`git clone https://github.com/Mebus/cupp.git /opt/cupp`

#### [Http Screenshot](https://github.com/breenmachine/httpscreenshot) {#tooling-setup-kali-linux-tools-i-use-that-need-adding-to-kali-linux-httpscreenshot}

This tool is useful for taking screen shots and HTML of a large number of websites concurrently.  
It can also use Masscan under the covers.

1. `pip install selenium`
2. `git clone https://github.com/breenmachine/httpscreenshot.git /opt/httpscreenshot`
3. phantomjs is a requirement. This was installed when we installed Discover Scripts from above. Discover Scripts installs rawr and rawr installs phantomjs in `/opt/rawr/data/phantomjs/bin/phantomjs`, so we need to create a symlink:  
`ln -s /opt/rawr/data/phantomjs/bin/phantomjs /usr/bin/phantomjs`
4. `apt-get install swig libssl-dev python-dev python-pip`  
According to the installation directions, swig2.0 was required, although the relevant swig2.0 or swig3.0 is a dependency, so it doesn't need to be specified
5. `cd /opt/httpscreenshot && pip install -r requirements.txt`
6. If you want to run `masshttp.sh`, edit the `/opt/httpscreenshot/masshttp.sh`
  * Make sure the path to the masscan binary is correct. `whereis masscan` will reveal its location. By default it will be in `/usr/bin/masscan`
  * Change the two locations that specify where httpscreenshot.py is from  
  `~/tools/httpscreenshot.py` to `/opt/httpscreenshot/httpscreenshot.py`
  * You can also add any extra ports you want to scan, `8080,8001` etc
  * Create a networks.txt and add the cidr range you want to scan
  * `cd /opt/httpscreenshot && ./masshttp.sh`
7. If you want to run `httpscreenshot.py` by itself, the simplest way:
  * `cd /opt/httpscreenshot && vim inputURLs`  
  Now, add the URLs you want screen shots of and save
  * Then run it:  
  `./httpscreenshot.py -l inputURLs -p -w 5 -a -vH`

%% Issue submitted: https://github.com/breenmachine/httpscreenshot/issues/20

#### [Psmsf](https://github.com/nixawk/psmsf)

Useful for PowerShell and other attacks. Generates PowerShell and other payloads useful for evading anti-virus (AV) detection. Also provides the Metasploit resource files.

`git clone https://github.com/nixawk/psmsf.git /opt/psmsf`

#### [PowerSploit](https://github.com/PowerShellMafia/PowerSploit/) {#tooling-setup-kali-linux-tools-i-use-that-need-adding-to-kali-linux-powersploit}

PowerSploit includes PowerShell scripts for all phases of an assessment.

`git clone https://github.com/PowerShellMafia/PowerSploit/.git /opt/PowerSploit`

#### Nishang

These are also a collection of PowerShell scripts useful for exploitation and post-exploitation.

`git clone https://github.com/samratashok/nishang /opt/nishang`

#### Responder

This is a LLMNR, NBT-NS and MDNS poisoner. It has built in authentication servers for SMB (Supports NTLMv1, NTLMv2 hashes with Extended Security NTLMSSP), MSSQL, HTTP(s), LDAP, FTP, POP3, IMAP, SMTP as well as a built-in DNS server.

`git clone https://github.com/SpiderLabs/Responder.git /opt/Responder`

#### Custom Scripts from The Hacker Playbook 2

A collection of scripts produced by Peter Kim:

* `git clone https://github.com/cheetz/Easy-P.git /opt/Easy-P`
* `git clone https://github.com/cheetz/Password_Plus_One /opt/Password_Plus_One`
* `git clone https://github.com/cheetz/PowerShell_Popup /opt/PowerShell_Popup`
* `git clone https://github.com/cheetz/icmpshock /opt/icmpshock`
* `git clone https://github.com/cheetz/brutescrape /opt/brutescrape`
* `git clone https://github.com/cheetz/reddit_xss /opt/reddit_xss`

%% Permission requested to include: https://github.com/cheetz/Easy-P/issues/1

#### BypassUAC {#tooling-setup-kali-linux-tools-i-use-that-need-adding-to-kali-linux-bypassuac}

BypassUAC is used to bypass User Access Controls, post-exploitation, allowing us to get system privileges. I used this in the demo attack I describe in the Website section, under Spoofing, in the Risks section of the Network chapter of [Fascicle 1](https://leanpub.com/holistic-infosec-for-web-developers-fascicle1-vps-network-cloud-webapplications). There is, in fact, a way to do this that is less likely to trigger an AV alert. I cover this under the below set of directions.

In Kali 1.1

1. `cd /opt/`
2. `wget http://www.secmaniac.com/files/bypassuac.zip`
3. `unzip bypassuac.zip`
4. `cp bypassuac/bypassuac.rb /opt/metasploit/apps/pro/msf3/scripts/meterpreter/`
5. `mv bypassuac/uac/ /opt/metasploit/apps/pro/msf3/data/exploits/`

Use of the above script typically leads to an executable that is dropped on the target machine, which also spawns a second file. Often, AV will trigger from one of these two files. A better way, and the way I now use, is to use the `bypassuac_injection` module instead.

In Kali 2016.1 rolling, executing the demo attack mentioned above may appear as follows:

{linenos=off}
    msf exploit(handler) >
    [*] Starting the payload handler...
    ...
    msf exploit(handler) > sessions -i 1
    [*] Starting interaction with 1...
    meterpreter > getsystem
    [-] priv_elevate_getsystem: Operation failed: The environment is incorrect.
    
    # If you can obtain an administrative account,  
    # usually it will be UAC that prevents getsystem from being successful.
    
    meterpreter > background
    [*] Backgrounding session 1...
    msf exploit(handler) > use exploit/windows/local/bypassuac_injection
    msf exploit(bypassuac_injection) > set target 1
    target => 1
    msf exploit(bypassuac_injection) set payload <your chosen payload>
    payload => <your chosen payload>
    msf exploit(bypassuac_injection) exploit
    ...

#### [NoSQLMap](http://www.nosqlmap.net/)

NoSQLMap is designed to audit and automate injection attacks, and exploit default configuration weaknesses in NoSQL databases, as well as web applications using NoSQL.

`git clone https://github.com/tcstool/NoSQLMap.git /opt/NoSQLMap`

#### [Spiderfoot](http://www.spiderfoot.net/)

Spiderfoor is an automated OSINT gathering tool. 

1. `pip install lxml netaddr M2Crypto cherrypy mako`
2. `git clone https://github.com/smicallef/spiderfoot /opt/spiderfoot`
3. `cd /opt/spiderfoot && ./sf.py`  
  SpiderFoot starts a web server on http://127.0.0.1:5001  
  The interface and port are configurable, so you can run it from anywhere.

#### [OWASP SecLists](http://github.com/danielmiessler/SecLists.git) {#tooling-setup-kali-linux-tools-i-use-that-need-adding-to-kali-linux-owasp-seclists}

These are a collection of many different lists used during security assessments, which include user-names, passwords, URLs, sensitive data patterns, fuzzing payloads, web shells, and many more. Their goal is to enable a security tester to pull this repository onto a new testing system and have access to every type of list that may be needed.

`git clone http://github.com/danielmiessler/SecLists.git /opt/SecLists`

#### [Net-creds](https://github.com/DanMcInerney/net-creds)

Sniff passwords and hashes from a network interface or pcap file with Net-creds. 

`git clone https://github.com/DanMcInerney/net-creds.git /opt/net-creds`

#### Unix-privesc-check

The old 1.x shell script is shipped with Kali Linux (now the 1_x branch), but the newer branch "master" is more thorough, the code is cleaner, although still considered somewhat experimental. The master branch also has a lot more files.

The old 1.x shell script for obvious reasons may be more portable for you. It also runs faster, because it does less. unix-privesc-check can be cloned from github.

The home page is: [http://pentestmonkey.net/tools/audit/unix-privesc-check](http://pentestmonkey.net/tools/audit/unix-privesc-check)

`git clone https://github.com/pentestmonkey/unix-privesc-check.git /opt/unix-privesc-check`

#### LinEnum

Another excellent shell script for performing host reconnaissance when you are on the host and looking to escalate your privilages. You need to get this script onto your targets box and just run it. You can find the required details on the `README.md` at the github.

`git clone https://github.com/rebootuser/LinEnum.git /opt/LinEnum`

#### Chromium {#tooling-setup-kali-linux-tools-i-use-that-need-adding-to-kali-linux-chromium}

`sudo apt-get install chromium`

As a web developer I use the chromium dev tools more frequently than FireFox dev tools, although both have their strengths.

In Kali Linux 1.1:

In order to run this under the root account, you will need to add the following parameter to `/etc/chromium/default` between the quotes for `CHROMIUM_FLAGS=""`

`--user-data-dir`

In Kali 2016.1 rolling:

We no longer must run everything as root, so this is no longer an issue.

#### Chromium Extensions {#tooling-setup-kali-linux-tools-i-use-that-need-adding-to-kali-linux-chromium-extensions}

##### FoxyProxy Standard {#tooling-setup-kali-linux-tools-i-use-that-need-adding-to-kali-linux-chromium-extensions-foxyproxy-standard}

FoxyProxy Standard really reduces friction with web proxy interception. FoxyProxy is a very handy add-on for both Chromium and FireFox, although it seems to have more options for FireFox, or at least they are more easily accessible. It allows you to set-up a list of proxies, and then switch between them as you need.

FoxyProxy provides a menu button, so with two clicks you can disable the add-on completely to revert to your previous settings, or select any of your predefined proxies. This is a real time saver.

Once you add it to your browser, open it up, and add a new proxy. We will use this one for Burpsuite later.

Proxy Name: Burp 8080 # or what ever you like.  
Host or IP Address: `127.0.0.1`  
Port: `8080`

##### ScriptSafe

I like to be in control of where my JavaScript is coming from.

##### Cookies

##### EditThisCookie

##### SessionBuddy

For storage of browser sessions and easy hydration.

##### User Agent Switcher for Chrome

##### Web Developer

I'm a web developer, it has some really useful tools that provide visibility and insight.

#### IceWeasel (FireFox with different Licensing) add-ons {#tooling-setup-kali-linux-tools-i-use-that-need-adding-to-kali-linux-iceweasel-add-ons}

##### FoxyProxy Standard

For [Chromium](#tooling-setup-kali-linux-tools-i-use-that-need-adding-to-kali-linux-chromium-extensions-foxyproxy-standard)

##### NoScript

I like to know where my JavaScript is coming from.

##### Tamper Data

##### Web Developer

I'm a web developer, it has some really useful tools that provide visibility and insight.

##### HackBar

HackBar is somewhat useful for (en/de)coding (Base64, Hex, MD5, SHA-(1/256), etc), manipulating and splitting URLs

##### Advanced Cookie Manager

##### NoScript

I like to know where my JavaScript is coming from.

##### SQL Inject Me

This tool is simple and often useful for running a quick vulnerability assessment and is open source software (GPLv3) from Security Compass Labs. SQL Inject Me is a component of the Exploit-Me suite, allowing you to test all or any number of input fields on all or any of a page's forms. Just fill in the fields with valid data, then test with all the tools attacks, or with those you have defined in the options menu. It then looks for database errors which are rendered into the returned HTML as a result of sending escape strings, it does not cater for blind injection. You can also add or remove escape strings, and resulting error strings that SQL Inject Me should look for on response. The order in which each escape string can be tried can also be changed. All you need to know can be found [here](http://labs.securitycompass.com/exploit-me/sql-inject-me/sql-inject-me-faq/).

##### XSS Me

XSS Me is also simple and often useful for running a quick vulnerability assessment. It also is open source software (GPLv3) from Security Compass Labs and an component of the Exploit-Me suite. This tools behaviour is very similar to SQL Inject Me (follows the Principle of Least Astonishment (POLA)), which makes using the tools very easy. Both these add-ons have next to no learning curve. The level of entry is very low and I think are exactly what web developers need to eliminate excuses for not testing their own security. It also helps developers understand how these attacks can be carried out. XSS Me currently only tests for reflected XSS. It does not attempt to compromise the security of the target system. Both XSS Me and SQL Inject Me are reconnaissance tools, where the information provided is simply the vulnerabilities found. XSS Me does not support stored XSS or user supplied data from sources such as cookies, links, or HTTP headers. XSS Me's effectiveness in finding vulnerabilities is also determined by the list of attack strings the tool has available. Out of the box, the list of XSS attack strings are derived from RSnake's collection, which were donated to OWASP and who now maintains it as one of their [cheat-sheets](https://www.owasp.org/index.php/XSS_Filter_Evasion_Cheat_Sheet). Multiple encodings are not yet supported, but are planned for the future. You can help to keep the collection up to date by submitting new attack strings.

##### [Tamper Data](https://addons.mozilla.org/en-US/firefox/addon/tamper-data/)

This is a light weight HTTP intercepting proxy as an Iceweasel add-on which allows you to view and modify headers and post parameters.

##### User Agent Switcher

%% #### OpenVAS
%% http://blog.binarymist.net/2014/03/29/up-and-running-with-kali-linux-and-friends/#openVAS   

### Additional Hardware {#tooling-setup-kali-linux-additional-hardware}


#### TP-LINK TL-WN722N USB Wireless Adapter

It is often very helpful to have at least a couple of Wi-Fi adapters when performing wireless reconnaissance or attacks, or simply to maintain internet connectivity if using your phone as a wireless hot-spot while you are on another network. It is also useful for researching while your connected to a target's wireless Access Point (AP). An easy way to do this is to use the laptop's on-board wireless interface to connect to the phone's wireless hot-spot, and pass the USB Wi-Fi adapter straight to the guest.

There are many devices of varying specifications. I have found (along with others) that the TL-WN722N hits a good sweet spot of cross platform compatibility, price, size, ease of purchase, and a few other considerations.

As I find it flexible to run pen testing set-ups on VMs, the following addresses such configurations. Using VirtualBox 5.0.4, USB devices have to be "passed through" to the guest. The following process explains what is involved.

The following is the process I have found to set-up the pass-through on Kali 2016.1 (first Kali rolling release. Kernel 4.3, Gnome 3.18), by-passing the Linux Mint 17.3 (Rosa) Host (in my case).

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

The first four hex digits are the Vendor ID, the second four hex digits are the Product ID.

If you have a look from the bottom up of the `/var/log/syslog` file, you will see similar output to the following:

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

First of all, you need to add the user that controls the guest to the vboxusers group on the host, so that VMs can control USB devices. Be sure to logout of the host, then log back in.

##### Provide USB recognition to guest:

Install the appropriate VirtualBox Extension Pack on to the host. These packs can be found here ([https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)) for the most recent,  
and older builds here: ([https://www.virtualbox.org/wiki/Download_Old_Builds_5_0](https://www.virtualbox.org/wiki/Download_Old_Builds_5_0)). Do not forget to checksum the pack before you add the extension. The version of the extension pack must match that of the VirtualBox installed. Now in your guest, check to see if you have the appropriate linux-headers package installed. If you do not, run the following:

1. `apt-get update`
2. `apt-get upgrade`
3. `apt-get dist-upgrade`
4. `apt-get install linux-headers-$(uname -r)`
5. Shutdown Linux guest OS.
6. Apply extension to VirtualBox in the host at: File -> Preferences -> Extensions.

##### Blacklist Wi-Fi Module on Host:

Unload the `ath9k_htc` module to take effect immediately, and blacklist it so that it does not load on boot. The module needs to be blacklisted on the host in order for the guest to be able to load it. Now we need to check to see if the module is currently loaded on the host with the following command:

`lsmod | grep -e ath`

We are looking for `ath9k_htc`. If it is visible in the output produced from the previous command, unload it with the following command:

`modprobe -r ath9k_htc`

Next you will need to create a blacklist file in `/etc/modprobe.d/`  
Create `/etc/modprobe.d/blacklist-ath9k.conf` and add the following text into it and save:

`blacklist ath9k_htc`

I had to do the following step on Kali 1.1, but it seems it is no longer necessary in Kali 2016.1 rolling. If you are still on 1.1, go into the settings of your VM -> USB -> and add a Device Filter. I named this tl-wn722n and added the Vendor and Product IDs we discovered with `lsusb`. Make sure Enable USB 2.0 (EHCI) Controller is also enabled.

![](images/USBDeviceFilter.png)

%% ##### Upgrade Driver on Guest:

%% Start the VM.

%% Install the latest firmware-atheros package:

%% On the guest, check to see which version of firmware-atheros is installed:

%% `dpkg-query -l '*atheros*'`

%% Will probably be `0.44kali` whether you're on Kali Linux 1.0.0 or 2.

%% `aptitude show firmware-atheros`

%% Will provide lots more information if you are interested. So now we need to remove this old package:

%% `apt-get remove --purge firmware-atheros`



%% Don't add any extra repositories to your install.
%% https://www.offensive-security.com/kali-linux/top-10-post-install-tips/



%% Add the jessie-backports (Debian 8.0) repository to your `/etc/apt/sources.list` in the following form:

%% `deb http://ftp.nz.debian.org/debian jessie-backports main contrib non-free`

%% Change the country prefix to your country if you like and follow it up with an update:

%% `apt-get update`

%% Then install the later package from the new repository we just added:

%% `apt-get install -t jessie-backports firmware-atheros`

%% Now if you run the `dpkg-query -l '*atheros*'` command again, your package should be on version `0.44~bp8+1`

##### Test: 

Plug your Wi-Fi adapter into your laptop.

In the Devices menu of your guest -> USB Devices, you should be able to select the ATHEROS USB2.0 WLAN adapter.

Run `dmesg | grep htc`, you should see something similar to the following printed:

{linenos=off, lang=bash}
    [ 4.648701] usb 2-1: ath9k_htc: Firmware htc_9271.fw requested
    [ 4.648805] usbcore: registered new interface driver ath9k_htc
    [ 4.649951] usb 2-1: firmware: direct-loading firmware htc_9271.fw
    [ 4.966479] usb 2-1: ath9k_htc: Transferred FW: htc_9271.fw, size: 50980
    [ 5.217395] ath9k_htc 2-1:1.0: ath9k_htc: HTC initialized with 33 credits
    [ 5.860808] ath9k_htc 2-1:1.0: ath9k_htc: FW Version: 1.3

You should now be able to select the phone's wireless hot-spot you want to connect to in network manager.

## Windows {#tooling-setup-windows}

We will be exploiting Windows machines and networks in Fascicle 1, so for that, you will need a Windows 7, optional Windows 10, and any other Windows Operating Systems you can get set-up to help you hone your knowledge of the systems, how to defend and attack them. Most of the tools I have used have been installed / set-up on a Windows 7 VM. This way we can use the VM for both offence and exploitation. Then just restore to a previous snapshot after each test.

### Tools I Use That Need Adding to Windows {#tooling-setup-windows-tools-i-use-that-need-adding-to-windows}

I now take a backup in case I need to revert. With VirtualBox it is very easy to take a snap-shot that can be reverted to at any time. Snap-shots are excellent for returning to a known state between penetration tests. Testing is not really testing at all unless you can reproduce the same results during each test. Starting from a known state is essential for this.

#### MinGW {#tooling-setup-windows-tools-i-use-that-need-adding-to-windows-mingw}

I tried installing this in July of 2015 and ran into troubles (detailed under the Hyperion section), looks like they have applied a fix now though. The following worked for me:

1. First of all have a read of [http://www.mingw.org/wiki/Getting_Started](http://www.mingw.org/wiki/Getting_Started)
2. Then downloaded and install MinGW from [http://sourceforge.net/projects/mingw/](http://sourceforge.net/projects/mingw/), we just need gcc selected
3. I also read [http://www.mingw.org/wiki/HOWTO_Install_the_MinGW_GCC_Compiler_Suite](http://www.mingw.org/wiki/HOWTO_Install_the_MinGW_GCC_Compiler_Suite) for some more information
4. I also needed to add `C:\MinGW\bin` to my System Path

#### Hyperion {#tooling-setup-windows-tools-i-use-that-need-adding-to-windows-hyperion}

In July of 2015, this was my process:

I started following these directions: [http://e-spohn.com/blog/2012/08/02/pe-crypters-hyperion/](http://e-spohn.com/blog/2012/08/02/pe-crypters-hyperion/).  
Kali does not have g++ in `/root/.wine/drive_c/MinGW/bin/` but I did not see any point in installing it into wine as it would still have the same issues it had on windows.

I followed the directions on setting up the MinGW compiler to compile hyperion, this did not work, I kept getting errors.  
I made a fix on one of the files ([http://www.gaia-gis.it/spatialite-2.4.0-3/mingw_how_to.html#libgeos](http://www.gaia-gis.it/spatialite-2.4.0-3/mingw_how_to.html#libgeos)), but kept getting more errors, and just ended up copying the Hyperion-1.2 from [http://nullsecurity.net/tools/binary.html](http://nullsecurity.net/tools/binary.html) to the Windows 7 desktop.

I installed MinGW from [http://sourceforge.net/projects/mingw/](http://sourceforge.net/projects/mingw/), but did not end up using it as it had to many errors.  
It was missing a file `libgcc_s_dw2-1.dll` from `C:\MinGW\bin\` so I got this from the archive here: http://sourceforge.net/projects/mingw/files/MinGW/Base/gcc/Version4/Previous%20  
Release%20gcc-4.4.0/ as discussed here: [http://stackoverflow.com/questions/14502080/missing-libgcc-s-dw2-1-dll-error-when-launching-mingw-compiled-exe](http://stackoverflow.com/questions/14502080/missing-libgcc-s-dw2-1-dll-error-when-launching-mingw-compiled-exe). Then after reading this: http:  
//mingw-users.1079350.n2.nabble.com/Question-libgmp-10-dll-not-found-td7443661.html realised that to get hyperion to run, I would be best to copy `libgcc_s_dw2-1.dll` and `libstdc++-6.dll` from `C:\MinGW\bin` to `C:\Users\testaccount\Desktop\Hyperion-1.2`

&nbsp;

Now in 2017 MinGW is installing and running without problem, it should be a simple case of just downloading, checking the MD5 sum, although it is not over HTTPS, Join their IRC channel to confirm the MD5 sum, extract and run `make` and you should have the binary.