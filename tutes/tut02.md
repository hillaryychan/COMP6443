# Tutorial 2

Common DNS record types and what do they do?

A     (Host address) translate hostname to IP address
AAAA  (IPv6 host address)
CNAME (Canonical name for an alias) alias a hostname to another name
MX    (Mail eXchange) specifies SMTP email server for the domain
NS    (Name Server) specifies DNS servers responsible for a zone
PTR   (Pointer) reverse DNS lookup allows a DNS resolver to provide an IP address and receive a hostname
SOA   (Start Of Authority) - stories record about a certain DNS zone
SRV   (location of service) - service discovery
TXT   (Descriptive text)
CERT  (Certificate record) stores certificates

What happens when you type [google.com](google.com) in the address bar

1. Check DNS cache record
2. If no record, make DNS query
    1. Get IP address back
3. Visit the website
4. Send HTTP request to server

How DNS queries work?

SPF records - what do they do?  
Sender Policy Framework

## nmap

Port scanning

TCP handshake
client ----SYN----> server
client <--SYN-ACK-- server
client ----ACK----> server

For port scanning the client just sends SYN to ports on a server
If the client receives a SYN-ACK it knows the port is open

``` sh
nmap barducks.gq
-p- # all the ports There are 65565 ports
```

* `-p<ports>` specifies which ports to scan
    * `-` specifies all ports
* `--top-ports`
* `-T[1-4]` specifies the speed. The most commonly used is `4`
* `-v` for verbose
* `-A` detects what OS is running, scrip running

Port states: `closed`, `filtered`, `open`
UDP takes longer cause its not a secure and require more testing to be certain its open

TODO:

* What is a spf record
* What happens when you types in an address bar
* How does a DNS query work?
* Difference between cookies and sessions
* Auth related vuln
* #bugbounty tips - hackerone
* ctf - natas, hack the box

IDOR - insecure direct object reference
