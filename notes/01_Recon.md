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

The **Domain Name System (DNS)** is a naming system for computer, services or other resources connected to the Internet or a private network.

### Subdomains

We can obtain subdomains through the following methods:

* brute forcing
* reverse ip
* search engines
Web Searches
* inurl:, filetype: to identify low-hanging fruit
* web.archive.org
Domain Searches
* <https://dnsdumpster.com/>
* <https://hackertarget.com/ip-tools/>
Certificates
* crt.sh
* Domains from shared certs

Intuition is sometimes better than tools. Always manually look

### Enumerating Content

Active: tries millions of
combinations of words /
characters

* dirb
* dirbuster
* gobuster
* fierce.pl

Passive: watches for new
URL’s as you browse a website

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

Vulnerabilities arise when assumptions are challenges.

For example, if we are given a prompt that asks for your name ,the expectation is that we enter a name with alphabetical characters.
However, we can enter other excellent names:

* Null byte
* Newline
* XML/JSON/SQL
* OS Commands
* Backticks (Unix)
* Large/small names
* Strange character sets

A quick informal "sniff" test for inputs is using **`‘”>1#--;``wget blah``\x00\nabc`** as input.  
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

Never blame users for unintentional / uninformed failure.
