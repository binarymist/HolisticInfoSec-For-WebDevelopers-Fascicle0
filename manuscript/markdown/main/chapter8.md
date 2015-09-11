# People {#people}

![10,000' view of People Security](images/10000People.gif)

Like the section on [Physical](#physical) security, the people problem is often over-looked, not only by technical people this time, but by all. For a proficient social engineer, it's generally fairly easy to craft and execute attacks. Although not quite as easy and simple as walking through the front door that's left open. With a little patience, practice and the right frame of mind, the majority of people can be played successfully, even those that are very aware.

Why is it often over-looked? A couple of reasons I can think of.

1. It's too simple
2. Many of us don't like to put the spot-light on ourselves and look deep inside to try and find what makes us tick, what our flaws look like, why they exist, how we can re-architect the affected areas and in some cases work around them, but being aware of them. There is often real pain when we start poking and prodding at our inner weak areas.

As with many other attacks, this is often one that is a key component of a larger attack. 

## 1. SSM Asset Identification
Take results from [higher level Asset Identification](#ssm-asset-identification). Remove any that are not applicable. Add any newly discovered.

* People carry huge amounts of confidential information and not only in their bags and devices but also most importantly their brains. People are like sponges, we soak up information everywhere. We also leak a lot and are capable of leaking without even knowing it when played by a skilled social engineer, but I'll leave that part to the Identify Risks section.
* State of mind. That's right, an engaged, devoted and loyal worker is truly an asset.

## 2. SSM Identify Risks
Risks based on the failures of people present a very different set of attack vectors than any others mentioned in this material. People are both complex, complicated and our personalities are full of faults just waiting to be compromised, thus the approach at finding vulnerabilities is quite different.  
You can still use some of the processes from [2. SSM Identify Risks](#ssm-identify-risks), but the outcomes can look quite different.  
I find the [Threat agents cloud](#threat-agents) and [Likelihood and impact](#likelihood-and-impact) diagrams still quite useful. Also [MS 5. Document the Threats](#ms-5-document-the-threats), [OWASP Risk Rating Methodology](#ms-5-document-the-threats) and the [intel-threat-agent-library](#intel-threat-agent-library) as they're technology agnostic.  
[OWASP Ranking of Threats](#ms-6-rate-the-threats), [MS 6. Rate the Threats](#ms-6-rate-the-threats)  and DREAD to a degree are useful.

People are the strongest point in a security process, they are often also the weakest.

### Morale, Productivity and Engagement Killers

#### Undermined Motivation

Studies show that motivation has a larger effect on productivity and quality than any other factor.  
Managers that don't lead from the front and walk the talk.  
I once worked in a team many years ago that was responsible for delivering a large project for one of the worlds largest courier companies. This meant architecting and implementing a mobile software solution that would have well over 100,000 couriers doing their pick-up and deliveries using our software to manage it all. This was before mobile devices were common place. The team was told that they would be rewarded for working many overtime hours to get this project deployed on time. As a team of mostly young software engineers, we accepted the challenge and we delivered what the client needed and on time. The team members that did most of the over-time hours were rewarded with approximately an extra 50c per hour.

#### Adding people to a Late Project

Common questions you may hear a manager ask:  
1. What can we do to make this project go faster?
2. Do you need more engineers?  
Throwing more engineers at a problem usually makes it worse. The only definite way to get something built faster is to build a smaller thing.

#### Noisy, Crowded Offices

Noise everywhere kills deep thought and concentration.  
   
#### Email

The content is only 7% of communication. The rest is voice, tone, body language and context. It takes much longer to craft emails than to just open your mouth or to engage in just about any other form of communication. 
   
#### Meetings

Be aware that this is actually not creating software. Just be careful when you hold them.

#### Context Switching

Dividing your workload between multiple tasks destroys motivation and actually makes as more stupid. That's right. There has been studies performed at the Institute of Psychiatry which found that excessive use of technology reduced workers' intelligence. "Those distracted by incoming email and phone calls saw a 10-point fall in their IQ - more than twice that found in studies of the impact of smoking marijuana", said researchers

![](images/ContextSwitching.png)

Gerald Weinberg's rule that 20% of our time is lost every time we perform a context switch.

I've been around many administrative people that don't understand how much context switching effects all peoples effectiveness at performing any task. It's just more apparent in deeply technical workers. Each task we try and do in parallel, negatively impacts the other tasks we're working on. The more tasks we juggle, the less processing power we have for each one.

### Employee Snatching

A very profitable tactic is for an adversary to in one shape or form acquire a staff member of a target organisation, perhaps a competing organisation. Depending on how long the staff member has worked for the target will depend how much of a gold mine they are. Why bother doing something illegal when you can just offer the staff member a better deal than they are currently getting?

### Phone Calls {#people-identify-risks-phone-calls}
![](images/ThreatTags/average-common-average-severe.png)

With practice this can become reasonably easy to pull off, also with very little risk. Plus the social engineer (SE) can practice, practice, practice until they get the results they're looking for. If at first they don't succeed, then they can simply hangup and try again in a few days. Each attempt improving on what let them down last time.  
As mentioned in Bruce Schneier's Beyond Fear: Convicted hacker Kevin Mitnick testified before Congress in 2000 about social engineering. “_I was so successful in that line of attack that I rarely had to resort to a technical attack,_” he said. “_Companies can spend millions of dollars toward technological protections, and that’s wasted if somebody can basically call someone on the telephone and either convince them to do something on the computer that lowers the computers defences or reveals the information they were seeking._”  
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
An Attacker (Pretexter) calls someone at the target organisation (often an employee). Then explains to them that something has just happened that will stop them from doing their work. Ideally something that the victim can not immediately verify. Attacker suggests that they may be able to fix the problem for the employee, but it'll be a big favour. Victim agrees and is very appreciative. Once the attacker has pretended to fix the employees issue, they get the victim to confirm that it is now working (of course it always was working). Victim confirms it's now working and is very appreciative. Now the victim essentially owes the attacker a favour. The attacker can use this favour that's essentially owed to them immediately or often more effectively the next day (next day seems more genuine).

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

{#wdcnz-demo-2}
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

### Infectious Media {#people-identify-risks-infectious-media}

_Todo_

%% Resources:

%% http://www.social-engineer.org/framework/se-tools/computer-based/social-engineer-toolkit-set/

%% Book: Social Engineering The Art Of Human Hacking
%%   Pg 50 has good info about what people are likely to click on.-->

## 3. SSM Countermeasures

### [Morale, Productivity and Engagement Killers](https://speakerdeck.com/binarymist/how-to-increase-software-developer-productivity) {#people-countermeasures-morale-productivity-and-engagement-killers}

Staff are often thought of as resources. If you want the best out of your people, treat them like valuable assets. Go out of your way to build meaningful relationships with them and create empathy. People base their decisions on emotions, later justifying them with facts. With this in mind, keep them in a good emotional place. Staff that feel appreciated work much harder and will be more likely to:
  * be conscientious about everything including following organisational policy
  * remain engaged and be alert
  * have the organisations best interests at heart
  * Not bad mouth their seniors behind their backs
  * Withhold confidential information from anyone eliciting

#### Undermined Motivation

If you're a manager don't screw your workers. It may sound obvious, but I still see it happening in many organisations. Software Engineers are smart people that have paid a price to get into the field they work in. They are self motivated. As a manager, you have a strong influence on how motivated they remain. If you're smart, you will do everything in your power to feed the engineers appetite to excel.

#### Adding people to a Late Project

Throwing more engineers at a problem usually makes it worse. The only definite way to get something built faster is to build a smaller thing.

#### Noisy, Crowded Offices

Allow your developers to create a space that they love working in. When I work from home my days are far more productive than when working for a company that insists on cramming as many workers around you into as small and often noisy a space as possible.
   
#### Email

We have many communication tools that are far more effective than email. Lets use them. In order of effectiveness, we have:  
1. Face to face. You can't beat this. It uses as many of our senses as possible and includes the human spirit. Those that are aware need to encourage developers to talk to whoever it is they are working with face to face if it's practical.
2. Audio Visual. We have cyber networks now, the internet, and many great AV tools for one on one and team collaboration. I could list all the tools I know of and/or use, but this landscape is constantly evolving. Find what works for you and your teams. Many people find that using a good AV tool fits their work flow better than physical face to face communications. This is because it allows workers to have the best of both worlds. It gives them their own space and at the push of a button allows them to see their co-worker(s) and hear them. I have personally found good AV tools to be very liberating and great productivity enhancers.
3. Audio only. Push to talk. The likes of Mumble can be an excellent staple tool for talking things through during the working day, when you don't need that extra information that the visual aspect provides. I've used this on many occasions and found it to be a very effective medium. When you do need more, then you can just escalate to a higher form of communication with the added visual component.
4. Phone calls are similar to Push to talk, but the context switching overhead is larger, because you have to dial, often put a headset on and wait for an answer. Audio quality is often worse also.
5. Instant Messaging (IM) tools. These are great for quick short communications sessions where you need to pass links and/or have a quick discussion where you don't need the additional overhead of audio. There is often little context switching overhead. It's easy to work and talk with co-workers. You are really starting to loose a large percentage of the communication information here though. Misunderstandings can creep in. Always be ready to escalate to a higher level of communication.
6. Email. Leave this as a last resort. It's ineffective and costly in many ways. Especially for software engineers, where there is a high price for context switching. Software engineers and architects have to build large landscapes of the programmes they are working on in their heads before they can start to make any changes. Essentially loading a programme into memory and wrapping our human understanding of how the mechanics work around it. If a developer has this picture loaded into their memory and then switches to deal with an email, a large part of the picture can easily drop out of memory. Once the email work is finished, the developer has to re-load the picture. Second time around is quicker, but this can still be very costly.

Also don't forget there are many live collaborative drawing, documenting and coding tools available now. These can actually make a work-flow far more effective than being in the same room as co-workers but having to work on one set of screens, as you can both work form your own workstations on the same work.
   
#### Meetings

Find out when the most productive times are for your developers and let them own that time. Often developers work late because it's the only time they don't get interrupted and they have quiet time to concentrate. If your a Product Owner or Manager, make sure your developers have plenty of uninterrupted time to work their magic. Schedule meetings in the least productive part of the day. It may sound obvious, but ask developers when this is. A managers role is to facilitate the developers and get out of their way. Let them manage themselves.

#### Context Switching

Don’t do it. This is a real killer. This is hard. What you need to do is be aware of how much productivity is killed with each switch. Then do everything in your power to make sure the Development Team is sheltered from as much as possible. There are many ways to do this. For starters, you’re going to need as much visibility as possible into how much this is currently happening. track add-hock requests and any other types of interruptions that steel the developers concentration. In a previous Scrum Team that I was Scrum Master of, The Development Team decided to include another metric to the burn down chart that was on the middle of the wall, clearly visible to all. Every time one of the developers was interrupted during a Sprint, they would record this time, the reason and who interrupted them, on the burn down chart. The Scrum Team would then address this during the Retrospective and empirically address why this happened and work out how to stop it happening every Sprint. Jeff Atwood has an informative post on why and how context-switching/multitasking kills productivity. Be sure to check it out.

"_The trick here is that when you manage programmers, specifically, task switches take a really, really, really long time. That's because programming is the kind of task where you have to keep a lot of things in your head at once. The more things you remember at once, the more productive you are at programming. A programmer coding at full throttle is keeping zillions of things in their head at once: everything from names of variables, data structures, important APIs, the names of utility functions that they wrote and call a lot, even the name of the subdirectory where they store their source code. If you send that programmer to Crete for a three week vacation, they will forget it all. The human brain seems to move it out of short-term RAM and swaps it out onto a backup tape where it takes forever to retrieve._"

> Joel Spolsky

#### Top Developer Motivators in Order

1. Developers love to develop software. The best way to motivate developers is to let them develop. Provide an environment to stimulate focus.
2. The Work it self
  * Variety of Skills. Developers enjoy freedom and expectation to exercise a variety of skills, thus producing 'T' shaped developers, those with broad experience and some deep specialities.
  * Responsibility, Significance. People who tighten nuts on air-planes will feel that their work is more important and meaningful than people who tighten nuts on decorative mirrors. Developers must experience meaning in their work.   
  * Task Identity. People care more about their work when they can complete whole and identifiable pieces of work. User Stories for example.   
  * Consumer and Pair Association. Knowing the results of their work and how it affects others   
  * Autonomy. Control over what and how they perform their work. Self managing teams. Many managers still need to realise that they can be freed up from a lot of work if they will just let the Development Team manage their own destiny.
3. Ownership / Buy-in. People work harder to achieve their own goals than someone else's. The team decides how much it wants to tackle, the developer decides what to tackle. The schedules developers create are always ambitious. This is why Scrum, when run by the book can be very effective.   
4. Goal Setting. Setting objectives for developers works, but only pick 1 or 2.
More than that and developers loose the focus. Developers don't respond well to objectives that change daily or are collectively impossible to
meet.   
5. Opportunities for Growth. Half of what a developer knows in order to do his job today will be out of date in 3 years time. If not moving forward in this industry, you're moving backwards. Developers are very motivated by learning new things. Let developers determine how they wish to grow professionally. The best developers will gravitate toward organisations that foster personal growth.
6. Personal Life. Top developers like to invest a lot in their own time (experimenting, researching, teaching others, running events). Let them, and in-fact encourage them.
7. Technical Leadership. Top developers love passing on their knowledge. This is part of what drives them. Assign each team member to be technical lead and possibly mentor for a particular domain, technology and process area.

### Employee Snatching

A lot of combating snatching of staff comes down to the countermeasures of [Morale, Productivity and Engagement Killers](#people-countermeasures-morale-productivity-and-engagement-killers).

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
