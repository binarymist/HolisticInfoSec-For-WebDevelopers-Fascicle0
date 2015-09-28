# Cloud {#cloud}

![10,000' view of Cloud and In-house Cloud Security](images/10000Cloud.gif)

See [VPS](#vps), as it has a lot of similarities due to the fact that in many cases your VPS may be on someone else's hardware and in their control.

%% Using cloud services can be a good thing especially for smaller businesses and start ups, but before the decission is made as to whether to use an external cloud provider or whether to use or create your own, there are some very important considerations to be made.

%% Using IaaS and even more so PaaS can provide great productivity gains, but everything comes at a cost. You don't get productivity gains for free. You will be sacrificing something and usually that something is at least security. You no longer have control of your data.

%% If you decide to use an external cloud provider, you need to be aware that what ever goes into the cloud is almost completly out of your control and you can not pull it back once it has been released, as you have no idea whether the existing data is really removed from the cloud.

%% http://stackoverflow.com/questions/9802259/why-do-people-use-heroku-when-aws-is-present-whats-distinguishing-about-heroku

%% If you deal with sensitive customer data, then you have an ethical and legal responsibility for it. If you are putting sensitive data in the cloud then you could very well be being irresponsible with your responsibility.

%% 

## 1. SSM Asset Identification
Take results from [higher level Asset Identification](#asset-identification). Remove any that are not applicable. Add any newly discovered.

_Todo_

Flesh this out. Pull from research project that the following was based on:

https://speakerdeck.com/binarymist/does-your-cloud-solution-look-like-a-mushroom

## 2. SSM Identify Risks {#cloud-identify-risks}

Go through same process as we did at the [top level](#identify-risks), but for your VPS(s).

### Questions to Ask Your CSP {#cloud-identify-risks-questions-to-ask-your-csp}

The following is an excellent set of ten must-answer questions from [DarkReading](http://www.darkreading.com/) to throw at your CSP as part of your threat modelling before (or even after) you sign their service agreement.  
Most of these questions were already part of my [Cloud vs In-house talk](http://blog.binarymist.net/presentations-publications/#does-your-cloud-solution-look-like-a-mushroom) at Saturn before I saw them. I'd recommend using these as a basis for identifying risks that may be important for you to consider. Then you should be well armed to come up with countermeasures and possibly think of additional risks.

#### Do You Keep a Signed Audit Trail of Which Users Performed What Actions When, Both Through Your UI & API?
"_It's important to help protect against both mistaken and malicious actions when users know there is an audit trail, they will act with greater potential to detail, and also be dissuaded from using the platform as a vehicle for an attack. Having an audit trail is also helpful for troubleshooting purposes and root cause analysis._"

> Bernard Sanders, CTO, CloudBolt Software

#### What is My Role & Your Role in the Protection of My Data?
"_Understanding that enterprises have to play a critical role in protecting their own data and how that data is accessed, even if leveraging a cloud provider, is critical for risk management. Most cloud providers will require a shared responsibility for security and enterprises cannot assume the provider is liable for data breaches._"

> Rehan Jalil, CEO, Elastica

#### Do You Encrypt All Data Transmissions, Including All Server-To-Server Data Transmissions, Within Data Centers?
"_Security is only as strong as the weakest link. While it is very common to encrypt the traffic between the customer and the service provider in order to ensure integrity and confidentiality, it is less common for service providers to encrypt intra-server communications within the companies own perimeter. Too often attackers are able to exploit this type of weakness once a single breach in the perimeter has occurred._"

> Paul Hill, senior consultant, SystemExperts

#### What Access do You Provide to Logs?
"_As simple as it sounds, access to logs should be one of the top concerns when evaluating providers. End users are not going to get the rich log information set that they would get from the server in their data center as they will get from a cloud provider and the organization must carefully consider what information they will and will not obtain from the provider. While some information may not be relevant to the organization, it is possible that other critical pieces might not be revealed and if necessary the organization should try to negotiate relevant log access early on._"

> Rob Ayoub, research director, NSS Labs

#### What is Your Termination or 'Exit Process' for Ensuring Successful Transition from your Services to an Alternative Offering?
"_Let’s face it. Not all relationships can last forever. That said, companies need to review how to gracefully and effectively exit a relationship with a cloud provider. It’s important for organizations to have the following included in their contract with their cloud provider:_

* _How the provider will assist with the transition, including providing the company’s data back to them or a third party in an effective manner._

* _What the provider’s destruction or electronic shredding policies are so the company can have evidence that its data is no longer resident on the provider’s systems and, therefore, not subject to attack or e-discovery._

* _That the provider has independent third parties that review and certify the efficacy of their exit procedures._"

> Renee Guttmann, vice president, Office of the CISO, Accuvant


#### Where do Your Servers, Processes, & Data Physically Reside?
"_Although cloud computing is often promoted as a borderless construct, cloud providers must house all of your organization’s processes and data in real countries, which have varying legal requirements for data privacy and security. Be aware of the requirements of both your home country and the country where your assets will be hosted._"

> Stephen Ellis, manager, iSIGHT Partners

#### Who Can View Enterprise Data in the Cloud?
"_Just like in an internal data center, there will be support staff who will maintain the cloud provider infrastructure. Understand which of these personnel can see your data. What internal controls are in place to prevent unauthorized viewing, copying or emailing of customer information?_"

> Danelle Au, vice president of strategy, Adallom

#### What is Your Service Level Agreement (SLA) for Uptime?
"_Many providers offer a 99.9% uptime, which equates to nearly 45 minutes of unscheduled downtime per month. Even when the SLA is breached, a 'credit' issued to your account is only a percentage of the monthly fee - not anywhere near to the downtime cost to your business. Selecting a cloud provider that offers the right uptime guarantee is critical in finding the right solution for your specific company’s needs._"

> Nick Zeigler, senior cloud architect, All Covered, a division of Konica Minolta Business Solutions

#### Do you have ISO27001:2013 Certification? & if you do, What is Within its Scope?
"_This question is aimed at finding out if a cloud provider has achieved a recognized information security standard and if their entire business is within the scope of the certification—in other words, their business systems and operations, and operational platforms, rather than just one or the other._"

> Orlando Scott-Cowley, cyber security specialist, Mimecast

#### Do You Allow Customers to Perform Scheduled Penetration Tests of Either the Production Environment or a Designated Testing Environment?
"_Penetration testing is a common method used by companies to ensure their systems are well defended from attacks. Cloud service providers that allow customers to perform such testing are willing to be transparent about their security practices and also likely to be confident that their systems are well secured._"

> Paul Hill, senior consultant, SystemExperts

## 3. SSM Countermeasures

Once you have sprung the questions from the ["Questions to Ask Your CSP"](#cloud-identify-risks-questions-to-ask-your-csp) section on your service provider and received their answers, you will be in a good position to feed these into the next stages.

### Lack of Visibility

#### Monitoring

##### Statistics Graphing:

_Todo_
collectd and graphite on OpenStack

%% Plugins for collecting OpenStack metrics: https://github.com/catalyst/collectd-openstack

%% Nagios, Graphite, Collectd for OpenStack.


## 4. SSM Risks that Solution Causes
> Are there any? If so what are they?

## 5. SSM Costs and Trade-offs
> An exercise for the reader. What are they?

