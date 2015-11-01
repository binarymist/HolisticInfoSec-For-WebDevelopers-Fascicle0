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

## [Process](#process)







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



**If you look at the public statistics** on businesses loosing value due to being compromised regularly, the figures are staggering  
[http://www.meetup.com/OWASP-New-Zealand-Chapter-Christchurch/events/223462991/](http://www.meetup.com/OWASP-New-Zealand-Chapter-Christchurch/events/223462991/) 

**Cost of Change** curve adapted from Scott W. Ambler's article on Examining the Agile Cost of Change Curve, which I've used in many presentations and workshops.  
[http://www.agilemodeling.com/essays/costOfChange.htm](http://www.agilemodeling.com/essays/costOfChange.htm)




## [Physical](#physical)

**There are competitions devoted** to reassembling shredded printed documents with contestants that have successfully reassembled all printed matter:  
[http://archive.darpa.mil/shredderchallenge/](http://archive.darpa.mil/shredderchallenge/)

**Not all paper shredders** are created equal. Understand the pros and cons:  
[https://en.wikipedia.org/wiki/Paper_shredder](https://en.wikipedia.org/wiki/Paper_shredder)

**Detection works where prevention fails** and detection is of no use without response.  
Beyond Fear by Bruce Schneier

**You will also need to think about company culture** and whether this needs some work:  
[http://blog.binarymist.net/2014/04/26/culture-in-the-work-place/](http://blog.binarymist.net/2014/04/26/culture-in-the-work-place/)

**People can be your strongest or your weakest defence**. This is your choice. Cultural change can be implemented from any level. The most successfully being from the shop flaw:  
[http://blog.binarymist.net/2014/04/26/culture-in-the-work-place/](http://blog.binarymist.net/2014/04/26/culture-in-the-work-place/)

## [IoT](#iot)

 
 

## [Mobile](#mobile)

**Sells cell phone tracking systems** to both corporations and governments worldwide.
Bruce Schneier: Data and Goliath: Introduction

**Locate. Track. Manipulate**:  
[http://www.washingtonpost.com/business/technology/for-sale-systems-that-can-secretly-track-where-cellphone-users-go-around-the-globe/2014/08/24/f0700e8a-f003-11e3-bf76-447a5df6411f_story.html](http://www.washingtonpost.com/business/technology/for-sale-systems-that-can-secretly-track-where-cellphone-users-go-around-the-globe/2014/08/24/f0700e8a-f003-11e3-bf76-447a5df6411f_story.html)

**SkyLock not only finds people**:  
[http://apps.washingtonpost.com/g/page/business/skylock-product-description-2013/1276/](http://apps.washingtonpost.com/g/page/business/skylock-product-description-2013/1276/)

**UK company Cobham sells a system** that allows someone to send a "blind" call to a phone. Bruce Schneier: Data and Goliath: Introduction

**The blind call forces the phone** to transmit on a certain frequency
Bruce Schneier: Data and Goliath: Introduction. Also [blog posted](https://www.schneier.com/blog/archives/2014/12/nsa_hacking_of_.html).

**Infiltrator**  
The global real-time [tracking system](http://infiltrator.mobi/defentek_infiltrator_real-time_global_tracking_technologies.html).
Also discussed in Bruce Schneier: Data and Goliath: Introduction. Bruce Schneier's [blog post](https://www.schneier.com/blog/archives/2014/12/nsa_hacking_of_.html) and the [Washington Post](http://www.washingtonpost.com/business/technology/for-sale-systems-that-can-secretly-track-where-cellphone-users-go-around-the-globe/2014/08/24/f0700e8a-f003-11e3-bf76-447a5df6411f_story.html).
The website also claims: "*It is a strategic solution that infiltrates and is undetected and unknown by the network, carrier, or the target.*"

**Many of the applications installed on our phones** are [collecting your personal information](http://www.nytimes.com/2012/10/29/technology/mobile-apps-have-a-ravenous-ability-to-collect-personal-data.html)

**Locate mobile phones using SS7**  
Initial findings from Bruce Schneier: Data and Goliath: Introduction. Other resources found:
Tobias Engel's presentation at the [25th Chaos Communication Congress](http://events.ccc.de/congress/2008/Fahrplan/events/2997.en.html)  
Tobias Engel's [slide-deck](http://berlin.ccc.de/~tobias/25c3-locating-mobile-phones.pdf)  
Wikipedia entry for [Signalling System No. 7](http://en.wikipedia.org/wiki/Signalling_System_No._7)  
Tobias Engel's [video of his presentation](https://www.youtube.com/watch?v=lQ0I5tl0YLY)  
Nicholas Skiller's [implementation of Tobias Engel's research](http://www.ns-tech.co.uk/products/track-any-mobile/)

**Many offerings available to allow people to spy** on other individuals activities and location in regards to mobile phones.  
News story discussed in Bruce Schneier: Data and Goliath: Introduction. Along with [hellospy](http://hellospy.com/homepage.aspx?lang=en-US) Also [reported on](http://www.washingtonpost.com/business/technology/make-of-app-used-for-spying-indicted-in-virginia/2014/09/29/816b45b8-4805-11e4-a046-120a8a855cca_story.html) by Craig Timberg and Matt Zapotosky.

**Use location data to track people**  
Bruce Schneier: Data and Goliath: Introduction: USA National Security Agency (NSA) and its UK counterpart, Government Communications Headquarters (GCHQ), use location data to track people:  
[http://www.washingtonpost.com/world/national-security/nsa-tracking-cellphone-locations-worldwide-snowden-documents-show/2013/12/04/5492873a-5cf2-11e3-bc56-c6ca94801fac_story.html](http://www.washingtonpost.com/world/national-security/nsa-tracking-cellphone-locations-worldwide-snowden-documents-show/2013/12/04/5492873a-5cf2-11e3-bc56-c6ca94801fac_story.html)  
[http://www.washingtonpost.com/blogs/the-switch/wp/2013/12/10/new-documents-show-how-the-nsa-infers-relationships-based-on-mobile-location-data](http://www.washingtonpost.com/blogs/the-switch/wp/2013/12/10/new-documents-show-how-the-nsa-infers-relationships-based-on-mobile-location-data)


## [People](#people)

**Studies show that motivation** has a larger effect on productivity and quality than any other factor. Software Engineering Economics by Barry W. Boehm 1981.

**Those distracted by incoming email** and phone calls saw a 10-point fall in their IQ by BBC:  
[http://news.bbc.co.uk/2/hi/uk_news/4471607.stm](http://news.bbc.co.uk/2/hi/uk_news/4471607.stm)

**Gerald Weinberg's rule** that 20% of our time is lost every time we perform a context switch. This is from "Quality Software Management: Systems Thinking" by Gerald Weinberg.

**The Multi-Tasking Myth** by Jeff Atwood:  
[http://blog.codinghorror.com/the-multi-tasking-myth/](http://blog.codinghorror.com/the-multi-tasking-myth/)

**What you need to do** is be aware of how much productivity is killed with each switch. Then do everything in your power to make sure your Development Team is sheltered from as much as possible. [How to Increase Software Developer Productivity](http://blog.binarymist.net/2013/03/02/how-to-increase-software-developer-productivity/#context-switching) by Kim Carter.

**The trick here** is that when you manage programmers, specifically, task switches take a really, really, really long time. [Human Task Switches Considered Harmful](http://www.joelonsoftware.com/articles/fog0000000022.html).

## [Cloud](#cloud)


## [VPS](#vps)

**Any attacker worth their weight** will try to cover their tracks as they progress:  
[http://www.win.tue.nl/~aeb/linux/hh/hh-13.html](http://www.win.tue.nl/~aeb/linux/hh/hh-13.html)

**Taking things further**, an attacker may load a kernel module that modifies the `readdir()` call:  
[http://pubs.opengroup.org/onlinepubs/9699919799/functions/readdir.html](http://pubs.opengroup.org/onlinepubs/9699919799/functions/readdir.html)

**PSExec is a telnet replacement** also providing full interactivity for console applications. Check out the [home page](https://technet.microsoft.com/en-us/sysinternals/bb897553.aspx).

**The PSExec utility requires** a few things on the remote system. [Details on rapid7](https://community.rapid7.com/community/metasploit/blog/2013/03/09/psexec-demystified).

**The PSExec executable** has a Windows Service image inside which it deploys to the Admin$ share on the target machine:  
[https://community.rapid7.com/community/metasploit/blog/2013/03/09/psexec-demystified](https://community.rapid7.com/community/metasploit/blog/2013/03/09/psexec-demystified).

## [Network](#network)

**If the victim's SMTP server does not perform reverse [lookups on the hostname](http://www.social-engineer.org/framework/se-tools/computer-based/social-engineer-toolkit-set/)**, an email `from` and `reply-to` fields can be successfully spoofed.

**Doppelganger Domains** An old trick brought back to light by Peter Kim's [research](http://www.wired.com/2011/09/doppelganger-domains/) involving fortune 500 companies where they intercepted 20 GB of email from miss typed addresses.  
Peter Kim discusses in "The Hacker PlayBook" about how he set-up SMTP and SSH doppelganger domains. This is an excellent book that I recommend reading.

**All of the CAs now use intermediate certificates** to sign your certificate, so that they can keep their root certificate off line. Similar to what I did with GPG in my blog post  
[http://blog.binarymist.net/2015/01/31/gnupg-key-pair-with-sub-keys/#master-key-pair-generation](http://blog.binarymist.net/2015/01/31/gnupg-key-pair-with-sub-keys/#master-key-pair-generation).

**This URL is known as the Certification Revocation List (CRL)** distribution point.  
[http://en.wikipedia.org/wiki/Revocation_list](http://en.wikipedia.org/wiki/Revocation_list)  

**The next stage of the evolution was Online Certificate Status Protocol (OCSP)**  
[http://en.wikipedia.org/wiki/Online_Certificate_Status_Protocol](http://en.wikipedia.org/wiki/Online_Certificate_Status_Protocol)  
which came about in 1998  
[http://tools.ietf.org/html/rfc6960#page-31](http://tools.ietf.org/html/rfc6960#page-31).

**pinning** [http://en.wikipedia.org/wiki/HTTP_Public_Key_Pinning](http://en.wikipedia.org/wiki/HTTP_Public_Key_Pinning)

**You can read more about pinning** on the OWASP Certificate and Public Key Pinning page  
[https://www.owasp.org/index.php/Certificate_and_Public_Key_Pinning](https://www.owasp.org/index.php/Certificate_and_Public_Key_Pinning)  
and also the specification  
[https://tools.ietf.org/html/draft-ietf-websec-key-pinning-21](https://tools.ietf.org/html/draft-ietf-websec-key-pinning-21).

**Details of what An OCSP request should look like** can be seen in 2.1 of the OCSP specification  
[http://tools.ietf.org/html/rfc6960#section-2.1](http://tools.ietf.org/html/rfc6960#section-2.1).

**Details of what the OCSP response will look like** can be seen in 2.2 of the OCSP specification  
[http://tools.ietf.org/html/rfc6960#section-2.2](http://tools.ietf.org/html/rfc6960#section-2.2).

**OCSP Stapling**  
[http://en.wikipedia.org/wiki/OCSP_stapling](http://en.wikipedia.org/wiki/OCSP_stapling)

**You can read the specification for OCSP stapling** officially known as the TLS "Certificate Status Request" extension in the TLS Extensions: Extensions Definitions  
[http://tools.ietf.org/html/rfc6066#section-8](http://tools.ietf.org/html/rfc6066#section-8).

**digicert** [https://www.digicert.com/help/](https://www.digicert.com/help/)

**ssl labs** [https://www.ssllabs.com/ssltest/](https://www.ssllabs.com/ssltest/)

**OCSP Must-Staple** [https://wiki.mozilla.org/CA:ImprovingRevocation](https://wiki.mozilla.org/CA:ImprovingRevocation)

**X.509 Certificate Revocation Evolution**  
[https://www.grc.com/revocation/ocsp-must-staple.htm](https://www.grc.com/revocation/ocsp-must-staple.htm)  
[http://twit.cachefly.net/audio/sn/sn0453/sn0453.mp3](http://twit.cachefly.net/audio/sn/sn0453/sn0453.mp3)  
[https://www.grc.com/sn/sn-453-notes.pdf](https://www.grc.com/sn/sn-453-notes.pdf)  
[https://www.grc.com/sn/sn-453.htm](https://www.grc.com/sn/sn-453.htm)

## [Web Applications](#web-applications)

**Before the breach**, the company boasted about airtight data security but ironically, still proudly displays a graphic with the phrase “trusted security award” on its homepage.
[Dark Reading](http://www.darkreading.com/operations/what-ashley-madison-can-teach-the-rest-of-us-about-data-security-/a/d-id/1322129)

**Other notable data-store compromises were LinkedIn** with 6.5 million user accounts compromised and 95% of the users passwords cracked in days. Why so fast? Because they used simple hashing, specifically SHA-1. Details provided [here](http://securitynirvana.blogspot.co.nz/2012/06/final-word-on-linkedin-leak.html) on the findings.

**EBay with 145 million active buyers** had a small number of employee log-in credentials [compromised](http://www.darkreading.com/attacks-breaches/ebay-database-hacked-with-stolen-employee-credentials-/d/d-id/1269093) allowing unauthorised access to eBay's corporate network.

**the winston-syslog-posix package** was inspired by blargh  
[https://www.npmjs.com/package/winston-syslog-posix](https://www.npmjs.com/package/winston-syslog-posix)  
[http://tmont.com/blargh/2013/12/writing-to-the-syslog-with-winston](http://tmont.com/blargh/2013/12/writing-to-the-syslog-with-winston)

**There were also some other options** for those using Papertrail as their off-site syslog and aggregation PaaS:  
[http://help.papertrailapp.com/kb/configuration/configuring-centralized-logging-from-nodejs-apps/](http://help.papertrailapp.com/kb/configuration/configuring-centralized-logging-from-nodejs-apps/)

**Excellent resource for dealing with user input** based on the execution contexts that it passes through  
[https://www.owasp.org/index.php/XSS_%28Cross_Site_Scripting%29_Prevention_Cheat_Sheet](https://www.owasp.org/index.php/XSS_%28Cross_Site_Scripting%29_Prevention_Cheat_Sheet)

**Hackers halfway across the world** _might know your password, but they don't know who your friends are_  
[https://m.facebook.com/story.php?story_fbid=191422450875446&id=121897834504447](https://m.facebook.com/story.php?story_fbid=191422450875446&id=121897834504447)

**helping to digitise text** for The New York Times and Google Books  
[https://en.wikipedia.org/wiki/ReCAPTCHA](https://en.wikipedia.org/wiki/ReCAPTCHA)

**Disqus tracks users activities** from hosting website to website whether you have an account, are are logged in or not.  
[http://perltricks.com/article/104/2014/7/29/Your-users-deserve-better-than-Disqus](http://perltricks.com/article/104/2014/7/29/Your-users-deserve-better-than-Disqus)

**Any information they collect** such as IP address, web browser details, installed add-ons, referring pages and exit links may be disclosed to any third party.  
[https://en.wikipedia.org/wiki/Disqus#Criticism_and_privacy_concerns](https://en.wikipedia.org/wiki/Disqus#Criticism_and_privacy_concerns)

**His (Matt Mullenweg) first attempt** was a JavaScript plugin which modified the comment form and hid fields, but within hours of launching it, spammers downloaded it, figured out how it worked, and bypassed it. This is a common pitfall for anti-spam plugins: once they get traction  
[https://en.wikipedia.org/wiki/Akismet](https://en.wikipedia.org/wiki/Akismet)


**Given the fact that many clients count on conversions to make money**, _not receiving 3.2% of those conversions could put a dent in sales.  Personally, I would rather sort through a few SPAM conversions instead of losing out on possible income._  
[https://moz.com/blog/captchas-affect-on-conversion-rates](https://moz.com/blog/captchas-affect-on-conversion-rates)

**Spam is not the user’s problem;** _it is the problem of the business that is providing the website. It is arrogant and lazy to try and push the problem onto a website’s visitors._  
[http://timkadlec.com/2011/01/death-to-captchas/](http://timkadlec.com/2011/01/death-to-captchas/)

**According to studies**, captchas just do not cut it  
[http://www.smashingmagazine.com/2011/03/in-search-of-the-perfect-captcha/](http://www.smashingmagazine.com/2011/03/in-search-of-the-perfect-captcha/)

**If you have some CSS that hides a form field** and especially if the CSS is not inline on the same page, they will usually fail at realising that the field is not supposed to be visible.  
[http://haacked.com/archive/2007/09/11/honeypot-captcha.aspx/](http://haacked.com/archive/2007/09/11/honeypot-captcha.aspx/)

**The Offensive Web Testing Framework (OWTF)** also has a [plugin](https://github.com/owtf/owtf/wiki/Listing-Plugins) for testing captchas. While you are at it. Check out the [OWTF](https://www.owasp.org/index.php/OWASP_OWTF#tab=Main). It's a very useful tool for penetration testers and developers testing their own work. Focussed on making the process of penetration testing efficient with time. The main documentation is [here](http://docs.owtf.org/en/latest/).

**The function used to protect stored credentials** should balance attacker and defender verification. The defender needs an acceptable response time for verification of users’ credentials during peak use. However, the time required to map `<credential> -> <protected form>` must remain beyond threats’ hardware (GPU, FPGA) and technique (dictionary-based, brute force, etc) capabilities:  
[https://www.owasp.org/index.php/Password_Storage_Cheat_Sheet#Impose_infeasible_verification_on_attacker](https://www.owasp.org/index.php/Password_Storage_Cheat_Sheet#Impose_infeasible_verification_on_attacker)

**You may read in many places** that having data-store passwords and other types of secrets in configuration files in clear text is an insecurity that [must be addressed](https://www.owasp.org/index.php/Password_Plaintext_Storage).

**Encrypt sections** of a web, executable, machine-level, application-level or configuration files with Aspnet_regiis.exe:  
[SQL Authentication](https://msdn.microsoft.com/en-us/library/ff648340.aspx)  
[Windows Authentication](https://msdn.microsoft.com/en-us/library/ff647396.aspx)

"**vSentry protects desktops without requiring patches or updates**_, defeating and automatically discarding all known and unknown malware, and eliminating the need for costly remediation._":  
[https://www.google.co.nz/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0CBsQFjAAahUKEwj4h8br2ofIAhULpJQKHdE1Cfg&url=http%3A%2F%2Fwww.bromium.com%2Fsites%2Fdefault%2Ffiles%2FBromium-Datasheet-vSentry.pdf&usg=AFQjCNHj08aV26awuSnIc9rQiHf6ER9ANQ&sig2=ynekdS3BwHgB9_8WBfGzTw&bvm=bv.103073922,d.dGo](https://www.google.co.nz/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0CBsQFjAAahUKEwj4h8br2ofIAhULpJQKHdE1Cfg&url=http%3A%2F%2Fwww.bromium.com%2Fsites%2Fdefault%2Ffiles%2FBromium-Datasheet-vSentry.pdf&usg=AFQjCNHj08aV26awuSnIc9rQiHf6ER9ANQ&sig2=ynekdS3BwHgB9_8WBfGzTw&bvm=bv.103073922,d.dGo)

**Every user** [**task is isolated**](http://security.stackexchange.com/questions/23674/how-will-microvirtualisation-change-the-security-field-if-at-all) into its own micro-VM.

"**vSentry empowers users** _to access whatever information they need from any network, application or website, without risk to the enterprise_"

"_Traditional security solutions rely on detection and often fail to block targeted attacks which use unknown “zero day” exploits. Bromium uses hardware enforced isolation to stop even “undetectable” attacks without disrupting the user._"

[Bromium](http://www.bromium.com/sites/default/files/Bromium-Datasheet-vSentry.pdf)

"**With Bromium micro-virtualization**_, we now have an answer: A desktop that is utterly secure and a joy to use_"

[Bromium](http://www.ervik.as/virtualization-community/groups/display?categoryid=11)

[Remind your customers](https://speakerdeck.com/binarymist/passwords-lol) to **always use unique passwords** that are made up of alphanumeric, upper-case, lower-case and special characters.

[**scrypt**](http://www.tarsnap.com/scrypt.html)

**bcrypt which uses the Eksblowfish cipher** which was designed specifically for bcrypt from the blowfish cipher, to be very slow to initiate thus boosting protection against dictionary attacks which were often run on custom Application-specific Integrated Circuits (ASICs) with [low gate counts](http://security.stackexchange.com/questions/4781/do-any-security-experts-recommend-bcrypt-for-password-storage)

**far greater memory required** for each hash, small and frequent pseudo-random memory accesses, making it harder to cache the data into faster memory.
[http://openwall.info/wiki/john/GPU/bcrypt](http://openwall.info/wiki/john/GPU/bcrypt)

**bcrypt brute-forcing** is becoming more accessible due to easily obtainable cheap hardware.
[http://www.openwall.com/lists/announce/2013/12/03/1](http://www.openwall.com/lists/announce/2013/12/03/1)  
[http://www.openwall.com/presentations/Passwords13-Energy-Efficient-Cracking/](http://www.openwall.com/presentations/Passwords13-Energy-Efficient-Cracking/)

"**Using four AMD Radeon HD6990 graphics cards**, _I am able to make about 15.5 billion guesses per second using the SHA-1 algorithm._"
[Per Thorsheim](http://securitynirvana.blogspot.co.nz/2012/06/final-word-on-linkedin-leak.html)

**Dibbe Edwards discusses** some excellent initiatives on how they do it at IBM  
[https://soundcloud.com/owasp-podcast/dibbe-edwards-devops-and-open-source-at-ibm](https://soundcloud.com/owasp-podcast/dibbe-edwards-devops-and-open-source-at-ibm)

**There is an excellent paper by the SANS Institute** on Security Concerns in Using Open Source Software for Enterprise Requirements that is well worth a read. It confirms what the likes of IBM are doing in regards to their consumption of free and open source libraries:  
[http://www.sans.org/reading-room/whitepapers/awareness/security-concerns-open-source-software-enterprise-requirements-1305](http://www.sans.org/reading-room/whitepapers/awareness/security-concerns-open-source-software-enterprise-requirements-1305)

**As a developer, you are responsible** for what you install and consume:  
[https://blog.liftsecurity.io/2015/01/27/a-malicious-module-on-npm#you-are-responsible-for-what-you-require-](https://blog.liftsecurity.io/2015/01/27/a-malicious-module-on-npm#you-are-responsible-for-what-you-require-)

**The** [**official way**](https://github.com/nodesource/distributions) **to install NodeJS.** Do not do this.  

**Check to see if any package has hooks** that will run scripts:  
[https://blog.liftsecurity.io/2015/01/27/a-malicious-module-on-npm#inspect-the-source-before-you-npm-install-it](https://blog.liftsecurity.io/2015/01/27/a-malicious-module-on-npm#inspect-the-source-before-you-npm-install-it)

**Can define scripts to be run** on specific NPM hooks:  
[https://docs.npmjs.com/misc/scripts](https://docs.npmjs.com/misc/scripts)

**People often miss-type** what they want to install:  
[https://blog.liftsecurity.io/2015/01/27/a-malicious-module-on-npm#make-sure-you-re-installing-the-right-thing](https://blog.liftsecurity.io/2015/01/27/a-malicious-module-on-npm#make-sure-you-re-installing-the-right-thing)

**For NodeJS developers**: Keep your eye on the [nodesecurity advisories](https://nodesecurity.io/advisories)

**There is an NPM package** that can help us with this called `precommit-hook` which installs the git pre-commit:  
[https://www.npmjs.com/package/precommit-hook](https://www.npmjs.com/package/precommit-hook)

**To install RetireJS locally to your project** and run as a git precommit-hook  
[https://blog.andyet.com/2014/11/19/managing-code-changes](https://blog.andyet.com/2014/11/19/managing-code-changes)

**RequireSafe provides** "_intentful auditing as a stream of intel for bithound_":  
[https://blog.liftsecurity.io/2015/02/10/introducing-requiresafe-peace-of-mind-third-party-node-modules](https://blog.liftsecurity.io/2015/02/10/introducing-requiresafe-peace-of-mind-third-party-node-modules)

