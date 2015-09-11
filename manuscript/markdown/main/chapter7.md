# Mobile {#mobile}

![10,000' view of Mobile Security](images/10000Mobile.gif)

_Todo_

Some resources to start with:  
https://bluebox.com/business/bluebox-and-nist-addressing-mobile-threats/  
https://media.blackhat.com/bh-us-12/Briefings/C_Miller/BH_US_12_Miller_NFC_attack_surface_WP.pdf

%% I'll probably use [NetHunter](https://www.kali.org/kali-linux-nethunter/) to get up and running quickly on a mobile device.

_Todo_

## 1. SSM Asset Identification
Take results from [higher level Asset Identification](#ssm-asset-identification). Remove any that are not applicable. Add any newly discovered.

* Taking the confidential business and client information from the "Starting with the 30,000' view" chapter, we have another set of attack vectors that threaten the information that may be carried around on laptops and mobile devices such as smart-phones, tablets and other devices, which can and do of course get taken away from an organisations premises. Many forms of confidential information may reside on or be accessible via these transient devices. Everything that could reside on your internal network could also reside on or be accessible from these transient devices.
* Email on any number of devices
* Data-stores: These could reside on a laptop being transported by a developer, sales person, executive, whoever really.
* Documents: Any number of these could reside on many types of transient device.
* VPN's, SSH tunnels terminating on a transient device, or even logged in web applications. It's very common for the owners of mobile devices to keep their applications logged in for convenience sake. Thus making mobile devices a very attractive asset, even if only to use as a stepping stone.
* User credentials. These could be stored in many forms. In documents, memory, credential data stores.  
* Identity information
  such as: Email addresses, physical addresses, phone numbers
* Credit card numbers and similar
* Reputation is again a biggie.

## 2. SSM Identify Risks

Kali Linux [NetHunter](http://www.nethunter.com/) can be very useful for identifying risks in the mobile landscape. [zANTI](https://www.zimperium.com/zanti-mobile-penetration-testing) can also be [very useful](https://forums.kali.org/showthread.php?23861-Tutorial-Easy-Beef-XSS-hook).

### Data Collection

Verint (a USA company) sells a cell phone tracking system called [SkyLock](http://apps.washingtonpost.com/g/page/business/skylock-product-description-2013/1276/) with a subtitle of "Locate. Track. Manipulate" to both corporations and governments worldwide. SkyLock not only finds people but also tracks them over time periods.

*The UK company Cobham sells a system that allows someone to send a "blind" call to a phone*. A blind call doesn't ring and is not detectable by casual visual inspection of the phone. "The blind call forces the phone to transmit on a certain frequency, allowing the sender to track that phone to within one meter"

Then there's [Infiltrator](http://infiltrator.mobi/defentek_infiltrator_real-time_global_tracking_technologies.html) from Defentek, a real-time global tracking system that *enables the end-user to monitor the targeted individual(s) activities and collect geo-location data to profile the subject(s) pattern of life and habits. This allows one too to derive to a plan of action or a motion for a warrant for further surveillance, investigation, apprehension, or decommissioning.* Infiltrator claims to be able to *locate and track any phone number in the world.* with abilities such as being able to infiltrate and be undetected by the network, carrier or the target.

Many of the applications installed on our phones are collecting your personal information such as location, sex and your phones unique identification number (UID). Applications such as Angry Birds and even the flash-light app. Even apps that deliver bible quotes.

Tobias Engel discussed in his [presentation](http://events.ccc.de/congress/2008/Fahrplan/events/2997.en.html) at the 25th Chaos Communication Congress how to locate mobile phones using SS7. His slide deck is [here](http://berlin.ccc.de/~tobias/25c3-locating-mobile-phones.pdf). Recording of the presentation [here](https://www.youtube.com/watch?v=lQ0I5tl0YLY). There are various open implementations that use the same technique such as Nicholas Skinner's [PHP application](http://www.ns-tech.co.uk/products/track-any-mobile/).

There are many offerings available to allow people to spy on other individuals activities and location in regards to mobile phones. Even though the CEO and maker of StealthGenie which was a mobile app used for spying, was [indicted and arrested](http://www.washingtonpost.com/business/technology/make-of-app-used-for-spying-indicted-in-virginia/2014/09/29/816b45b8-4805-11e4-a046-120a8a855cca_story.html) for selling it in the USA, there are many other options for doing the same like [HelloSpy](http://hellospy.com/homepage.aspx?lang=en-US), [Highster](http://www.highstermobi.com/), [MySpy](http://www.mspy.com/), [FlexiSPY](http://www.flexispy.com/)

The USA National Security Agency (NSA) and its UK counterpart, Government Communications Headquarters (GCHQ), use location data to track people.


## 3. SSM Countermeasures


## 4. SSM Risks that Solution Causes


## 5. SSM Costs and Trade-offs

