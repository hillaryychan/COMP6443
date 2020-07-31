# Tutorial 9

A bird's eye view:

* Enterprise Architecture
* Web Application Firewalls

## Enterprise Architecture

A way of logically thinking about components and flows/

Context determines the importance of a vulnerability

Know what you have so you can protect it

## Defence in Depth

Multiple layers of security controls and protection

Increase complexity for an attacker

## Security controls

Something we apply to a system in hopes of it detecting, alerting, preventing attacks

Jump host act as a proxy to broker requests to system. E.g. connecting to a VPN. Reduces attack surface.

Central key management - storing secrets, keys. How to manage them

## WAFs

Lots of vendors; imperva, akamai, f5, cloudflare, aws, azure

Goal:

* block hackers sending hakz
* log hackers sending hakz

Different flavours:

* cloud
* on premise/applicance

They usually:

* Inspect payload
* Block request if it sees certain patterns
* Change the payload to be safe

## Social Engineering

Psychological manipulation of people into performing actions or divulging confidential information. Confidence tricks (cons) are not new. Not a science, more *an art*.

Usually predicated on a pretext. Justification for a course of action that is not the real reason. A **well researched lie**

E.g.

* "Hey I really need to print this assignment cover sheet" *plugs in USB*
* "I'm here to fix the printer" *there is always one broken*
* "I need access to my husband's account" cue crying baby noises

### Foundational Techniques

Primarily focused around Robert Cialdini's six principle of persuasion.

1. Reciprocity - someone does something for you, you feel inclined to do something back in return
2. Liking - do stuff for people we like
3. Commitment & Consistency - don't like to challenge the status quo; public commitments are more binding. e.g. "You said that ...", "That doesn't seem fair, can I get that too?"
4. Authority - "Act like you are *meant to be there*". Societies have power structures, we are socialised to abide; clothes, titles, trappings/accessories, forged documents  
Note: **DO NOT IMPERSONATE LAW ENFORCEMENT**
5. Social Proof - tend to trust things other people trust; experts, ratings, peers, "wisdom in crowds"/bystander effect
6. Scarcity - we are drawn to *exclusive* and *rare* things. Urgency works really well. People make bad decisions under stress.

Other primitives:

* curiosity
* power of silence - people will try to fill it in
* LISTEN - people tell you what they need
* Asking ***"why?"*** to drill to the truth

Cyber space:

* social media
    * check-ins
    * default privacy settings
    * graph search
    * information is *everywhere*
* username checker: <https://namechk.com/>
* people search services: <https://pipl.com/>
* wayback machine: <https://archive.org/web/>
* ~~website registration~~ :'(  RIP GDPR
* OSINT
* much more

### Takeways

* Understand your flaws - catch yourself falling victim
    * are you getting too caught up in something?
    * are you feeling like you *have* to do something?
    * know when you are weakest
* Reduce the impact of **human errors** - we will make them
* Friendly scepticism
* Have we as technology designers failed parts of society? e.g. the elderly
