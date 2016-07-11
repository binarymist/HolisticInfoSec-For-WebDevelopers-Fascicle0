# Introduction

%% An introduction deals with the subject of the book, supplementing and introducing the text and indicating a point of view to be adopted by the reader. The introduction usually forms a part of the text [and the text numbering system]; the preface does not." (In other words, the arabic numbering of the book (1,2,3) starts with the introduction, if there is one. The other front matter takes i, ii, iii, etc.)

In-depth guidance for Web Developers, Engineers, Architects and their teams, based on 25 years experience architecting, engineering, breaking and redesigning physical and technological systems then repeating the cycle iteratively.

Holistic InfoSec For Web Developers is focused toward agile teams and the realisation that high quality doesn't have to be expensive, providing its introduced at the right points in the Development Life Cycle (DLC). I focus on bringing quality building practises into each Sprint as part of the Teams Definition of Done (DoD), rather than waiting until going-live when the cost of fixing defects is at its highest.

Traditionally, security has been applied at the end of projects where it is the most costly in the development life cycle to re-design and re-deploy. This is why security, and in fact quality in general, is often seen as being too expensive and corners are cut. I will discuss many processes and practises in chapter 4 to reduce the focus of labour intensive and costly security assessments, and tests being performed by deeply specialised security consultants, to techniques and activities that can be crafted by the Development Team and carried out within each Sprint. Effectively removing defects as they are being introduced.

The processes and practices I am going to introduce will help you reduce the most likely to be compromised security defects first, at the earliest possible point in time, right when they are introduced. Iterate on Design -> Build -> Break, at every point of the development life cycle, including within each Sprint for each Product Backlog Item (PBI) that is pulled into work in progress (WIP). 

We become good at what we do by failing fast in development, fixing it, then trying again. This same strategy applies to all areas of life.

W> Don't wait until you're on the stage, where the cost of your mistakes is at its highest.

When it comes to providing countermeasures to the identified risks, measuring the security posture of an application or network is the step before. The best defence against an attacker is offence. This means your best defence is to have someone with your best interests, either someone employed or contracted by you when discussing your assets, who assess the vulnerabilities of your assets and attempts to exploit them.

Each of the topic chapters (as shown on the cover) utilise a five step threat modelling process not dissimilar to Bruce Schneier's [Sensible Security Model](http://www.win.tue.nl/~wstomv/quotes/beyond-fear.html) (SSM), in which I take you the reader through the five steps for the specific topic.

Asset Identification (Step 1): Provides insight on what your assets are. This is not always obvious at first. By studying our adversaries; their behaviours and goals, what they are attempting to obtain from you and how they go about acquiring it, assists us in defining what our assets are. These are the items we want to protect. I provide many examples throughout the book.

Identify Risks (Step 2): By starting to understand what our assets are, we are able to start thinking about the possible risks to each of them are. Throughout this book, I reveal the different agendas of your attackers, what their goals are and the types of attacks they carry out to achieve them. We will study their attack life cycle which is covered in the Penetration Testing subsection of the Process and Practises chapter, their tools, techniques and strategies for exploiting weaknesses in your defences. By beating your attackers to your weaknesses, we are able to determine where and what they are, and mitigate them before your adversaries can exploit them. We will also work through many hands on attacks to provide you with context of how to start building up countermeasures.

Countermeasures (Step 3): Once we have a fairly good idea of the risks to your assets, we will explore many countermeasures. These are then converted into security focussed Product Backlog Items, which you will work together with your Product Owner in ordering them within the Scrum Product Backlog, based on the lowest hanging fruit for an attacker being the items nearest the top of the Backlog. Your Scrum team will pull the highest rating PBI(s) into the Sprint Backlog at Sprint Planning. 

Risks that Solution Causes (Step 4): There will be new risks that the countermeasures introduce. We will work through what these might be for every countermeasure identified and how to recognise them. This helps us feed into the last step in which we make trade-offs based on what we learn from this step.

Costs and Trade-offs (Step 5): We look at some techniques for establishing what the costs of the security solutions may be and then discuss many trade-offs. This encapsulates the essence of pragmatism. These steps are not hard and fast. We will learn more as we work though them and we will frequently revisit previous steps and refine our Product Backlog Items, just as the Scrum Team refines any PBI as it approaches the top of the Backlog to be pulled into a Sprint.

The general approach to reading this book, is to iterate on the 30,000' view, which is covered in the first chapter. Then iterate on each of the 10,000' views that are applicable for your specific domain and systems. The Tooling Setup chapter will establish your tool-box which is to be used throughout this book. In the Process and Practises chapter we take learnings from the attackers perspective and apply them to the Scrum Teams work-flow

I have used a similar graphic set that the OWASP Top 10 uses for vulnerabilities through out this book for the risk in the following vein:  
Exploitability: [EASY|AVERAGE|DIFFICULT|VERY DIFFICULT]  
Prevalence: [VERY WIDESPREAD|WIDESPREAD|COMMON|UNCOMMON]  
Detectability: [DIFFICULT|AVERAGE|EASY|VERY EASY]  
Impact: [SEVERE|MODERATE|LOW]  

Then for the countermeasures again following OWASP's lead:  
Prevention: [DIFFICULT|AVERAGE|EASY|VERY EASY].

Where ever you see the following fiddling devil. It means it's hands on attack sequence time:

![](images/HandsOnHack.png)

W> This is a Warning.

T> This is a Tip.

I> This is extra information.

