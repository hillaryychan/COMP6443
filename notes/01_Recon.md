# Recon

**Recon** (or **reconnaissance**) is the act on conducting preliminary exploration of a potential target.

## Threat Modelling

**Thread modelling** is a process by which potential threats, such as structural vulnerabilities or the absence of appropriate safeguards, can be identified, enumerated, and mitigations can be priorities. Threat modelling answers questions like "Where am I most vulnerable to attack?", "What are the most relevant threats?", and  "What do I need to do to safeguard against these threats?"

Generally speaking, it is the process of identifying weaknesses and prioritising fixes against these weaknesses

Threat modelling methods are used to create:

* an abstraction of the system
* profiles of potential attackers, including their goals and methods
* a catalog of potential threats that may arise

## The Modern Web

The modern web is implemented in a client-server model, where the client is a browser that requests, receives and displays Web objects, and the server sends objects in response to requests.

HTTP is a web protocol that sends requests and receives responses.

A HTTP request for www.news.com.au:

``` http
GET / HTTP/1.1
Host: www.news.com.au
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:76.0) Gecko/20100101 Firefox/76.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Language: en-GB,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: close
Cookie: nk=ab90da3e0150d5e72355933d25010d66
Upgrade-Insecure-Requests: 1
```

A HTTP response from www.news.com.au:

``` http
HTTP/1.1 200 OK
Server: nginx
Content-Type: text/html; charset=UTF-8
Set-Cookie: nk=ab90da3e0150d5e72355933d25010d66; expires=Fri, 02 Jun 2023 05:33:21 GMT; path=/; domain=news.com.au; SameSite=None; Secure;
X-Content-Type-Options: nosniff
Content-Security-Policy: block-all-mixed-content; style-src https: 'unsafe-inline'; script-src https: blob: 'unsafe-inline' 'unsafe-eval'; img-src https: data:; frame-src https:;
X-Content-Security-Policy: block-all-mixed-content; style-src https: 'unsafe-inline'; script-src https: blob: 'unsafe-inline' 'unsafe-eval'; img-src https: data:; frame-src https:;
X-Webkit-CSP: block-all-mixed-content; style-src https: 'unsafe-inline'; script-src https: blob: 'unsafe-inline' 'unsafe-eval'; img-src https: data:; frame-src https:;
is-https: true
Vary: User-Agent
X-ARRRG1: /blaize/decision-engine?path=https%3a%2f%2fwww.news.com.au%2f&blaizehost=cdn.theaustralian.newscorp.blaize.io&content_id=&session=ab90da3e0150d5e72355933d25010d66
X-ac: 1.syd _bur
X-XSS-Protection: 1
Vary: Accept-Encoding
Expires: Tue, 02 Jun 2020 05:33:21 GMT
Cache-Control: max-age=0, no-cache, no-store
Pragma: no-cache
Date: Tue, 02 Jun 2020 05:33:21 GMT
Connection: close
Connection: Transfer-Encoding
Content-Length: 161889

<!doctype html><html lang="en"><head><meta charset="utf-8"> ...
```

## DNS

The **Domain Name System (DNS)** is a naming system for computer, services or other resources connected to the Internet or a private network. It is commonly used to translate user-supplied hostnames to IP addresses and vice versa.

![DNS translation](../imgs/01-19_dns-translation.png)

Each device connected to the Internet has a unique IP address which other machines use to find the device.

### Subdomains

A **subdomain** is a domain that is part of another (main) domain.  
Basically an additional part to your main domain name. They are created to organise and navigate different sections of your websites. E.g. store.yourwebsite.com - 'store' is the subdomain, 'yourwebsite' is the primary domain and '.com' is the top level domain (TLD)

Subdomains are different from directories. E.g example.com/yn points to a directory within the example.com domain, not to a subdomain of example.com.

Finding subdomains is a useful for reconnaissance. Why?  
Because security is usually focused on the main domains, leaving subdomains to be less secure and more vulnerable.

![subdomain security](../imgs/01-20_subdomain-security.png)

We can obtain subdomains through the following methods:

* brute forcing - using a list of subdomains, we send requests to the DNS, if the DNS responds, we know the subdomain exists  
![subdomain bruteforce](../imgs/01-21_subdomain-bruteforce.png)
* reverse ip - identify hostnames that have DNS records associated with an IP address
![subdomain reverse ip](../imgs/01-22_subdomain-reverseip.png)
* search engines
    * Web Searches
        * inurl:, is a Google Search operator which can be used to filter Google's search results to identify low-hanging fruit  
        `site: www.mypage.com InURL: “my search term” filetype:pdf`
        * web.archive.org
    * Domain Searches
        * <https://dnsdumpster.com/>
        * <https://hackertarget.com/ip-tools/>
    * Certificates
        * crt.sh
        * Domains from shared certs

Here are some scripts that enumerate subdomains:

* [subbrute](https://github.com/TheRook/subbrute)
* [Sublist3r](https://github.com/aboul3la/Sublist3r)
* [knock](https://github.com/guelfoweb/knock)

Be sure to have a look at their implementation to better understand the tools.

Intuition is sometimes better than tools. Always manually look at the applications and get a feel for their functionality

### Content Enumeration

**Enumeration** is defined as a process which establishes an active connection to the target hosts to discover potential attack vectors in the system. It is used to gather the following information

* usernames, group names
* hostnames, machine names
* network resources
* network shares and services
* IP tables and routing tales

The gathered information is used to identify the vulnerabilities or weak points in system security and tries to exploit

Enumeration can be:

* **Active**: tries millions of combinations of words/characters
    * dirb
    * dirbuster
    * gobuster
    * fierce.pl
* **Passive**: watches for new URL’s as you browse a website
    * Burpsuite
    * LinkFinder
    * lots of open source alternatives

Enumeration pro tips:

* localisation (by a different developer)
* technology-specific things
    * Wordpress/Drupa Admin, Themes
    * Build a set of working exploits
* Databases/Data stores (e.g. s3 buckets)
* Legacy applications
* Mismatched technology stacks
* others

## Basic Tests

***Vulnerabilities arise when assumptions are challenges.***

For example, if we are given a prompt that asks for your name ,the expectation is that we enter a name with alphabetical characters.
However, we can enter other excellent names:

* Null byte
* Newline
* XML/JSON/SQL
* OS Commands
* Backticks (Unix)
* Large/small names
* Strange character sets

A quick informal "sniff" test for inputs is using **``‘”>1#--;`wget blah`\x00\nabc``** as input.  
This is **not** a definitive test of whether a website is safe.  
It's a test of whether a website makes any attempt to handle unexpected input.

* Does the HTML Layout break? You may have cross-site scripting
* Do you get a database error? You may have SQL Injection
* Does your call-back get pinged? Congratulations, assume it's compromised

(*Make sure to use a proxy - browsers may modify your request*)

## Securing The Perimeter

Fix the low-hanging fruit first:

* Delete content that isn’t necessary.
* Restrict access to non-hardened content.
* Test your applications, fix the bugs.

Change user behaviour:

* Use secure passwords (admin:admin is not ok).
* Patch your environment.

BeyondCorp <https://cloud.google.com/beyondcorp/>

Never blame users for unintentional/uninformed failure.

## Disrupting Nation State Hackers

Video link: <https://www.youtube.com/watch?v=bDJb8WOJYdA>

If you want to protect your network, you need to ***really*** know your network; know the devices, security, technology, everything inside it.

Intrusion phases:

**1. Reconnaissance** - understand the target  
There is a difference between what you intend your networks to use and what the network actually uses. Evaluate what you will use, lock down and disable what you won't be using

**2. Initial exploitation** - find a way to get inside the network.  
This can happen from

* spear fishing
* water holing
* exploiting a known CVE (common vulnerabilities and exposures)
* SQL injection
* exploiting zero day

The most common intrusions are

* email - user clicks on something they shouldn't've
* website - malicious website
* removable media - inserting contaminated media

Defence mechanisms:

* Do not rely on the user to make the right decisions
* anti-exploitation features like [Microsoft EMET](https://support.microsoft.com/en-au/help/2458544/the-enhanced-mitigation-experience-toolkit)
* Take advantage of software improvements (preferably as a background activity outside the users control)
* Use a secure host baseline
* Have processes and plans to understand what is normal for your network; e.g. if someone has credentials re they operating in the norm for their credentials
* Segmenting off portions of a network, whitelisting
* Don't hardcode and include things in scripts
* Enable logs, and **look at them**

**3. Establish persistence** - being in the network is not enough. Stay in the network, privilege escalate, find run keys, scripts etc.

Defence mechanisms:

* Segmenting off portions of a network
* Application whitelisting

**4. Install Tools** - bring in tools that will do the work

Defence mechanisms:

* Reputation services - every software that wants to run on your machine gets hashed and pushed to the cloud

**5. Move Laterally** - find the things you need to find

Defence mechanisms:

* Network segmentation
* Monitoring
* Access privileges - limit admin privileges, segment accesses, enforce 2FA
* Manage trust relationships between accounts

**6. Collect exfil and exploit** - once your in the network, get what you need and get out undetected

Defence mechanisms:

* Off-site backups
* Have a plan to deal with data corruption, data manipulation or data destruction
* Differentiate between criminals and nation-state intruders
* Defend, improve, evaluate
