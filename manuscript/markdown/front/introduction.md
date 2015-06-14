# Introduction

Target Audience:

* Software Architects
* Software Developers/Engineers
* Product Owners

If your looking to save money and save face due to potential embarrassment because your business assets have been compromised, continue reading. If your looking to become better at delivering secure solutions, continue reading.

Traditionally security has often been applied at the end of projects where it's most costly in the development life cycle to re-design and re-deploy. This is why security, in fact quality in general, is often seen as being to expensive and corners are cut.

Moving much of the focus of labour intensive and costly security assessments and tests being performed by deeply specialised security consultants to automated approaches that can be crafted by the Development Team within each Sprint.

The processes and practices I'm going to introduce will help you reduce (the most likely to be compromised (thus removing wasted effort)) security defects at the earliest possible point in time. Right where they are introduced. Iterate on Design -> Build -> Break, at every point of the development life cycle. Including within each Sprint for each Product Backlog Item (PBI) that's pulled into work in progress (WIP). We become good at what we do by failing fast in development, fixing it, then trying again. This same strategy applies to all areas of life.

> Don't wait until you're on the stage, where the cost of your mistakes are at their highest.

<br>

The crux of the Bruce Schneier Sensible Security Model (SSM) can be found here.

"MS" identifies a Microsoft resource. "OWASP" identifies an "Open Web Application Security Project" resource.

Iterate on the 30,000' view, then iterate on each of the 10,000' views that are applicable for your specific domain and system.

I've used a similar graphic as the OWASP Top 10 uses for vulnerabilities through out the book for the risk in the following vein:  
Exploitability: [EASY|AVERAGE|DIFFICULT|VERY DIFFICULT]  
Prevalence: [VERY WIDESPREAD|WIDESPREAD|COMMON|UNCOMMON]  
Detectability: [DIFFICULT|AVERAGE|EASY|VERY EASY]  
Impact: [SEVERE|MODERATE|LOW]  

Then for the countermeasures again following OWASP's lead:  
Prevention: [DIFFICULT|AVERAGE|EASY|VERY EASY].

Where ever you see the following fiddling devil. It means it's hands on attack demo time:

![](images/HandsOnHack.png)
