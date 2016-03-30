# 5. Physical {#physical}

![10,000' view of Physical Security](images/10000Physical.gif)

The area of physical security is often over looked, especially by technical people, although in most cases, it is the easiest and simplest to circumvent and also the easiest and simplest to mitigate.

## 1. SSM Asset Identification {#physical-asset-identification}
Take results from [higher level Asset Identification](#starting-with-the-30000-foot-view-asset-identification). Remove any that are not applicable. Add any newly discovered. Here are some to get you started:

* Servers, the cabinets they reside in, the rooms cabinets reside in.
* Physical buildings, their openings such as doors and windows. Keys, RFID cards, Access codes
* Sensitive printed matter
* Plant, such as work-stations or any other devices that could be used to launch attack sequences from, or even cause diversions to allow an attacker to carry out an action they wouldn't be able to otherwise. Laptops and mobile devices (including personal) configured to access internal Access Points (APs). Wi-Fi APs
* Visibility into who is where and doing what. Visibility brings immense power to make good decisions and is often an excellent target for an attacker to remove from you.

## 2. SSM Identify Risks {#physical-identify-risks}
You can take the same process that we did at the [top level](#starting-with-the-30000-foot-view-identify-risks), but abstract the ideas and then solidify them on physical components. Some of the ideas will work, some wont.

Many times physical security is good, but it is compromised by the people problem. My wife used to do a lot of commercial cleaning and every night she would come home with new stories about how people were the weakest link when it came to security. Most of the time when physical security is breached there actually is no security, because **people** have failed the design.

I am also a qualified carpenter and as part of my job when I was practising was often required to break into buildings when tenants had done the runner, so I know a few things about physical security as well. Even when doors and windows are locked, their security is usually trivial to compromise without damaging anything. Funnily enough, it was seldom required to force latches, bump or pick locks. There is always a path of least resistance, the lowest hanging fruit, because [people](#people) are usually the weakest link in any security solution. This does not actually have to be the case though, they can be the strongest link also. Refer to the chapter on People for further details.

### Fortress Mentality {#physical-identify-risks-fortress-mentality}

![](images/ThreatTags/easy-widespread-easy-severe.png)

We see the same analogy of the candy bar (fortress) running through many areas of security.  
“_Many organizations still adopt a fortress mentality, where everyone on the outside is bad and stuff on the inside is less dangerous_,” said Brian Krebs, author of the Krebs on Security blog. “_Years of experience has taught us that the biggest problems often stem from the fact that once something gets through the outer defences, it’s often a cakewalk to move around the internal network unimpeded._”  
We not only see this principle applied to networks, but also physical security.

### Internal Doors and Cabinets Left Unlocked {#physical-identify-risks-internal-doors-and-cabinets-unlocked}

![](images/ThreatTags/easy-common-easy-moderate.png)

Doors that lead to isolated areas. Filing cabinets and other cupboards left unlocked or locked and keys or combinations put in obvious places. What assets or potential foot holds are in those areas? What about electronic systems other than core computing systems, like alarm, surveillance or air conditioning components. Sometimes all that's in the way of a successful exploit being carried out is the fact that someone's watching something. If an attacker can take that someone's attention away from an exploitable target (be it a computer, another unlocked door, person or what ever) for a short period of time, they may be able to carry out an activity that takes them further along their attack traversal path. 

### Insecure Doors and/or Windows {#physical-identify-risks-insecure-doors-windows}

![](images/ThreatTags/easy-widespread-easy-severe.png)

Open doors, open windows. Uninteresting right, because there seems to be little challenge to mitigate. So little challenge that the vulnerability often gets ignored. Believe it or not, but it is often the cleaners responsibility to lock doors, windows and set alarms. Anyone can be or masquerade as a cleaner. I have discussed some of these types of threat agents on my [blog](http://blog.binarymist.net/2012/11/04/sanitising-user-input-from-browser-part-1/#threat-agents). Many places I have worked in have had double latched windows only single latched when I do the rounds just before I leave. It is very easy to pry a double latch window open from the outside that is single latched.

### Easily Penetrable Building Materials

![](images/ThreatTags/easy-widespread-easy-moderate.png)

There are many building materials in use today that are trivially easy to compromise. Although cyber attacks as opposed to simple physical penetration are on the increase, physical attacks are generally lower tech and easier to carry out by less technically skilled entities. One thing that is being realised in regard to cyber-crime though is that the physical vector can also be a key component of carrying out a successful technology based attack, similarly to social engineering which we cover in the [People](#people) chapter.

1. Roofing iron, in fact most roofing materials are very easy to either break through or remove. Once an attacker has a small section of roofing material removed, there is little in the way of physical elements stopping them entering the human inhabitable areas. Ceiling and wall linings have little strength and are easily just pushed from their substrate.
2. Sky-lights, as are windows, are easy to get through. Sky-lights are often just less obvious than windows, not always though of course. There are usually metal strips with rubbers underneath holding the glass panes in. With a flat blade screw-driver or any other similar tool, these are usually trivial to remove. This is my usual approach at entering my house when I have misplaced my keys. This is quick, easy and does not have to damage anything.
3. Weak exterior cladding. There are lots of fairly weak building materials in use today. Fibre cement for example is often only 6mm thick. Sometimes this may have a plaster coat on it, which may give a deceptive appearance of strength. Either way, with a good kick, penetration is fairly straight forward.
4. Suspended Ceiling. If you have sensitive information stored in any area, such as server rooms, archive rooms, etc which has a false or suspended (dropped) ceiling, then this is an easily penetrable vulnerability. GIB or Acoustic NRC tiles are extremely common in office environments, but can be removed easily with bare hands, no tools required.

### Service Labels

![](images/ThreatTags/easy-common-difficult-moderate.png)

Often labels are attached to doors, windows, air conditioning units and any other number of electrical appliances and similar, clearly displaying who the service agent is. This sort of information can be very useful to aid an attacker in their social engineering pretexts. With this sort of information, an attacker can pretend to be the service agent specified on the label and will more likely get away with this attack.

### Sensitive Printed Matter {#physical-identify-risks-sensitive-printed-matter}

![](images/ThreatTags/average-common-difficult-moderate.png)

1. Dumpster diving. Trash baskets or shredded material inside the building or dumpsters outside of the building. Whether the printed material is non-shredded or shredded. There have been quite a few tests and studies performed on the effectiveness of many shredders and how easy it is to reassemble shredded printed matter with time. In-fact there are competitions devoted to reassembling shredded printed documents with contestants that have successfully reassembled all printed matter. I have also known of organisations that pay for a service of picking up printed matter, taking away and destroying. What is to stop an attacker masquerading as someone from the organisation you pay to do this?
2. Printed matter left in insecure places: Your desk is insecure. Visitors or service contractors can easily photo documents when you are not there without you ever being aware that they have copied them.
3. Printer Management: Most businesses use Multi Function Devices (MFD’s) for the convenience of having Printers, Scanners, Faxes, Binders etc all combined into a single unit. This is generally a cost saving initiative, and also saves floor/desk real estate considerably. However, MFD’s contain hard drives which can store up to 3000 previously printed documents. These hard drives are not only easy to remove, but are generally forgotten about when a device is retired / lease expired etc. Any sensitive information that is retained on the hard drive at time of sale / release, then becomes the legal property of the new owners / leaseholders.  
There is a well known incident involving a Californian Police department who put 6 old MFD’s up for auction. All of these devices were subsequently purchased by a local motorcycle gang. Unfortunately for the police, they forgot to wipe / remove the MFD hard drives, resulting in thousands of sensitive police reports and case files becoming the legal property of the motor cycle gang.

### RFID Tags {#physical-identify-risks-rfid}

![](images/ThreatTags/average-uncommon-easy-severe.png)

Then there is all the RFID cards, tags for accessing buildings, elevators, car parks, etc. Again, relatively easily exploitable with [readers and cloners](http://hackerwarehouse.com/product/proxmark3-kit/) which you can buy for varying prices.

Or build your own with the likes of the Tessel and its [RFID module](https://tessel.io/modules#module-rfid).

![](images/tesselRFID.jpg)

%% https://github.com/tessel/rfid-pn532/issues/24 Used with permission.

&nbsp;

Of course there are many more that I may go into in another book or blog post.

%% https://www.google.co.nz/search?q=rfid+tags&oq=rfid+tags&aqs=chrome..69i57j0l5.13744j0j7&client=ubuntu&sourceid=chrome&es_sm=93&ie=UTF-8#q=rfid+tag+exploitation
%% http://www.rfidvirus.org/

### Computers Logged in and Unlocked {#physical-identify-risks-computers-logged-in-unlocked}

![](images/ThreatTags/easy-widespread-easy-severe.png)

Computers logged in, not locked and even screens still turned on. Often with very sensitive material clearly displayed on the screens. This is very common. Again physical security is rendered useless due to the people problem.

### Networking Equipment

![](images/ThreatTags/average-common-average-severe.png)

Where ever you have these components stored, they are first class targets for an attacker looking to compromise assets on your network. An attacker needs just enough time to plant some infectious media that will do their bidding of destroying something or perhaps opening a reverse shell to a machine they control on the internet and giving them full access to your network. Do not underestimate how quickly this can be done. Often simply plugging some media in is all that is required.

### Network Ports

![](images/ThreatTags/easy-common-average-severe.png)

Active network ports are just that, active and waiting for a malicious actor to plug in a rouge Wi-Fi Access Point or drop box that will do malicious acts on the network, or phone home with the information it is gathered or with a reverse shell. Devices such as the [Lan Turtle](http://hakshop.myshopify.com/products/lan-turtle) are popular for this.

### Wi-Fi Access Points {#physical-identify-risks-wifi}

![](images/ThreatTags/average-widespread-average-moderage.png)

Wi-Fi pretty much took the world by storm. It provided an extremely convenient way to access the network from both transient and stationary devices. Connected devices no longer required a physical data connection. Because of the amazing convenience Wi-Fi brought with it, users were blind-sided to the huge attack surface Wi-Fi created. With any increases in convenience, there is also an increase in attack surface. You can not have one without the other.

As the human race, we seem bent on adopting technologies as integral parts of our lives with often almost complete ignorance of the costs we are incurring. Every thing that looks good comes at a cost. Generally the better it looks the higher the cost. With Wi-Fi, the cost is the huge attack surface that comes with it. An attacker no longer needs to break into a building and find an active port. They can just sit in their car in the parking lot pretending to answer email or what ever. Or wait until the workers have gone home for the day and comfortably establish themselves outside the organisations premises and go to work on the internal network. It could not be much easier. Keep in mind, most attacks come from within organisations.

Discussed in more depth in the [Network](#network) chapter

#### Hiding the SSID

![](images/ThreatTags/easy-common-difficult-low.png)

Hiding your SSID is security via obscurity. Useful against the casual unskilled observer, but it is not really the casual unskilled observer you need to be concerned about. Depending on an attackers kit, there are plenty of ways to locate hidden SSIDs.

#### Wi-Fi Protected Set-up (WPS)

![](images/ThreatTags/easy-common-average-moderate.png)

WPS is a network security standard that is supposed to allow users to easily add devices to an existing network without entering pass-phrases. Available on many consumer grade APs and even some entry level enterprise devices. This standard as implemented provides a large easy attack surface. Providing convenience thus significantly reducing security.

There are several methods in the WPS standard to provide authorisation to an Enrollee (entity wanting to connect to the AP). All methods provide no security if the Enrollee has physical access to the AP at a point in time and little security even if they do not. If the enrollee can obtain physical access at some point in time, then a trust relationship can be established for an ongoing remote relationship.
The method names are:

* PIN
* Push button
* Near field communication (NFC)
* USB.

Several brute-force attacks exist in which an attacker can compromise the PIN method. Depending on how the WPS on the specific AP is implemented determines how long any of the brute-force attacks could take to crack the PIN. In most cases this is only a matter of hours.

Once connected, the wireless pass-phrase can be extracted from the Enrolled device with no special tools. Essentially the AP just trusts the Enrollee.

### Transient Devices

![](images/ThreatTags/easy-verywidespread-average-severe.png)

From my experience, when a staff member finishes working for an organisation, they are never made to or even encouraged to remove wireless access point credentials from their personal transient devices, but yet, keys and RFID tags are claimed. This makes no sense at all.

### Lack of Visibility

![](images/ThreatTags/easy-widespread-average-moderate.png)

Removing the ability to make well informed decisions, or being able to react when a malicious actor is doing something to compromise your asset(s) is what the removal of visibility is all about.  
Depending on what tools you use to provide visibility, will determine what an attacker needs to do to go about avoiding or removing it.

## 3. SSM Countermeasures

There is a common theme through this section of prevent, detect and respond. Discussed in a little more depth in the [RFID Tags](#physical-countermeasures-rfid) section.

### Fortress Mentality {#physical-countermeasures-fortress-mentality}

![](images/ThreatTags/PreventionAVERAGE.png)

Harden internal attack vectors. [BinaryMist](http://binarymist.io) takes the approach of [De-perimeterisation](http://blog.binarymist.net/2014/12/27/installation-hardening-of-debian-web-server/#fire-walling). That is not relying on network firewalls or LAN segmentation. Hardening every layer (defence in depth) as though all other layers are weak and easily compromised.

![](images/DefenceInDepth.png)

I have [blogged](http://blog.binarymist.net/2012/11/04/sanitising-user-input-from-browser-part-1/#defense-in-depth) and [spoken](http://blog.binarymist.net/presentations-publications/#whats-our-software-doing-with-all-that-user-input) about this on many occasions. The same principle applies to physical security. When the outer layer is removed, there should be many layers of defence within a physical premises as well. We discuss some of these below.

### Internal Doors and Cabinets Left Unlocked {#physical-countermeasures-internal-doors-and-cabinets-unlocked}

![](images/ThreatTags/PreventionEASY.png)

Provide education -> monitor -> test that the education is taking effect and repeat if not. Have someone with a devious creative mindset test these areas. There are also quite a few ideas in the [People](#people) chapter on increasing staff engagement.

### Insecure Doors and/or Windows {#physical-countermeasures-insecure-doors-windows}

![](images/ThreatTags/PreventionVERYEASY.png)

A lot of what you can do to make sure staff secure premises openings when they leave comes down to engagement. See the [People](#people) chapter for ideas on how to increase staff engagement and motivation to care about the organisation they work for and be alert. Do not underestimate how much of staff members attitude has to do with this.  
Do not work your staff so hard that they are so tired that when they leave the premises they just do not think about doing a full rounds check, or if they do miss something.  
It is the age old law. What goes around comes around. Treat your staff as you would like to be treated and they will be much more likely to... that's right, make sure premises openings are locked properly when they leave.

Train staff. The training loop is also important. Educate -> monitor -> test, repeat.

### Easily Penetrable Building Materials

![](images/ThreatTags/PreventionAVERAGE.png)

If you have the luxury of building from scratch for your premises, steer clear of using materials that just look good. Tilt panel concrete and 8" steel reinforced grouted block-work is very good against most types of attacks. Of course if you have a determined attacker, they will get through that, so you will have to rely on defence in depth. Make sure you have many layers of protection and think as though every one of them will be compromised with enough time, planning and/or perseverance.

1. Roofing iron or other roofing materials are less likely to be compromised if the roof is clearly visible from a public place. The busier the public place is the better. If you have to use roofing iron, consider screwing it on with Tek screws, or even better with screws that do not have a common head, thus requiring a specialised tool. If your attacker is smart, they will do their reconnaissance on what is holding this material on before the attack. Tek screws are very difficult to remove with a claw hammer or small pinch bar. Nothing is impenetrable, but you can make an attackers job very difficult.
2. Sky-lights. If you are using these, put them in places that are as public as possible and as hard to reach as possible. Also having them a long way from the floor will make the break-in harder. Fixed sash sky-lights are also a little more difficult. You could also consider fitting bars on the insides of these. Same goes for windows that are in secluded quite places.
3. Weak exterior cladding. Just try and steer clear of using these types of materials. If you have no option, consider making your other layers of defence stronger to compensate.
4. Suspended Ceiling. Filling the roof cavity with obstacles such as barbed wire, bunched chicken wire etc can demotivate all but the most determined attackers.

#### Crime Prevention Through Environmental Design (CPTED)

Again, if you are in the position to be building a premise from scratch, or completing an indepth refurbishment, you can implement CPTED. This is a focus on the external elements of a premise. Using landscaping techniques focused on security, you can use pathways, benches, hedgerows, sculptures and plantings in combination with lighting techniques and highly visible external security cameras to control access to the building, increasing the difficulty for an attacker to approach undetected. A well designed CPTED strategy can deter many attackers before they even reach your building, let alone attempt to access the building.

&nbsp;

There are of course detection followed by response techniques that you can use to help in the case where you are unable to or it does not make sense to change weak building materials. These are covered below.

### Service Labels

![](images/ThreatTags/PreventionVERYEASY.png)

Just remove them. Do not make information publicly available that could be used against you.

### Sensitive Printed Matter

![](images/ThreatTags/PreventionAVERAGE.png)

1. Dumpster diving. Make sure your trash baskets only get used for trash that is not sensitive. Educate -> monitor -> test, repeat. All bins that could hold not only sensitive printed material, but any sensitive material whether shredded or not should be locked and fixed securely to something difficult to move. Destroy your own sensitive material and use shredders that shred more than one way into very hard to re-assemble pieces.
Not all paper shredders are created equal. Understand the pros and cons.
2. Printed matter left in insecure places: Your desk is insecure. Consider whether printing sensitive information is necessary. Is there a better way? If you have to print it and if it needs to be retained, then it should also be locked in some kind of safe storage that can not easily just be removed.
3. Printer Management: Most modern MFD’s have the capability to encrypt hard drives, which should be used whilst the device is in operation, however it should also be considered best practice to remove or [degausse](http://blog.binarymist.net/2013/03/17/erasing-data-from-your-drives/) MFD hard drives before the device leaves premise at end of life / lease.

There are many attack vectors represented here in the following countermeasures:

* Outside bins that lock
* High quality shredders
* easily accessible and convenient lockable storage for each individual staff member. Make sure they're very easy to use, else they won't be used.
* Policy and possible company culture change for all of the above points and any that I have missed
* Education -> monitor -> test, repeat

### RFID Tags {#physical-countermeasures-rfid}

![](images/ThreatTags/PreventionAVERAGE.png)

There are at least a couple of strategies to deal with RFID tag cloning.

1. Preventative.
    1. Multi-factor authentication. Something you are: Fingerprint for example. Something you know: PIN number or something else that only the actor would know. Something you have: The card for example.
    2. There are all sorts of methods that the chip manufacturers and security community are using to make the cloning more costly in terms of time and effort.
2. Detecting, reacting when it does happen. Methods such as:
    1. writing a new random number on the tags memory every time it is scanned. When a tag is scanned with the same id as another tag that is been issued a new random number which is also recorded by the back-end, the back-end knows there is a duplicate tag.
    2. Video surveillance of the area where front-end (tag) to back-end (reader) communications are made

Keep in mind though, as Bruce Schneier said: "_Detection works where prevention fails and detection is of no use without response_."  
As part of Identify Risks, you will need to apply the ranking techniques discussed in order to decide the Likelihood, Impact on what is important to your business and all the other factors that the threat modelling techniques walk you through.  
You can then use your ranking as input to the Countermeasures step. Thus helping you decide which, if any countermeasures are worth implementing.

### Computers Logged in and Unlocked {#physical-countermeasures-computers-logged-in-unlocked}

![](images/ThreatTags/PreventionEASY.png)

Most of what you can do here comes down to the people problem described in the chapter on [People](#people).

1. Educating your workers
2. Creating a [culture](http://blog.binarymist.net/2014/04/26/culture-in-the-work-place/) that thinks about security and includes it as part of who they are
3. Test your workers (come in after hours and check who has left monitors on). Measure the results. Make sure your investment is falling on fertile ground and actually taking root. Adjust your training and change techniques to address weak areas. Measure again. Keep iterating on this.
4. Again simple stuff, but from my experience, if your organisation falls into this bucket, until an attacker takes advantage of this, management seems blissfully unaware. So... If you see it, put your [change agent skills](http://blog.binarymist.net/2014/04/26/culture-in-the-work-place/#effecting-change) to work. This can take significant cultural change. Be patient and make that change move from ground up. It does not matter where you sit in the organisations hierarchy. If you do not really know where to start, I would recommend grabbing a copy of [Fearless Change](http://blog.binarymist.net/2013/06/22/ideas-for-more-effective-meetings-and-presentations/). It is not about meetings and presentations by the way. I have gained a lot of insight on how to gently change organisations and bring big improvements in many areas. I would also recommend [Nonviolent Communication](http://en.wikipedia.org/wiki/Nonviolent_Communication) by Marshall Rosenberg.
5. Getting buy-in from end users can often be a difficult task in this area. Often this can be due to an inherent distrust of management by line level staff. Any change of process that relates to security will result in negative feelings and questions such as “what has happened”, “what are they looking for now”, “I haven't done anything wrong” etc. The key word here is transparency. Right from the outset, management need to inform staff of plans, what processes are being put in place and why. What are the possible ramifications if these processes are not established etc. Also this discussion should be had first hand from senior management to line staff, not via a supervisor / team-leader etc. This removes an additional layer of suspicion.

### Networking Equipment

![](images/ThreatTags/PreventionAVERAGE.png)

Ideally these components should be stored in server cabinets which are locked, do not have the keys sitting in the locks or in obvious places and have all panels fitted. So often I have seen these cabinets wide open due to laziness or because no one knows where the keys are any-more. The server rooms should also be locked when no one is in them and closely guarded. Also consider using detection mechanisms. Movement detection devices and cameras that are set-up to capture and send alerts to someone that will notice them and respond. This way when your prevention fails. Your detection and response will save the day.

You will also need to think about company culture and whether this needs some work.

### Network Ports

![](images/ThreatTags/PreventionEASY.png)

Do not leave network ports active unless the devices using them are authorised and documented. Network ports should be audited regularly and all connected devices documented. Ideally executable documentation. Not static. What I mean by that is, for example document the devices using the ports within the switch or router itself. This way the documentation can not rot in terms of having the connected devices listed, it can however rot when devices are physically removed. This may require an auditing schedule.
As with [Transient Devices](#physical-countermeasures-transient-devices), use DHCP static mappings and have your DHCP server configured to deny unknown clients.

### Wi-Fi Access Points

![](images/ThreatTags/PreventionAVERAGE.png)

Discussed in more depth in the [Network](#network) chapter.

The “outside” vulnerability can be mitigated by the implementation of Li-Fi (Light Fidelity) systems. Li-Fi transmits high-speed data using visible light communication (VLC), and as the light waves cannot penetrate walls, external access becomes virtually impossible. Li-Fi AP’s also have an inherent short range which is essentially diffused by windows. The [speed benefits](http://www.sciencealert.com/li-fi-tested-in-the-real-world-for-the-first-time-is-100-times-faster-than-wi-fi) of Li-Fi are also compelling, many times faster than current Wi-Fi speeds. 

I usually use a pass-phrase of about 40 characters long, made up of as close as I can get to random characters which are a mix of alphanumeric, upper case, lower case and symbols. For some people it can be a little awkward. For those abiding by best practises of using password vaults, it should actually be easier than typing a short pass-phrase, because they will be copy -> pasting it. In this case there is no convenience lost, in fact if it feels inconvenient, this should push the user toward using a password vault, which they should be using already.

The [Transient Devices](#physical-countermeasures-transient-devices) section may also be of interest.

#### Hiding the SSID

![](images/ThreatTags/PreventionDIFFICULT.png)

Feel free to hide the SSID, but it adds little in the way of security and does add quite a bit in the way of inconvenience. 

#### Wi-Fi Protected Set-up (WPS)

![](images/ThreatTags/PreventionEASY.png)

* Do not purchase an AP with WPS
* If you already have one and the firmware allows you to turn WPS off then do so. Just be aware that sometimes you will turn this off and the firmware will ignore your choice, so be sure to test it or at least obtain a reliable configuration from the device. For example, I have worked with Cisco Aironet 1142 devices and their Web UI displays something completely different to the configuration when you print it out.
* Consider re-flashing the device with a firmware that does not have WPS.
* Research and purchase a device(s) that does not have WPS.

#### WPA2 and WPA

![](images/ThreatTags/PreventionEASY.png)

Make sure your AP is set-up to use one of the encryption algorithms considered to be secure enough today. Wi-Fi Protected Access 2 (WPA2), but not with the flawed WPS standard which undermines the security of WPA(2).  
Do not use WEP or TKIP, they are trivial to crack.

WPA, which was a stop gap solution created due to the insecurities found in WEP, is a significantly stronger protocol than its predecessor. WPA implements Temporal Key Integrity Protocol (TKIP) which takes the initiative of generating a new 128 bit key for each packet rather than using the same key for each packet which is WEPs downfall, thus mitigating replay type attacks.

WPA Also includes the message integrity checking algorithm "Michael" which is much stronger than the cyclic redundancy check (CRC) used by WEP.

WPA2 uses the Counter Mode Cipher Block Chaining Message Authentication Code Protocol (CCMP), which is an Advanced Encryption Standard (AES) based encryption mode which is still considered fairly secure in its own right.

You may have also noticed the pre-shared key (PSK) acronym related to WAP(2). It is simply in relation to a shared secrete (often called a pass phrase) that the AP clients know and is used for authentication.

### Transient Devices {#physical-countermeasures-transient-devices}

![](images/ThreatTags/PreventionAVERAGE.png)

Make sure that as part of a staff members exit interview, whether they be a permanent or contractor, that you do everything in your power to have them remove Wi-Fi credentials from their transient devices, or better still, have the network administrator remove their ability to connect to your network.

Be very careful who credentials are handed out to. Visitors etc. Also consider setting up an AP that has access to the internet alone. No access to your internal resources may work in some cases.

Use DHCP static mappings and have your DHCP server configured to deny unknown clients. There are other ways you can use to stop rouge wireless devices connecting to your network. Get creative if you want to, just make sure ex staff members can no longer access your assets (any assets). Aerohive Networks also have a "Private Pre-Shared Key" [Solution](http://www.aerohive.com/solutions/technology-behind-solution/ppsk).

Company policy and culture may require some work also.

### Lack of Visibility

![](images/ThreatTags/PreventionEASY.png)

#### Cameras, Sensors and Alarms

Detection is an important part of the over-all security of your premises. When your prevention fails, you are going to want to know about it so that you can react appropriately. Ideally surveillance systems should also be configured to send alerts to someone that is going to take notice of them. I have addressed some of the concerns around alerts that fail to trigger human reaction in regards to "Morale, Productivity and Engagement Killers" in the [People](#people) chapter. I have found ZoneMinder which is an open source video surveillance solution to be excellent at recording, detecting motion and providing events. You can then do what ever you like with the events, including email, SMS, push notifications, etc. Be prepared to get your hands dirty here though as this is an open and extensible platform. I have also noticed NodeMinder which was of interest to me, but at the time of writing this, is not being maintained, like so many NPM packages.

## 4. SSM Risks that Solution Causes

### Fortress Mentality

I do not see many risks associated with improving a premises internal security other than possible over confidence that may be produced as a by-product.

### Internal Doors and Cabinets Left Unlocked

The inconvenience of always having to go through the process of unlocking and locking can cause frustration and lack of understanding around why this process is necessary. Additional openness, empathy and education may be required by (servant leader) management to keep staff motivated and devoted to the cause.

### Insecure Doors and/or Windows

Possible lack of traversal and/or egress in some areas that are required, and sometimes only for service personal, if doors are locked or self locking. Make sure visitors and new workers know the escape routes and have unlocking mechanisms (keys, cards, codes) on them.

### Easily Penetrable Building Materials

Few risks are involved with increasing the quality (strength) of building materials. Some visual or other aspects of the building architecture may be affected if you also take into account security of building materials. It is another aspect that the architect has to think about. Costs may be more than they would have been otherwise, depending on what the "otherwise" entailed.

### Service Labels

Maybe someone will forget who services the appliances.

### Sensitive Printed Matter

People may rebel against policy if they are not treated the right way. Sometimes it is hard to see this. People are good at hiding their true feelings. I discuss strategies of getting the most out of your people in the [People](#people) chapter so that hidden lack of buy-in and engagement can be avoided.
As with security in all other areas, increasing it is likely to cost a little more and decrease some convenience. Although with thought and planning, this can become part of your company culture. People can be your strongest or your weakest defence. This is your choice. Cultural change can be implemented from any level. The most successfully being from the shop flaw.  
High quality shredders are not much more expensive than their lesser counter-parts.

### RFID Tags

With multi-factor authentication, the something you have or know parts could potentially be lost, forgotten. In a drastic case, the something you are could also be removed. Increasing security decreases convenience.

### Computers Logged in and Unlocked

Once this is addressed, there is a risk that staff will be starting to think about security in many areas, but this is the whole point right? This is really simple stuff. There is a risk that it takes a second longer to access your desktop.

### Networking Equipment

It may be a little inconvenient to have to unlock server cabinet doors and remove panels. There is a risk that this could produce frustration for those that do not fully understand why the policy is in place. If this is the case, the fault lies with the people pushing the policy. Check which part of the "educate -> monitor -> test" cycle is failing and improve the faulty component. Repeat the cycle again. Remember we are dealing with people here. People can be complex. Get to their level and do what ever it takes to gain understanding into their world, their problems and their frustrations. Empathise and build relationship. This alone will go a long way to getting them on your side.

### Network Ports

There may be some inconvenience encountered when new network components require a port connection. Schedules may be ignored or forgotten. Try adding some accountability and possibly electronic reminders. Get creative.

### Wi-Fi Access Points

In terms of having long and complex pass-phrases, I do not really see any risks here, other than what is already discussed that those without password vaults will be pushed toward using them. This should be looked at as a bonus. Yes there may be some initial frustration, but that would soon be replaced with a solution that is both convenient and that lifts the security standpoint of not just those using them but anyone related to the people using the password vaults, because unique credential sets become so much easier and ubiquitous.

#### Hiding the SSID

Possible false sense of security. Frustration caused due to not being able to easily initiate a contract with the AP. 

#### Wi-Fi Protected Set-up (WPS)

If you are used to using WPS to establish contracts, it may feel a little inconvenient for the first few times, but in doing so, you have removed an easy to compromise attack vector. Perhaps there is some risk of buy-in with the slight increase in effort in having to enter a pass-phrase. It often helps to get the understanding across if you can demonstrate how easy it is to compromise wireless security when WPS is employed. 

#### WPA2 and WPA

I am not sure that there is a risk here. Possible, although unlikely, you may not be using APs that support WPA2. I would actually see this as a blessing in disguise now though. If you were not aware of the insecurities of the older protocols, hopefully you know enough now to know you need to upgrade your wireless security.

### Transient Devices

There could be potential kickback due to the fact of tightening up policy and implementation of. People often do not like change. 

### Lack of Visibility

#### Cameras, Sensors and Alarms

Alarms may scare off a non determined attacker, but besides that, they generally just annoy neighbours due to the regular occurrence of false alarms. Do everything you can to minimise false alarms. Physically silent alarms that actually reach someone that is alert and that cares can be far more effective. Do what ever it takes to make sure the person supposed to be monitoring alarms and alerts does notice. Get creative if need be.

Possible complacency. We are under surveillance and have alarms, so we must be safe. Education may be required to make sure people understand that these are only one layer of defence and only as effective as the weakest link, which may be the person tasked with monitoring.

Consider the potential of an attacker avoiding, vandalising or removing any of these mechanisms.

## 5. SSM Costs and Trade-offs

### Fortress Mentality

There is often a lot of work that needs to be done internally. Work your way though the biggest wins that are the cheapest to implement first.

### Internal Doors and Cabinets Left Unlocked

There is a time cost and possibly costs of staff wondering whether the costs they have to pay associated with this policy are worthwhile. With these sorts of issues, solid relationships and empathy is important to be invested from those mandating the policies.

### Insecure Doors and/or Windows

There are trade-offs that need to be thought about and discussed in regards to what happens if people loose unlocking mechanisms and can not traverse the premises or exit in emergency. Coded locks may be an appropriate mechanism in this case, but then changing codes and assigning unique codes to people also need to be thought about.

### Easily Penetrable Building Materials

Consider where the lowest hanging fruit is in terms of your over-all security stature. This is why iterating on the threat model is important to build your intuition. Consider everything, then weigh up the importance of building materials for your situation. For example, if your internals are comprised of many layers of well thought out security and people are engaged, loyal and subscribed to well constructed policy, then your external will be less of an issue if it is breached. Before you can make this decision though, you will need to work through the various chapters gaining a good understanding of where your current security stand point is in each area.

### Service Labels

Record who the service agents are in a secure place that only those that need to know have access to.

### Sensitive Printed Matter

This decision needs to be made once you have a good understanding of the rest of your assets, risks and countermeasures, of all the possible attack vectors. In most cases the highest costs here will be in getting staff buy-in (firmly sold on organisational policy). Mandating policy may produce the appearance that people are on board, but in-fact it will have quite the opposite effect. Effective change comes through relationship.

### RFID Tags

You need to decide how much loss of convenience is worth the added security. Once you have threat modelled everything, you will be in a good place to make this judgement. Some convenience losses are not so much of a big deal and they may significantly increase security, thus making the change worth doing.

Potential increase in costs for a smarter back-end and possible addition of physical surveillance system. Although this could be a killing two birds with one stone scenario. Just bear in mind also, that increasing the smartness of the back-end also creates other possible attack vectors.

### Computers Logged in and Unlocked

This should be pretty much a no brainer. There are no real disadvantages to securing your session before you leave your work station at any time. Get into the habit. It only takes one weak link to get past a layer.

### Networking Equipment

Along with the training -> monitoring -> testing cycle, Adding detection mechanisms may be extra cost and overhead. Iterating on the threat model will help provide insight into where you can cut costs and where you may have to invest. Often it is a matter of moving expenditure rather than just adding it. Balancing out the areas that need it more than others. Over-all security can be increased in most cases by moving expenditure to areas that are more in need.

### Network Ports

An auditing schedule may cost some time to perform. Maybe you can piggy back it on another schedule that is already being carried out?

### Wi-Fi Access Points

A little effort to start using password vaults, possibly making it an organisation wide policy that staff need to use some type of password vault (not mandating which type, as existing users may have their preferences). Some cultural modification and over-all much increased security for very little time expenditure.

#### Hiding the SSID

Technically does not provide any increased security, but reduces convenience. May depend on how frequently new users are provided access.

#### Wi-Fi Protected Set-up (WPS)

A little increase in effort to establish a wireless connection. Those that are provided access should also be recorded as having access, discussed below in the [Transient Devices](#physical-costs-and-trade-offs-transient-devices) section

#### WPA2 and WPA

The costs of upgrading if you have to change your APs which is unlikely because we have had WPA2 support even in home grade APs for quite a few years now.

### Transient Devices {#physical-costs-and-trade-offs-transient-devices}

There may be a little more work in recording who access has been granted to and removed from. Consider whether the fact that you now have much better visibility of who has access to the network is worth the extra overhead. The fact that you can remove access from staff members, ex-staff members and guests and have a pretty good idea of who can access the network can be quite empowering and make it easier to track down unwanted visitors. 

### Lack of Visibility

#### Cameras, Sensors and Alarms

I have found CCTV systems using ZoneMinder an excellent cost effective physical surveillance tool-kit with many options for configuring to suite what ever scenario will work best for you. There will be some set-up time and you may have to pay for physical cameras if you do not already have them, computer hardware and video capture cards. The cameras can be expensive if you require high resolution images. Video capture cards and the other hardware is cheap. Lighting can be an issue as well. In dark areas, you may have to use infra-red lighting around the cameras (often come as part of the camera). Although movement sensors that switch flood lights on can also be an option in dark areas.
