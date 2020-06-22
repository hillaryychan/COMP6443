# Authentication, Session Management and Authorisation

## Overview

1. Authentication - is the user who they claim to be? If not then 401 error
2. Session Management - is it still that user?
3. Access Control - is that user allowed to access this thing?

### What Happens When You Type a URL in Your Browser?

The browser cache is checked
OS cache, followed by router cache

1. You enter a URL into a browser
2. The browser obtains the IP address of the domain name by
    1. Checking the cache for a DNS record to find the corresponding IP address  
    Caches include:
        1. browser cache
        2. OS cache
        3. router cache
        4. DNS server cache
    2. Looking up the IP address for the domain name via DNS
3. The browser initiates TCP/IP connection to the server (SYN, SYNACK, ACK)
4. The browser sends a HTTP request
5. The server sends a HTTP response
6. The browser begins rendering the HTML
7. The browser sends requests for additional objects embedded in HTML (images, css, JavaScript) and repeats steps 3-5.
8. Once the page is loaded, the browser sends further async requests as needed

## Web Authentication

In 2020 web authentication is usually done with the following methods:

* Username / Password
    * Password reset via email
    * 2FA: SMS, Token, Apps (incl TOTP)
    * Active vs Passive 2FA
* Authentication can be delegated (e.g. SSO, Oauth, JWT)
    * OAuth - a standard that works over HTTPS and authorises devices, APIs, servers, and applications with **access tokens** instead of credentials.  
    e.g. Logging onto a website using another website's/service's logon
    * JWT - a standard that defines a way for securely transmitting information between parties as a JSON object. Information can be verified and trusted because it is digitally signed.
* CAPTCHAs Sometimes

### Passwords

"The ASD's investigation found that Internet-facing services still had their default passwords, `admin:admin`, and `guest:guest`"

*Password security is a "people problem".*

When it comes to password resets we need to be mindful of the following things:

* is the reset link generated securely
* are "security" questions really secure?
    * can I get them off a user's Facebook?
    * can I Google the answers?
    * how many attempts do I get to answer these questions

Passwords obtained from previous data breaches can be found on [haveibeenpwnd.com](https://haveibeenpwned.com/Passwords)

[Password auditing](https://youtu.be/IchpQBbGbrE) also reveals that many user passwords can easily be found by matching hashes with rainbow tables (a precomputed dictionary of plaintext passwords and their corresponding hash values).

The 2019 NIST password guidelines recommends:

* 8 character min (human) otherwise 6 character min
* Support at least 64 characters max length
* Support All ASCII characters (incl 0x20)
* **NO** truncation of password when processed
* Allow at least 10 password attempts before lockout
* No SMS for 2FA (one-time password from an app)

Additionally:

* Check password with known dictionaries
* No complexity requirements (as it makes passwords harder to remember)
* No password expiration period
* No password hints
* No knowledge-based authentication (no questions) (these are easily obtainable via social engineering)

A study was conducted, notifying users with weak passwords to change their passwords. Here are the results

![password change study](../imgs/2-11_password-change.png)

### Attacking Passwords

When attacking passwords **brute force** is the ***best force***.

* Attempt logins with common passwords
* Try known email + password combinations from previous breaches
    * **Brute force** - one user, many passwords
    * **Credential stuffing** - many users, many passwords  
* Enumeration via information disclosure  
Error messages such as "Login failed: invalid username" inform us that the user does exists, while error messages like "Login failed: invalid username or password" make it more difficult to discern any information.

Note: the difference between brute force attacks and credential stuffing  
Brute force attacks attempt to guess passwords with no context or clues, using characters at random sometimes combined with common password suggestions.  
Credential stuffing uses credentials obtained from a data breach on one service are used to attempt to log in to another unrelated service.

Defences against these include:

* Login rate-limiting and lockouts
    * CAPTCHA
    * Lockouts (e.g. iPhone)
* Proactive monitoring
* User communication

### Man-in-the-middle Attacks

A **man-in-the-middle attack (MITM)** is an attack where the attacker secretly relays and possibly alters the communications between two parties who believe they are directly communicating with each other.

One example of a MITM attack is active ***eavesdropping***. We force a victim's browser into communicating with an adversary in plain-text over HTTP, and the adversary proxies the modified content from an HTTPS server.  
WiFi PineApple is a pentesting device that can perform MITM attacks

#### MITM Defences

**Transport layer security (TLS)** is used as a widely adopted security protocol to facilitate privacy and data security for communication over the Internet.  
HTTPS is an implementation of TLS encryption on top of the HTTP protocol. TLS is primarily used to encrypt communications between web applications and servers, such as web browsers loading a website. It evolved from a previous encryption protocol called Secure Socket Layer (SSL).

There are three main components to TLS:

* **Encryption**: hides the data being transferred from third parties
* **Authentication**: ensures that the parties exchanging information are who they claim to be
* **Integrity**: verifies that the data has not been forged or tampered with

**HTTP Strict Transport Security (HSTS)** is a web security policy mechanism that helps to protect websites against MITM attacks. It informs user agents and web browsers ***how*** to handle its connection through a response header. This sets the [Strict-Transport-Security policy field parameter](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Strict-Transport-Security), forcing those connections over HTTPS encryption, disregarding any script's calls to load any resource in that domain over HTTP.

## Session Management

The evolution of session management:

![session management](../imgs/02-19_session-mgmt.jpg)

We can see that session management has gotten complex over time...

HTTP is a ***stateless protocol***. Using cookies allow us to track the state of the application.

A **session** is a collection of data stored on the ***server*** which can retrieved by a unique id (usually a cookie). Whenever a session is created, a cookie containing the unique session id is stored on the user's computer and returned with every request to the server. Sessions can exists regardless of whether you are logged in or not as they can exist between your browser and the server.

A session ends when the user closes the browser or after leaving the site, the server will terminate the session after a predetermined period of time, commonly 30 minutes duration

**Cookies** are small text files stored on the ***client*** computer and they are kept tracking and identification purposes. Once a cookie has been set, all page requests that follow return the cookie name and value. A cookie can only be read from the domain it has been issued from.

Some key differences to note between cookies and sessions:

* Cookies are client-side files that contain user information, while Sessions are server-side files that contain user information.
* A cookie is not dependent on a session, but a session is dependent on a cookie.
* A cookie expires depending on the lifetime you set for it, while a session ends when a user closes his/her browser.
* The maximum cookie size is 4KB whereas in session, you can store as much data as you like for a session

### Anatomy of a Cookie

The main way we store session information is in a cookie.

The server sends to the client:

``` http
Set-Cookie: SSID=abcdef; Domain=lol.com; Expires=Mon, 20 Jan 2020 20:20:20 GMT; Secure; HttpOnly

name=value   the data to store
Domain       specifies the (sub)domain that the cookie belongs to
Expires      date when the cookie should be deleted
Secure       only send the cookie over secure connections (i.e. HTTPS)
HttpOnly     disable access to the cookie from JavaScript
```

The client sends to the server:

``` http
Cookie: country=aus; SSID=abcdef
```

### Attacking Sessions

* Session Creation
    * How are sessions created? Can I fake my own session?
    * Can I attack the PRNG, and generate my own cookie?
    * Can I “fixate” a session?
* Session Handling / Transfer / Usage
    * Can I steal the cookie through XSS (No “HttpOnly” flag?)
    * Can I steal the cookie through redirecting to HTTPS.
    * What information does the site trust the user to provide?
* Session Clean-up
    * What happens when I click “log out”?
    * Under what conditions is a session actually destroyed? What happens then?
    * Do sessions time out correctly?

#### Cross-Site Request Forgery

**Cross-Site Request Forgery (CSRF)** is an attack that forces an end user to execute unwanted actions on a web application in which they're currently authenticated.

CSRF attacks work by sending a rogue HTTP request from an authenticated user's browser to the application, which then commits a transaction without authorisation given by the target user. As long as the user is authenticated and a meaningful HTTP request is sent by the user's browser to a target application, the application does not know if the origin of the request is a valid transaction or a link clicked by the user (that was, say in an email) while the user is authenticated to the application.

Example of a HTTP POST to a ticket vendor to purchase a number of tickets

``` http
POST http://TicketMeister.com/Buy_ticket.htm HTTP/1.1
Host: ticketmeister
User-Agent: Mozilla/5.0 (Macintosh; U; PPC Mac OS X Mach-O;) Firefox/1.4.1
Cookie: JSPSESSIONID=34JHURHD894LOP04957HR49I3JE383940123K

ticketId=ATHX1138&to=PO BOX 1198 DUBLIN 2&amount=10&date=11042008
```

The response of the vendor is to acknowledge the purchase of the tickets:

``` http
HTTP/1.0 200 OK
Date: Fri, 02 May 2008 10:01:20
GMT Server: IBM_HTTP_Server
Content-Type: text/xml;charset=ISO-8859-1
Content-Language: en-US
X-Cache: MISS from app-proxy-2.proxy.ie
Connection: close

 Ticket Purchased, Thank you for your custom.
```

#### CSRF Defences

**CSRF Tokens** - with every request, you send a token to confirm you are running from a site the server is expecting.  
This can be bypassed with XSS and relies on secure token and consistent rolling + usage (and developer mistakes...)

**Compare Source and Target Origin** - some headers can't be messed with by code running on the browser

### Securing Sessions

* Minimise your attack surface - your session should be managed on the server side where possible, using a single session token
* Mostly mechanical fixes
    * Don't let people steal your tokens (urls, HTTP, etc)
    * Don't let people re-use tokens (expire them properly, log out)
    * Don't let people generate tokens (secure PRNG, avoid rolling your own crypto, don't allow users to supply tokens)
* Attention to detail is **key**

## Access Control (Authorisation)

Types of (web) access control:

* security through obscurity
* one-off access control
* rule-based access control

**Horizontal access control** is making sure one user can't access a different user's data without permission.

``` txt
http://bank.com/statement.php?user_id=12078
```

**Vertical access control** is making sure only administrative users can access administrative content.  
Attacking vertical access control is commonly known as privilege escalation

``` txt
http://bank.com/admin.php
```

### Attacking Access Control

* bypass access control by accessing privileges from least protected domains.
* `robots.txt` can reveal the levels of access control especially in regards to what should and shouldn't be accessed (not just by bots but users)
* copy legitimate users
* actual testing

    ![testing access control](../imgs/2-36_testing-access-control.png)

#### IDOR

**Insecure Direct Object References (IDOR)** are a type of access control vulnerability, when an application uses user-supplied input to access objects directly.

Often variables such as `id`, `pid`, `uid` are stored in headers, cookies and even urls. An attacker can access, edit or delete any of another user's objects by changing the values.

### Better Access Control

Using server-side validation:

![server access control](../imgs/2-37_server-access-control.png)

Using client-side access control:

![client access control](../imgs/2-38_client-access-control.png)
