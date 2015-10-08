# Additional Resources {#additional-resources}

* [MS Security Development Lifecycle](http://www.microsoft.com/en-us/SDL/process/design.aspx) is actually pretty good.
* Although not specifically referred to in this model, the [Building Security In Maturity Model (BSIMM)](https://www.bsimm.com/online/) resources are very helpful. They don't really tell you how to implement secure software, but they do provide scales to measure the maturity of your development processes on many dimensions.

## [Tooling Setup](#tooling-setup)

[**Kali Linux package version tracking**](http://tools.kali.org/version-tracking) is useful for seeing what versions of tools are currently in the distribution.

**TP-LINK TL-WN722N USB Wireless Adapter**

1. ath9k_htc [Debian Module](https://wiki.debian.org/ath9k_htc)
2. VirtualBox [information](https://www.virtualbox.org/ticket/9511?cversion=0&cnum_hist=2) around setting up the TL-WN722N
3. TP-LINK TL-WN722N [wiki](https://wikidevi.com/wiki/TP-LINK_TL-WN722N)
4. [Loading and unloading](http://docs.oracle.com/cd/E37670_01/E41138/html/ch06s03.html) Linux Kernel Modules
5. Kernel Module [Blacklisting](https://wiki.debian.org/KernelModuleBlacklisting)

## [People](#people)

**Morale, Productivity and Engagement Killers**: An excellent resource around motivation can be found in Chapter 11 of Steve McConnell's excellent "Rapid Development" book.

**Presentation I performed at AgileNZ 2014** around [increasing developer productivity](https://speakerdeck.com/binarymist/how-to-increase-software-developer-productivity)

[**Social Engineer**](http://www.social-engineer.org/) website has a plethora of excellent resources.

The following books have been influential for me and the content from the People chapter should reflect that. They are very insightful and well worth the investment.

* The Social Engineers Playbook: A Practical Guide to Pretexting by Jeremiah Talamantes
* Social Engineering: The Art of Human Hacking by Christopher Hadnagy
* The Art Of Deception by Kevin Mitnick
* Chapter 10 of Beyond Fear by Bruce Schneier
* Gray Hat Hacking The Ethical Hacker's Handbook

## [VPS](#vps)

[**Details**](https://community.rapid7.com/community/metasploit/blog/2013/03/09/psexec-demystified) on the Metasploit PSExec module.

[**Distributed Computing Environment / Remote Procedure Call**](https://en.wikipedia.org/wiki/DCE/RPC).

## [Web Applications](#web-applications)

[**Essentials for Creating and Maintaining a High Performance Development Team**](http://blog.binarymist.net/2014/01/25/essentials-for-creating-and-maintaining-a-high-performance-development-team/)

[**How to Increase Software Developer Productivity**](https://speakerdeck.com/binarymist/how-to-increase-software-developer-productivity)

**Pair Programming Metrics** from Fog Creek  
[http://discuss.fogcreek.com/joelonsoftware1/default.asp?cmd=show&ixPost=33575](http://discuss.fogcreek.com/joelonsoftware1/default.asp?cmd=show&ixPost=33575)

**Details that helped setup NodeJS logging**:  
[https://gist.github.com/rtgibbons/7354879](https://gist.github.com/rtgibbons/7354879)  
[https://thejsf.wordpress.com/2015/01/18/node-js-logging-with-winston/](https://thejsf.wordpress.com/2015/01/18/node-js-logging-with-winston/)

**Application logging to syslog server** on another machine:  
[http://unix.stackexchange.com/questions/67250/where-does-rsyslog-keep-facility-local0](http://unix.stackexchange.com/questions/67250/where-does-rsyslog-keep-facility-local0)  
[http://wiki.rsyslog.com/index.php/Very_simple_config_--_starting_point_for_modifications](http://wiki.rsyslog.com/index.php/Very_simple_config_--_starting_point_for_modifications)

**Or the new style configuration**:  
[http://www.rsyslog.com/doc/v8-stable/configuration/modules/imudp.html](http://www.rsyslog.com/doc/v8-stable/configuration/modules/imudp.html)

**Syslog compatible protocol severities**:  
[https://wiki.gentoo.org/wiki/Rsyslog#Severity](https://wiki.gentoo.org/wiki/Rsyslog#Severity)

**Monit is an excellent tool** for the dark cockpit approach:  
[http://blog.binarymist.net/2015/06/27/keeping-your-nodejs-web-app-running-on-production-linux/#monit](http://blog.binarymist.net/2015/06/27/keeping-your-nodejs-web-app-running-on-production-linux/#monit)

**I have personally had excellent success** with Monit:  
[http://blog.binarymist.net/2015/06/27/keeping-your-nodejs-web-app-running-on-production-linux/#getting-started-with-monit](http://blog.binarymist.net/2015/06/27/keeping-your-nodejs-web-app-running-on-production-linux/#getting-started-with-monit)

**statsd source code**:  
[https://github.com/etsy/statsd/](https://github.com/etsy/statsd/)

**One of the ways we can generate statistics for our statsd daemon** is by using one of the many language specific statsd clients  
[https://github.com/etsy/statsd/wiki#client-implementations](https://github.com/etsy/statsd/wiki#client-implementations)

**First statsd spec for metric types**:  
[https://github.com/b/statsd_spec/blob/master/README.md](https://github.com/b/statsd_spec/blob/master/README.md)  
**Current, or at least more recent statsd spec** for metric types:  
[https://github.com/etsy/statsd/blob/master/docs/metric_types.md](https://github.com/etsy/statsd/blob/master/docs/metric_types.md)

**I would recommend NSubstitute** instead if you were looking for a mocking framework for .NET.  
[http://blog.binarymist.net/2013/12/14/evaluation-of-net-mocking-libraries/](http://blog.binarymist.net/2013/12/14/evaluation-of-net-mocking-libraries/)

**Information on how jQuery plugins plugin**  
[https://learn.jquery.com/plugins/](https://learn.jquery.com/plugins/)

**jQuery Validation** documentation  
[http://jqueryvalidation.org/documentation/](http://jqueryvalidation.org/documentation/)

[http://jqueryvalidation.org/validate](http://jqueryvalidation.org/validate)

[http://jqueryvalidation.org/jQuery.validator.addMethod](http://jqueryvalidation.org/jQuery.validator.addMethod)

[http://jqueryvalidation.org/rules](http://jqueryvalidation.org/rules)

**express-form**  
[https://github.com/freewil/express-form](https://github.com/freewil/express-form)

**Recording and testing user time expenditure**

[http://www.smashingmagazine.com/2011/03/in-search-of-the-perfect-captcha/#recording-user-time-expenditure](http://www.smashingmagazine.com/2011/03/in-search-of-the-perfect-captcha/#recording-user-time-expenditure)

[http://stackoverflow.com/questions/8472/practical-non-image-based-captcha-approaches](http://stackoverflow.com/questions/8472/practical-non-image-based-captcha-approaches)




[**Blowfish cipher**](https://en.wikipedia.org/wiki/Blowfish_(cipher))

[**PBKDF2**](https://en.wikipedia.org/wiki/PBKDF2)

[**Key Derivation Function**](https://en.wikipedia.org/wiki/Key_derivation_function) (KDF)

[**bcrypt**](https://en.wikipedia.org/wiki/Bcrypt)

[**Cryptographic hash function**](https://en.wikipedia.org/wiki/Cryptographic_hash_function): MD5, SHA1, SHA2, etc

[**Key stretching**](https://en.wikipedia.org/wiki/Key_stretching)

[**scrypt**](https://en.wikipedia.org/wiki/Scrypt)

**bcrypt brute-forcing** becoming feasible with well ordered rainbow tables  
[http://www.openwall.com/presentations/Passwords13-Energy-Efficient-Cracking/Passwords13-Energy-Efficient-Cracking.pdf](http://www.openwall.com/presentations/Passwords13-Energy-Efficient-Cracking/Passwords13-Energy-Efficient-Cracking.pdf)  
[https://www.usenix.org/system/files/conference/woot14/woot14-malvoni.pdf](https://www.usenix.org/system/files/conference/woot14/woot14-malvoni.pdf)

[**Justin Searls talk**](http://blog.testdouble.com/posts/2014-12-02-the-social-coding-contract.html) on consuming all the open source.

[**Effecting Change**](http://blog.binarymist.net/2013/06/22/ideas-for-more-effective-meetings-and-presentations/)
