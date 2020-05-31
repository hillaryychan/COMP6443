# Getting Started

## Installing Certificates

1. Download the `*.p12` file.
2. In your browser settings, click "View Certificates"
3. Go to "Your Certificates" tab and click "Import..."
4. Select the `*.p12` file and enter password

Two free flags: `COMP6443{WELCOME_TO_COMP6843}` and `COMP6443{WELCOME_TO_COMP6443.ejUyMDk3NjA=.uR8lDwGIySCluuwoHKjceA==}`

## Browser Developer Tools

Accessing browser developer tools:

* Chrome: `top right dropdown >  More Tools > Developer Tools`
* Firefox: `top right dropdown >  Web Developer > Toggle Tools`

The keyboard shortcut is `ctrl+shift+i` for both browsers.

Through this you can edit html, see JavaScript output and via the network tab you can see network requests the page makes.

Relevant documentation:

* Chrome: <https://developers.google.com/web/tools/chrome-devtools/>
* Firefox: <https://developer.mozilla.org/son/docs/Tools>

## Opening Account

set value to "yes"
set checked="true"

Flag: `COMP6443{DEV_TOOL_WORKS_GREAT.ejUyMDk3NjA=.cbgOK5iYYlbPZ91vUQRigg==}`

It is useful to create another user in your browser. If you dont want your browser to remember anything locally, Incognito doesn't store local history but does not hide your traffic. Having a somewhat isolated environment for hacking is useful.

## Cookies

Extensions to edit cookies:

* Chrome: <https://chrome.google.com/webstore/detail/editthiscookie/fngmhnnpilhplaeedifhccceomclgfbg?hl=en>
* Firefox: <https://addons.mozilla.org/en-US/firefox/addon/edit-cookie/>

Set `lucky` cookie's value to `1`

Flag: `COMP6443{YUMMY.ejUyMDk3NjA=.03Sid76U91XA3IJN+VAdDA==}`

## Burp Suite

Burp Suite is a tool that sits as a proxy between your browser and the server so that you can see the actual traffic payload at a lower level of the network stack. It lets you modify the request/response before sending that to the server/browsers. You can also do some basic brute forcing and other things.

Burp Suite installation link: <https://portswigger.net/burp/communitydownload>

You need to set up a proxy so Burp Suite can step between you and servers. It is really useful to switch back and forth between using this proxy and not since it can slow down your Internet significantly. Here are some proxy switching extensions:

* Chrome: <https://chrome.google.com/webstore/detail/proxy-switchyomega/padekgcemlokbadohgkifijomclgjgif/related>
* Firefox: <https://addons.mozilla.org/en-US/firefox/addon/switchyomega/>

If you used the proxy switched suggested above do the following

1. Click on the proxy icon in the toolbar
2. Go to "Options"
3. Click on "proxy" under profiles
4. Edit the entry following  
Chrome: <https://support.portswigger.net/customer/portal/articles/1783065-configuring-chrome-to-work-with-burp>  
Firefox: <https://support.portswigger.net/customer/portal/articles/1783066-configuring-firefox-to-work-with-burp>
5. Click on "Apply changes" under actions on the left
