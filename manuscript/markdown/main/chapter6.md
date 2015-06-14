{#people)
# People

![10,000' view of People Security](images/10000People.gif)

Like the section on [Physical](#physical) security, the people problem is often over-looked, not only by technical people this time, but by all. For a proficient social engineer, it's generally fairly easy to craft and execute attacks. Although not quite as easy and simple as walking through the front door that's left open. With a little patience, practice and the right frame of mind, the majority of people can be played successfully, even those that are very aware.

Why is it often over-looked? A couple of reasons I can think of.

1. It's too simple
2. Many of us don't like to put the spot-light on ourselves and look deep inside to try and find what makes us tick, what our flaws look like, why they exist, how we can re-architect the affected areas and in some cases work around them, but being aware of them. There is often real pain when we start poking and prodding at our inner weak areas.

As with many other attacks, this is often one that is a key component of a larger attack. 

## 1. SSM Asset Identification
Take results from [higher level Asset Identification](#1-ssm-asset-identification). Remove any that are not applicable. Add any newly discovered.

## 2. SSM Identify Risks
Risks based on the failures of people present a very different set of attack vectors than any others mentioned in this material. People are both complex, complicated and our personalities are full of faults just waiting to be compromised, thus the approach at finding vulnerabilities is quite different.  
You can still use some of the processes from [2. SSM Identify Risks](chapter1.md/#2-ssm-identify-risks), but the outcomes can look quite different.  
I find the [Threat agents cloud](#threat-agents) and [Likelihood and impact](#likelihood-and-impact) diagrams still quite useful. Also [MS 5. Document the Threats](#ms-5-document-the-threats), [OWASP Risk Rating Methodology](#owasp-risk-rating-methodology) and the [intel-threat-agent-library](#intel-threat-agent-library) as they're technology agnostic.  
[OWASP Ranking of Threats](#owasp-ranking-of-threats), [MS 6. Rate the Threats](#ms-6-rate-the-threats)  and DREAD to a degree are useful.

People are the strongest point in a security process, they are often also the weakest.

{#people-identify-risks-phone-calls}
### Phone calls
![](images/ThreatTags/average-common-average-severe.png)

With practice this can become reasonably easy to pull off, also with very little risk. Plus the social engineer (SE) can practice, practice, practice until they get the results they're looking for. If at first they don't succeed, then they can simply hangup and try again in a few days. Each attempt improving on what let them down last time.  
As mentioned in Bruce Schneier's Beyond Fear: Convicted hacker Kevin Mitnick testified before Congress in
2000 about social engineering. “I was so successful in that line of attack that I rarely had to resort to a technical attack,” he said. “Companies can spend **millions of dollars toward technological protections**, and that’s wasted if somebody can basically call someone on the telephone and either convince them to do something on the computer that lowers the computers defences or reveals the information they were seeking.”  
The government that imprisoned Kevin Mitnick for nearly five years, later [sought his advice](http://www.politechbot.com/p-00969.html) about how to keep its own networks safe from intruders.

{#people-identify-risks-spoofing-caller-id}
### Spoofing Caller Id
![](images/ThreatTags/easy-widespread-difficult-low.png)

This is arranging an impersonated identification (name or number) to be displayed on the receiving end of a phone that's in the state of receiving a call. The telephony providers don't perform authentication on whether the caller Id is valid or spoofed. To disambiguate, this is the caller Id and not the caller number.  
This can be quite a useful confirmation for a social engineer to use as part of their pretexting. Caller Id spoofing has been around since at least 2004, with companies offering paid services like:

* SpoofCard
* SpoofTell
* Jumblo
* and many more.

Or you can DIY with the likes of [Asterisk](http://www.asterisk.org/get-started) an open source framework providing all the tools anyone would need to spoof caller Ids and much more.

{#people-identify-risks-favour-for-a-favour}
### Favour for a Favour
![](images/ThreatTags/easy-common-average-severe.png)

How this works:
An Attacker (Pretexter) calls someone at the target organisation (often an employee). Then explains to them that something has just happened that will stop them from doing their work. Ideally something that the victim can not immediately verify. Attacker suggests that they may be able to fix the problem for the employee, but it'll be a big favour. Victim agrees and is very appreciative. Once the attacker has pretended to fix the employees issue, they get the victim to confirm that it is now working (of course it always was working). Victim confirms it's now working and is very appreciative. Now the victim essentially ows the attacker a favour. The attacker can use this favour that's essentially owed to them immediately or often more effectively the next day (next day seems more genuine).

These attacks are the easiest to carry out on:

* new people
* people with limited computer knowledge and skills, as they probably won't realise what value exists on the computer or network for the person that's just apparently helped them out

{#people-identify-risks-the-new-employee}
### The New Employee
![](images/ThreatTags/easy-common-average-severe.png)

New employees won't intuitively know a lot of people in the organisation yet. They won't yet understand the hierarchical relationships and processes. They won't be aware of the security policies and culture in force. They'll be as keen as can be to make good impressions with everyone they relate with.

{#people-identify-risks-we-have-a-problem}
### We Have a Problem
![](images/ThreatTags/average-common-average-severe.png)

An excellent way of gaining additional trust is to fix the problem that doesn't exist. Lead the victim to believe that there's a problem that will stop them from getting there work done in some way. Request a little information. Fix the problem that never existed. Victim verifies that there is no longer a problem. Request additional sensitive information. Using the fact that you just did a favour for the victim to elicit what you're after.

{#people-identify-risks-its-just-the-cleaner}
### It's Just the Cleaner
![](images/ThreatTags/easy-common-easy-severe.png)

Grab your self a cleaners uniform and you've got access to just about everything within an organisation. It really is that simple. Often when my wife was doing commercial cleaning and she walked into a room at 20:00 - 21:00 with a few developers left working away. They would sometimes be surprised because they hadn't been told that the cleaners were coming, let-a-lone, who they were. The response would usually go something like "Oh, it's just the cleaners". Why? Because they had their free pass... a cleaners uniform, maybe a broom or vacuum cleaner or mop.

{#people-identify-risks-come-on-in-sir}
### Come On In Sir
![](images/ThreatTags/easy-common-easy-severe.png)

Two effective ways at getting into a work premises.

1. Mingle in with a crowd of people returning from a lunch break. Be prepared to drop some names of people that work there but are not in the current group. Yes, that means you have to do your home work and know their faces. You'll get through the locked front door 99% of the time.
2. Wait in the car park until you see someone close to the front door (inside or outside) and carry a large heavy **looking** box toward the door. 99% of the time, they will open the door for you. Just make sure you have thought about all possible questions that may get fired at you and have well researched answers ready to respond with.

## 3. SSM Countermeasures

{#people-countermeasures-phone-calls}
### Phone calls
![](images/ThreatTags/PreventionDIFFICULT.png)

Even the most technically and security focussed people can be played.

Focus on the low hanging fruit. Social Engineering attacks via phone calls is definitely one of the lowest. Using the output from the various risk rating and ranking of threats from [2 SSM Identify Risks](#2-ssm-identify-risks) will help you realise this.  
Make sure that people being trusted are trustworthy. Test them. Most attacks target trusted people because they have something of value. Make sure the amount of value they have access to is in proportion to how trustworthy they actually are. Don't assume. Educate (train) and test employees. Make sure they are well compensated. Do what ever it takes to keep their levels of passion and engagement high. People with these qualities will be far more likely to succeed in recognising and warding off attacks.

{#people-countermeasures-spoofing-caller-id}
### Spoofing Caller Id

![](images/ThreatTags/PreventionDIFFICULT.png)

Don't rely on caller Id. It's untrusted. I'm not aware of any way to successfully [detect](http://www.cse.sc.edu/~mustafah/download/cid_USC_CSE_TR-2013-001.pdf) caller Id spoofing before the call is answered.

{#people-countermeasures-favour-for-a-favour}
### Favour for a Favour
![](images/ThreatTags/PreventionAVERAGE.png)

If someone you don't know does you a favour then asks for a favour. You're probably being played. Get suspicious. Start thinking hard about what they're actually asking for. 

{#people-countermeasures-the-new-employee}
### The New Employee
![](images/ThreatTags/PreventionAVERAGE.png)

All employees must be educated in that they should never reveal their passwords. It doesn't matter who to. If a password is asked for, suspicions should be instantly raised to finding out the identity of the requester, as they're obviously malicious or extremely ill-informed. This is often a good candidate to practise reverse social engineering on the requester, finding out as much about the person and what they're after as possible without raising suspicions that their disguise has been identified.

{#people-countermeasures-we-have-a-problem}
### We Have a Problem
![](images/ThreatTags/PreventionAVERAGE.png)

Many of the social engineering attacks put a focus on a problem that doesn't exist, but sounds very real. If the pretend problem could stop the victim from getting their work done, that'll provide more leverage and more willingness on the part of the victim to give up sensitive data that they otherwise wouldn't.

{#people-countermeasures-its-just-the-cleaner}
### It's Just the Cleaner
![](images/ThreatTags/PreventionVERYEASY.png)

1. Think. Respect your workers. Provide them with the visibility as to who is allowed access to where and when.
2. Train your workers
3. Test them. Send some random in without telling your workers and see what happens. Adjust your training to suite

{#people-countermeasures-come-on-in-sir}
### Come On In Sir
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
