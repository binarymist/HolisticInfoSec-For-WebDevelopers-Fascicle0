{backmatter}

# Attributions

## [The 30,000' View](#starting-with-the-30000-foot-view)

### Rating of Threats

**Based on the MicroSoft 6. Rate the Threats**  
[https://msdn.microsoft.com/en-us/library/ff648644.aspx#c03618429_011](https://msdn.microsoft.com/en-us/library/ff648644.aspx#c03618429_011)

**Vulnerability advisories**  
[kitploit.com](http://www.kitploit.com/) did a small [explanation](http://www.kitploit.com/2015/06/the-exploit-database-git-repository.html) of using the [Exploit Database](https://www.exploit-db.com/) from [Offensive Security](https://www.offensive-security.com/)

## [Tooling Setup](#tooling-setup)

**Peter Kim discusses a selection of tools** in "The Hacker Playbook" that he uses regularly. I've included them in this section as they have been found to be very useful. 

**SecLists is a collection of multiple types of lists** used during security assessments. List types include usernames, passwords, URLs, sensitive data grep strings, fuzzing payloads, and many more.

**Install the particular VirtualBox Extension Pack** on to the host:  
[https://www.virtualbox.org/ticket/9511?cversion=0&cnum_hist=2](https://www.virtualbox.org/ticket/9511?cversion=0&cnum_hist=2)

## [Process](#process-and-practises)

**The fact that a WAF is in place** is often given away by simply inspecting the responses from the server side  
[https://pentestlab.wordpress.com/2013/01/13/detecting-web-application-firewalls/](https://pentestlab.wordpress.com/2013/01/13/detecting-web-application-firewalls/)

**To view all of the currently available local nmap scripts**  
[http://cyberpedia.in/nmap-scripting-engine-scanning-in-kali-linux/](http://cyberpedia.in/nmap-scripting-engine-scanning-in-kali-linux/)

**NMap has a couple of good scripts out of the box** for WAF detection  
http-waf-detect: [https://nmap.org/nsedoc/scripts/http-waf-detect.html](https://nmap.org/nsedoc/scripts/http-waf-detect.html)  
http-waf-fingerprint: [https://nmap.org/nsedoc/scripts/http-waf-fingerprint.html](https://nmap.org/nsedoc/scripts/http-waf-fingerprint.html)

**WAFW00F is also an excellent tool**  
[https://github.com/sandrogauci/wafw00f](https://github.com/sandrogauci/wafw00f)

**nslookup** which generally provides less information and uses its own internal libraries as opposed to the OS resolver libraries that dig uses.  
[http://unix.stackexchange.com/questions/93808/dig-vs-nslookup](http://unix.stackexchange.com/questions/93808/dig-vs-nslookup)

**/usr/share/dirbuster/wordlists/directories.jbrofuzz is not great**, but it is not bad either  
[http://null-byte.wonderhowto.com/how-to/hack-like-pro-abusing-dns-for-reconnaissance-0157448/](http://null-byte.wonderhowto.com/how-to/hack-like-pro-abusing-dns-for-reconnaissance-0157448/)

**theHarvester** is a tool for gathering e-mail acconts, subdomain names, virtual hosts, open ports/ banners, and employee names from different public sources (search engines, pgp key servers)  
[https://github.com/laramies/theHarvester](https://github.com/laramies/theHarvester)

**Microsoft bing virtual hosts** search feature  
"Penetration Tester's Open Source Toolkit" book

**I also wrote about a few other vulnerability scanners on my blog**  
[http://blog.binarymist.net/2014/03/29/up-and-running-with-kali-linux-and-friends/#vulnerability-scanners](http://blog.binarymist.net/2014/03/29/up-and-running-with-kali-linux-and-friends/#vulnerability-scanners)

**Docker used LXC** as the default execution environment before the release of version 0.9 on March 13, 2014  
[https://en.wikipedia.org/wiki/Docker_(software)](<https://en.wikipedia.org/wiki/Docker_(software)>)

**firejail**

**Allows a process and all its descendants** to have their own private view of the globally shared kernel resources, such as the network stack, process table, mount table.  
[https://firejail.wordpress.com/](https://firejail.wordpress.com/)

**The source code is on github**  
[https://github.com/netblue30/firejail](https://github.com/netblue30/firejail)

**Pre-built DEB, AUR and RPM packages** are available for download  
[https://firejail.wordpress.com/download-2/](https://firejail.wordpress.com/download-2/)

**Firejail can even run LXC, Docker and OpenVZ containers**  
[https://firejail.wordpress.com/support/frequently-asked-questions/](https://firejail.wordpress.com/support/frequently-asked-questions/)

**Qubes**

**Technically Qubes is not a Linux distribution**, it's closer to being a Xen distro if anything  
[https://www.qubes-os.org/doc/user-faq/#is-qubes-just-another-linux-distribution](https://www.qubes-os.org/doc/user-faq/#is-qubes-just-another-linux-distribution)

**USB stacks and drivers are sand-boxed** in their own unprivileged VM (currently experimental)  
[https://www.qubes-os.org/doc/qubes-architecture/](https://www.qubes-os.org/doc/qubes-architecture/)

**A storage domain has also been considered**  
[https://github.com/QubesOS/qubes-secpack/blob/master/QSBs/qsb-020-2015.txt#L97](https://github.com/QubesOS/qubes-secpack/blob/master/QSBs/qsb-020-2015.txt#L97)

**It provides proper GUI-level** (one of the main goals) isolation  
[http://theinvisiblethings.blogspot.it/2012/09/how-is-qubes-os-different-from.html](http://theinvisiblethings.blogspot.it/2012/09/how-is-qubes-os-different-from.html)

**Along with security** as one of the primary goals of the GUI virtualisation subsystem, performance was also priority so the virtualised applications feel as if they were executed natively  
[https://www.qubes-os.org/doc/user-faq/#is-qubes-just-another-linux-distribution](https://www.qubes-os.org/doc/user-faq/#is-qubes-just-another-linux-distribution)

**Based on monolithic kernels** usually containing tens of millions of lines of code. Most of this code is reachable from untrusted applications via all sorts of APIs, making the attack surface on the kernel huge.  
[http://theinvisiblethings.blogspot.it/2012/09/how-is-qubes-os-different-from.html](http://theinvisiblethings.blogspot.it/2012/09/how-is-qubes-os-different-from.html)

**Support for Windows 8+ is in development**  
[https://www.qubes-os.org/doc/windows-appvms/](https://www.qubes-os.org/doc/windows-appvms/)

**Excellent tools for this task**. There are also many others. Many of which are included in Kali Linux  
[http://tools.kali.org/reporting-tools](http://tools.kali.org/reporting-tools)

**Dradis is included in Kali Linux** and the source code can be accessed from the dradisframework repository  
[https://github.com/dradis/dradisframework](https://github.com/dradis/dradisframework)

**There is a collection of security tools** that Dradis integrates with  
[https://github.com/dradis/dradisframework#some-of-the-features](https://github.com/dradis/dradisframework#some-of-the-features)

**If you look at the public statistics** on businesses loosing value due to being compromised regularly, the figures are staggering  
[http://www.meetup.com/OWASP-New-Zealand-Chapter-Christchurch/events/223462991/](http://www.meetup.com/OWASP-New-Zealand-Chapter-Christchurch/events/223462991/) 

**I have already discussed the test condition workshop** many times on-line  
[http://blog.binarymist.net/2012/03/24/how-to-optimise-your-testing-effort/#planningTheTestEffort](http://blog.binarymist.net/2012/03/24/how-to-optimise-your-testing-effort/#planningTheTestEffort)

**Cost of Change** curve adapted from Scott W. Ambler's article on Examining the Agile Cost of Change Curve, which I've used in many presentations and workshops.  
[http://www.agilemodeling.com/essays/costOfChange.htm](http://www.agilemodeling.com/essays/costOfChange.htm)

**NodeGoat** The purposly vulnerable NodeJS web application  
[https://github.com/OWASP/NodeGoat](https://github.com/OWASP/NodeGoat)

**DBC**  
[http://blog.binarymist.net/2010/10/11/lsp-dbc-and-nets-support/](http://blog.binarymist.net/2010/10/11/lsp-dbc-and-nets-support/)

**Flow** looks to be a good option. Providing consumers with the ability of introducing type checking progressively  
[http://flowtype.org/](http://flowtype.org/)

**Example from the flow website**  
[http://flowtype.org/](http://flowtype.org/)

**Essentials for Creating and Maintaining a High Performance Development Team**  
[http://blog.binarymist.net/2014/01/25/essentials-for-creating-and-maintaining-a-high-performance-development-team/](http://blog.binarymist.net/2014/01/25/essentials-for-creating-and-maintaining-a-high-performance-development-team/)

**I really liked what Moxie Marlinspike** said on the topic of career advice  
[http://www.thoughtcrime.org/blog/career-advice/](http://www.thoughtcrime.org/blog/career-advice/)

## [Physical](#physical)

**There are competitions devoted** to reassembling shredded printed documents with contestants that have successfully reassembled all printed matter:  
[http://archive.darpa.mil/shredderchallenge/](http://archive.darpa.mil/shredderchallenge/)

**Not all paper shredders** are created equal. Understand the pros and cons:  
[https://en.wikipedia.org/wiki/Paper_shredder](https://en.wikipedia.org/wiki/Paper_shredder)

**Detection works where prevention fails** and detection is of no use without response.  
Beyond Fear by Bruce Schneier

**Remove or degausse MFD hard drives** before the device leaves premise at end of life / lease  
[http://blog.binarymist.net/2013/03/17/erasing-data-from-your-drives/](http://blog.binarymist.net/2013/03/17/erasing-data-from-your-drives/)

**You will also need to think about company culture** and whether this needs some work:  
[http://blog.binarymist.net/2014/04/26/culture-in-the-work-place/](http://blog.binarymist.net/2014/04/26/culture-in-the-work-place/)

**The speed benefits** of Li-Fi are also compelling, many times faster than current Wi-Fi speeds.  
[http://www.sciencealert.com/li-fi-tested-in-the-real-world-for-the-first-time-is-100-times-faster-than-wi-fi](http://www.sciencealert.com/li-fi-tested-in-the-real-world-for-the-first-time-is-100-times-faster-than-wi-fi)

**People can be your strongest or your weakest defence**. This is your choice. Cultural change can be implemented from any level. The most successfully being from the shop flaw:  
[http://blog.binarymist.net/2014/04/26/culture-in-the-work-place/](http://blog.binarymist.net/2014/04/26/culture-in-the-work-place/)

## [People](#people)

**The Arxan 5th Annual State of Application Security Report**  
[https://www.arxan.com/resources/state-of-application-security/](https://www.arxan.com/resources/state-of-application-security/) _Perception vs. Reality_

**The content Michael Bazzell has collated**  
[https://inteltechniques.com/links.html](https://inteltechniques.com/links.html) and his excellent books on the gathering of Open Source Intelligence.

**Studies show that motivation** has a larger effect on productivity and quality than any other factor. Software Engineering Economics by Barry W. Boehm 1981.

**Increase software developer productivity** on my blog  
[http://blog.binarymist.net/2013/03/02/how-to-increase-software-developer-productivity/](http://blog.binarymist.net/2013/03/02/how-to-increase-software-developer-productivity/)

**Those distracted by incoming email** and phone calls saw a 10-point fall in their IQ by BBC:  
[http://news.bbc.co.uk/2/hi/uk_news/4471607.stm](http://news.bbc.co.uk/2/hi/uk_news/4471607.stm)

**Gerald Weinberg's rule** that 20% of our time is lost every time we perform a context switch. This is from "Quality Software Management: Systems Thinking" by Gerald Weinberg.

**Who's your Daddy (WyD)** Another password profiling tool that "_extracts words/strings from supplied files and directories. It parses files according to the file-types and extracts the useful information, e.g. song titles, authors and so on from mp3's or descriptions and titles from images._"  
"_It supports the following filetypes: plain, html, php, doc, ppt, mp3, pdf, jpeg, odp/ods/odp and extracting raw strings._"  
[http://www.remote-exploit.org/articles/misc_research__amp_code/index.html](http://www.remote-exploit.org/articles/misc_research__amp_code/index.html)

**"_CeWL is a ruby app which spiders a given url_** _to a specified depth, optionally following external links, and returns a list of words which can then be used for password crackers_"  
[https://digi.ninja/projects/cewl.php](https://digi.ninja/projects/cewl.php)

**Hydra** has many other options. Plenty of good documentation out there.  
[http://null-byte.wonderhowto.com/how-to/hack-like-pro-crack-online-web-form-passwords-with-thc-hydra-burp-suite-0160643/](http://null-byte.wonderhowto.com/how-to/hack-like-pro-crack-online-web-form-passwords-with-thc-hydra-burp-suite-0160643/)

**You can DIY** with the likes of Asterisk  
[http://www.social-engineer.org/wiki/archives/CallerIDspoofing/CallerID-SpoofingWithAsterisk.html](http://www.social-engineer.org/wiki/archives/CallerIDspoofing/CallerID-SpoofingWithAsterisk.html)

**The government that imprisoned Kevin Mitnick** for nearly five years, later sought his advice about how to keep its own networks safe from intruders  
[http://www.politechbot.com/p-00969.html](http://www.politechbot.com/p-00969.html) 

**Open source framework** providing all the tools anyone would need to spoof caller Ids and much more  
[http://www.social-engineer.org/wiki/archives/CallerIDspoofing/CallerID-SpoofingWithAsterisk.html](http://www.social-engineer.org/wiki/archives/CallerIDspoofing/CallerID-SpoofingWithAsterisk.html)

**Some services can not handle return messages though**, unless the attacker has physical access to a phone that would contact the targets phone (as with flexispy)  
[http://blog.flexispy.com/spoof-sms-powerful-secret-weapon-shouldve-using/](http://blog.flexispy.com/spoof-sms-powerful-secret-weapon-shouldve-using/)

**SMS spoofing was removed from the social engineering toolkit** in version 6.0 due to lack of maintenance  
[https://github.com/trustedsec/social-engineer-toolkit/blob/master/readme/CHANGES#L285](https://github.com/trustedsec/social-engineer-toolkit/blob/master/readme/CHANGES#L285)

**Episode 5 of Mr Robot**  
[http://www.usanetwork.com/mrrobot/episode-guide/season-1-episode-5-eps143xpl0itswmv](http://www.usanetwork.com/mrrobot/episode-guide/season-1-episode-5-eps143xpl0itswmv)  
Elliot was making his way through the so called impenetrable storage facility (Steel Mountain) to plant a Raspberry Pi on the network

**His colleagues diverted the manager** that was escorting him out of the building by sending her a spoofed SMS message  
[http://null-byte.wonderhowto.com/how-to/hacks-mr-robot-send-spoofed-sms-text-message-0163331/](http://null-byte.wonderhowto.com/how-to/hacks-mr-robot-send-spoofed-sms-text-message-0163331/)

**SMiShing attacks are on the rise**  
[http://www.pcworld.com/article/254979/smishing_attacks_are_on_the_rise.html](http://www.pcworld.com/article/254979/smishing_attacks_are_on_the_rise.html)

**Also noted in the Arxan report that 80% of consumers would change providers** if they knew about the vulnerabilities and had a better option. 90% of the app execs (the creators) also believed that consumers would switch if they knew and better offerings were available.  
[https://www.arxan.com/wp-content/uploads/2016/01/State_of_Application_Security_2016_Consolidated_Report.pdf](https://www.arxan.com/wp-content/uploads/2016/01/State_of_Application_Security_2016_Consolidated_Report.pdf)

**What you need to do** is be aware of how much productivity is killed with each switch. Then do everything in your power to make sure your Development Team is sheltered from as much as possible. [How to Increase Software Developer Productivity](http://blog.binarymist.net/2013/03/02/how-to-increase-software-developer-productivity/#context-switching) by Kim Carter.

**The Multi-Tasking Myth** by Jeff Atwood:  
[http://blog.codinghorror.com/the-multi-tasking-myth/](http://blog.codinghorror.com/the-multi-tasking-myth/)

**The trick here** is that when you manage programmers, specifically, task switches take a really, really, really long time. [Human Task Switches Considered Harmful](http://www.joelonsoftware.com/articles/fog0000000022.html).

**Get yourself a OneRNG** for generating true randomness  
[http://onerng.info/](http://onerng.info/)

**Michael Bazzell has an excellent collection of tools** to assist with validating phone numbers under the "Telephone Numbers" heading at his website  
[https://inteltechniques.com/links.html](https://inteltechniques.com/links.html)

**Michael also has a simple tool** under the "Telephone Number" heading on the left  
[https://inteltechniques.com/intel/menu.html](https://inteltechniques.com/intel/menu.html)  
which leverage's a collection of phone number search API's.

**Services such that**

* Wombatsecurity  
  [https://www.wombatsecurity.com/](https://www.wombatsecurity.com/)
* LUCY  
  [http://phishing-server.com/](http://phishing-server.com/)  
* and others provide,

automate the above training techniques

**Retrospective** is a good time and place to raise the awareness and make sure change occurs  
[http://blog.binarymist.net/2012/07/28/guidance-on-running-scrum-retrospectives/](http://blog.binarymist.net/2012/07/28/guidance-on-running-scrum-retrospectives/) 

