# People {#people}

![10,000' view of People Security](images/10000People.gif)

Like the section on [Physical](#physical) security, the people problem is often over-looked, not only by technical people this time, but by all. For a proficient social engineer, it's generally fairly easy to craft and execute attacks. Although not quite as easy and simple as walking through the front door that's left open. With a little patience, practice and the right frame of mind, the majority of people can be played successfully, even those that are very aware.

Why is it often over-looked? A couple of reasons I can think of.

1. It's too simple
2. Many of us don't like to put the spot-light on ourselves and look deep inside to try and find what makes us tick, what our flaws look like, why they exist, how we can re-architect the affected areas and in some cases work around them, but being aware of them. There is often real pain when we start poking and prodding at our inner weak areas.

As with many other attacks, this is often one that is a key component of a larger attack. 

## 1. SSM Asset Identification
Take results from [higher level Asset Identification](#ssm-asset-identification). Remove any that are not applicable. Add any newly discovered.

## 2. SSM Identify Risks
Risks based on the failures of people present a very different set of attack vectors than any others mentioned in this material. People are both complex, complicated and our personalities are full of faults just waiting to be compromised, thus the approach at finding vulnerabilities is quite different.  
You can still use some of the processes from [2. SSM Identify Risks](#ssm-identify-risks), but the outcomes can look quite different.  
I find the [Threat agents cloud](#threat-agents) and [Likelihood and impact](#likelihood-and-impact) diagrams still quite useful. Also [MS 5. Document the Threats](#ms-5-document-the-threats), [OWASP Risk Rating Methodology](#ms-5-document-the-threats) and the [intel-threat-agent-library](#intel-threat-agent-library) as they're technology agnostic.  
[OWASP Ranking of Threats](#ms-6-rate-the-threats), [MS 6. Rate the Threats](#ms-6-rate-the-threats)  and DREAD to a degree are useful.

People are the strongest point in a security process, they are often also the weakest.

### Phishing {#people-identify-risks-phishing}
![](images/ThreatTags/average-widespread-easy-moderate.png)

Emails being sent out with payloads veiled with enticing email subjects, file names, etc. in the form of web links and files that when executed deliver a malicious payload. Also check out the [Infectious Media](#people-identify-infectious-media) section in this chapter. Casting a wide net in the hope that a number of the receivers will fall for the scam. This attack takes advantage of the large number of receivers that will potentially fall for the scam. Generally the percentage of victims will be much lower than if it was a spear phishing attack, because the email will be more general in the attempt to reach less specific targets.  
Like large commercial fishing enterprises, this type of attack is very common and being executed on large scale in many instances at a time.  
This type of attack is also usually a lot easier to detect, because the attackers have to be more general in their approach and they rely less on detailed information gathering of their victims when crafting the attack than if it was more targeted.  
The outcome on average is also usually less dramatic. Yes the odd victim will be fooled. This type of attack is a numbers game. For example 100 people may be fooled when a 50'000 email campaign is crafted and sent.

SET has many options to help with this type of attack. They're all covered at the [social-engineer.org](http://www.social-engineer.org/framework/se-tools/computer-based/social-engineer-toolkit-set/) website.

SET can use sendmail, gmail or your own open-mail relay out of the box to perform your attacks.

### Spear Phishing {#people-identify-risks-spear-phishing}
![](images/ThreatTags/easy-common-average-severe.png)

This type of attack is targeted toward a smaller number of receivers generally with more specifically tailored scams. In order to pull a successful attack off, that's one that has a high catch rate and where no receivers actually raise alarm bells with managers, scrupulous reconnaissance needs to be carried out on your target(s).

![](images/HandsOnHack.png)

The following attack was one of five that I demonstrated at WDCNZ in 2015. There was one leading up to this which focused on exploiting a Cross-Site Scripting (XSS) vulnerable website with the Browser Exploitation Framework (BeEF). This can be found in chapter 12 under the [Cross-Site Scripting (XSS)](#web-applications-identify-risks-cross-site-scripting) section.

You can find the video of how this attack is played out [here](https://www.youtube.com/watch?v=tb4o5UCHzSA).

I> ## Synopsis
I>
I> Clone a website that we know our victim visits and has to log-in at.  
I> Host the clone from our attack machine, although this could be hosted anywhere and would be more effective in not getting caught, hosting on the likes of one of the free VPS's on the internet.  
I> Using the Credential Harvester from the Social Engineer Toolkit (SET) or setoolkit in Kali Linux. This uses the `HTML` `referer` header, in which it intercepts the request that comes from the victims IP address and harvests the posted credential fields. We demonstrate a similar attack on a XSS vulnerable website that we have hooked with BeEF, where we use BeEF's "Pretty Theft" module to harvest the victims credentials when they log in to the website in the [Cross-Site Scripting (XSS)](#web-applications-identify-risks-cross-site-scripting) section in chapter 12 Web Applications.  
I> Social engineer our victim to our cloned website.  
I> Once victim enters their credentials and submits, SET harvests the credentials.  
I> They are redirected to the original website to make it look less conspicuous, kind of like a failed log-in.  
I> Making the URL look more legitimate is covered in the [ARP](#network-identify-risks-spoofing-arp) and [DNS](#network-identify-risks-spoofing-dns) spoofing section in the Network chapter.

{icon=bomb}
G> ## The Play
G>
G> Clear out the public web directory (`/var/www/`) on your Kali Linux machine. If you don't, SET will archive what's already in there.
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
G> Enter IP address that SET listens on to capture the key log. In this case it'll be your local IP address.
G>
G> We clone accounts.google.com
G>
G> SET now hosts cloned and php files in the apache web dir `/var/www/` and starts apache if it’s not already running.
G>
G> Now we see the cloned artefacts and the key log file `harvester_<yyyy-mm-dd HH:mm:ss.n>.txt` in `/var/www/` which will be currently empty.
G>
G> The victim clicks the link that was passed to them via social engineering. At this stage this is just the IP address that SET cloned and hosted your website from. Your cloned website is loaded in the victims browser. This could be any site that you know the victim has credentials for that you would like. As soon as the victim posts (as discussed above in the synopsis) SET:
G>
G> 1) intercepts the request
G>
G> 2) harvests the credentials
G>
G> 3) writes to the harvest file
G>
G> 4) and the page redirects to the real accounts.google.com.
G>
G> Now if you have a look in the `/var/www/harvester_<yyyy-mm-dd HH:mm:ss.n>.txt` file, you'll find the targets credentials.

T> ## Crafting Emails with SET
T>
T> SET provides the ability to craft emails with spoofed from address. You just need to install and configure sendmail.

### Infectious Media {#people-identify-infectious-media}

_Todo_

%% Resources:

%% http://www.social-engineer.org/framework/se-tools/computer-based/social-engineer-toolkit-set/

%% Book: Social Engineering The Art Of Human Hacking
%%   Pg 50 has good info about what people are likely to click on.-->

### Phone Calls {#people-identify-risks-phone-calls}
![](images/ThreatTags/average-common-average-severe.png)

With practice this can become reasonably easy to pull off, also with very little risk. Plus the social engineer (SE) can practice, practice, practice until they get the results they're looking for. If at first they don't succeed, then they can simply hangup and try again in a few days. Each attempt improving on what let them down last time.  
As mentioned in Bruce Schneier's Beyond Fear: Convicted hacker Kevin Mitnick testified before Congress in
2000 about social engineering. “I was so successful in that line of attack that I rarely had to resort to a technical attack,” he said. “Companies can spend **millions of dollars toward technological protections**, and that’s wasted if somebody can basically call someone on the telephone and either convince them to do something on the computer that lowers the computers defences or reveals the information they were seeking.”  
The government that imprisoned Kevin Mitnick for nearly five years, later [sought his advice](http://www.politechbot.com/p-00969.html) about how to keep its own networks safe from intruders.

### Spoofing Caller Id {#people-identify-risks-spoofing-caller-id}
![](images/ThreatTags/easy-widespread-difficult-low.png)

This is arranging an impersonated identification (name or number) to be displayed on the receiving end of a phone that's in the state of receiving a call. The telephony providers don't perform authentication on whether the caller Id is valid or spoofed. To disambiguate, this is the caller Id and not the caller number.  
This can be quite a useful confirmation for a social engineer to use as part of their pretexting. Caller Id spoofing has been around since at least 2004, with companies offering paid services like:

* SpoofCard
* SpoofTell
* Jumblo
* and many more.

Or you can DIY with the likes of [Asterisk](http://www.asterisk.org/get-started) an open source framework providing all the tools anyone would need to spoof caller Ids and much more.

### Favour for a Favour {#people-identify-risks-favour-for-a-favour}
![](images/ThreatTags/easy-common-average-severe.png)

How this works:
An Attacker (Pretexter) calls someone at the target organisation (often an employee). Then explains to them that something has just happened that will stop them from doing their work. Ideally something that the victim can not immediately verify. Attacker suggests that they may be able to fix the problem for the employee, but it'll be a big favour. Victim agrees and is very appreciative. Once the attacker has pretended to fix the employees issue, they get the victim to confirm that it is now working (of course it always was working). Victim confirms it's now working and is very appreciative. Now the victim essentially ows the attacker a favour. The attacker can use this favour that's essentially owed to them immediately or often more effectively the next day (next day seems more genuine).

These attacks are the easiest to carry out on:

* new people
* people with limited computer knowledge and skills, as they probably won't realise what value exists on the computer or network for the person that's just apparently helped them out

### The New Employee {#people-identify-risks-the-new-employee}
![](images/ThreatTags/easy-common-average-severe.png)

New employees won't intuitively know a lot of people in the organisation yet. They won't yet understand the hierarchical relationships and processes. They won't be aware of the security policies and culture in force. They'll be as keen as can be to make good impressions with everyone they relate with.

### We Have a Problem {#people-identify-risks-we-have-a-problem}
![](images/ThreatTags/average-common-average-severe.png)

An excellent way of gaining additional trust is to fix the problem that doesn't exist. Lead the victim to believe that there's a problem that will stop them from getting there work done in some way. Request a little information. Fix the problem that never existed. Victim verifies that there is no longer a problem. Request additional sensitive information. Using the fact that you just did a favour for the victim to elicit what you're after.

### It's Just the Cleaner {#people-identify-risks-its-just-the-cleaner}
![](images/ThreatTags/easy-common-easy-severe.png)

Grab your self a cleaners uniform and you've got access to just about everything within an organisation. It really is that simple. Often when my wife was doing commercial cleaning and she walked into a room at 20:00 - 21:00 with a few developers left working away. They would sometimes be surprised because they hadn't been told that the cleaners were coming, let-a-lone, who they were. The response would usually go something like "Oh, it's just the cleaners". Why? Because they had their free pass... a cleaners uniform, maybe a broom or vacuum cleaner or mop.

### Come On In Sir {#people-identify-risks-come-on-in-sir}
![](images/ThreatTags/easy-common-easy-severe.png)

Two effective ways at getting into a work premises.

1. Mingle in with a crowd of people returning from a lunch break. Be prepared to drop some names of people that work there but are not in the current group. Yes, that means you have to do your home work and know their faces. You'll get through the locked front door 99% of the time.
2. Wait in the car park until you see someone close to the front door (inside or outside) and carry a large heavy **looking** box toward the door. 99% of the time, they will open the door for you. Just make sure you have thought about all possible questions that may get fired at you and have well researched answers ready to respond with.

## 3. SSM Countermeasures

### Phone Calls {#people-countermeasures-phone-calls}
![](images/ThreatTags/PreventionDIFFICULT.png)

Even the most technically and security focussed people can be played.

Focus on the low hanging fruit. Social Engineering attacks via phone calls is definitely one of the lowest. Using the output from the various risk rating and ranking of threats from [2 SSM Identify Risks](#ssm-identify-risks) will help you realise this.  
Make sure that people being trusted are trustworthy. Test them. Most attacks target trusted people because they have something of value. Make sure the amount of value they have access to is in proportion to how trustworthy they actually are. Don't assume. Educate (train) and test employees. Make sure they are well compensated. Do what ever it takes to keep their levels of passion and engagement high. People with these qualities will be far more likely to succeed in recognising and warding off attacks.

### Spoofing Caller Id {#people-countermeasures-spoofing-caller-id}

![](images/ThreatTags/PreventionDIFFICULT.png)

Don't rely on caller Id. It's untrusted. I'm not aware of any way to successfully [detect](http://www.cse.sc.edu/~mustafah/download/cid_USC_CSE_TR-2013-001.pdf) caller Id spoofing before the call is answered.

### Favour for a Favour {#people-countermeasures-favour-for-a-favour}
![](images/ThreatTags/PreventionAVERAGE.png)

If someone you don't know does you a favour then asks for a favour. You're probably being played. Get suspicious. Start thinking hard about what they're actually asking for. 

### The New Employee {#people-countermeasures-the-new-employee}
![](images/ThreatTags/PreventionAVERAGE.png)

All employees must be educated in that they should never reveal their passwords. It doesn't matter who to. If a password is asked for, suspicions should be instantly raised to finding out the identity of the requester, as they're obviously malicious or extremely ill-informed. This is often a good candidate to practise reverse social engineering on the requester, finding out as much about the person and what they're after as possible without raising suspicions that their disguise has been identified.

### We Have a Problem {#people-countermeasures-we-have-a-problem}
![](images/ThreatTags/PreventionAVERAGE.png)

Many of the social engineering attacks put a focus on a problem that doesn't exist, but sounds very real. If the pretend problem could stop the victim from getting their work done, that'll provide more leverage and more willingness on the part of the victim to give up sensitive data that they otherwise wouldn't.

### It's Just the Cleaner {#people-countermeasures-its-just-the-cleaner}
![](images/ThreatTags/PreventionVERYEASY.png)

1. Think. Respect your workers. Provide them with the visibility as to who is allowed access to where and when.
2. Train your workers
3. Test them. Send some random in without telling your workers and see what happens. Adjust your training to suite

### Come On In Sir {#people-countermeasures-come-on-in-sir}
![](images/ThreatTags/PreventionAVERAGE.png)

&nbsp;

All of these attacks require: Train -> Test -> Repeat

Creating an organisation wide single point of contact that can process all security related concerns will help to see when an attack is in progress, as in most cases, the attacker(s) will be making multiple attempts to pre-load and elicit sensitive information. Often from multiple targets. Emphasise that employees should contact that single point at even the slightest feeling of something being a bit suspicious.

If you took the people out of the security equation, there would be no insecurities. People are the source of (I'm fairly sure) all of the security issues we face today. So realistically, this is where we need to focus most of our attention. Education, establishing cultures of people that understand the issues, are prepared to and expect to be played and can and do react appropriately. 


## 4. SSM Risks that Solution Causes
Often the modern thinking is that we can replace flawed people with technology. People are resilient and dynamic. They can spot a threat they've never seen before and defend against it. Computers can only defend against what they've been programmed to defend against. Technological defences are brittle. It's the role of technology to support humans to make good decisions.

Employees can get complacent with training, unless they are faced with very real simulations. Get creative, establish a base line training for all and additional training for areas of specialisation and extra trust. 

## 5. SSM Costs and Trade-offs
> An exercise for the reader. What are they?

## Additional Resources
The following books have been influential for me and the above content should reflect that. They are very insightful and well worth the investment.

* The Social Engineer's Playbook: A Practical Guide to Pretexting by Jeremiah Talamantes
* Social Engineering: The Art of Human Hacking by Christopher Hadnagy
* The Art Of Deception by Kevin Mitnick
* Chapter 10 of Beyond Fear by Bruce Schneier
* Gray Hat Hacking The Ethical Hacker's Handbook
