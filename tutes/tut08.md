# Tutorial 8

Content Review

* CSRF - force victim to perform an action on another webapp;  
    * Embedded code in the malicious site 'forces' the request or use a malicious link
    * Often prevented with randomised string called CSRF token
* Response splitting - inject values in header by abusing newline characters; e.g. `\n` and `\r`
* Click jacking - trick user to click hidden content using iframes and CSS trickery
Prevention by the server setting `X-Frame-Options`:
    * `X-Frame-Options: deny`
    * `X-Frame-Options: sameorigin`
    * `X-Frame-Options: allow-from https://realsite.com`
* CSP - enforce the 'where from' for what is running on a webapp; by default blocks -in-line JavaScript

Client side - no longer attacking server; trying to get payload to execute against another user.
