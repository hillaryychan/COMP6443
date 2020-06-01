# Getting Started

## Installing Certificates

1. Download the `*.p12` file.
2. In your browser settings, click "View Certificates"
3. Go to "Your Certificates" tab and click "Import..."
4. Select the `*.p12` file and enter password

## Browser Developer Tools

Accessing browser developer tools:

* Chrome: `top right dropdown >  More Tools > Developer Tools`
* Firefox: `top right dropdown >  Web Developer > Toggle Tools`

The keyboard shortcut is `ctrl+shift+i` for both browsers.

Through this you can edit html, see JavaScript output and via the network tab you can see network requests the page makes.

Relevant documentation:

* Chrome: <https://developers.google.com/web/tools/chrome-devtools/>
* Firefox: <https://developer.mozilla.org/son/docs/Tools>

It is useful to create another user in your browser. If you don't want your browser to remember anything locally, Incognito doesn't store local history but does not hide your traffic. Having a somewhat isolated environment for hacking is useful.

## Cookies

Extensions to edit cookies:

* Chrome: <https://chrome.google.com/webstore/detail/editthiscookie/fngmhnnpilhplaeedifhccceomclgfbg?hl=en>
* Firefox: <https://addons.mozilla.org/en-US/firefox/addon/edit-cookie/>

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

Your proxy will need a valid certificate to tell your computer "it's ok" and you're just hacking yourself. Install Burps CA Certificate following <https://portswigger.net/support/installing-burp-suites-ca-certificate-in-your-browser>

When your proxy and Burp Suite are activated and you try to visit any site, it should hang. The burp suite window will show the attempted request your browser made. You can forward the request to let it leave the computer.

Follow <https://portswigger.net/burp/documentation/desktop/options/tls#client-tls-certificates> to install your client certificate in Burp Suite. Backup the certificate from your browser to specify the certificate file.
