# url-based-nuclei-templates
Nuclei templates to run on urls

## ssrf-blind-host
This will inject the `interactsh` endpoint into the host header and check for an response. The video below is the inspiration beind creating the filter.
- https://www.youtube.com/watch?v=LRQuTEAwhc4&ab_channel=IfqyGifhaazhar

## dom-xss-web-message
This filter will check for `window.addEventListener('message'` in the response. This does not indicate that there is an xss, but that you need to look into this. There are certain sources and sinks that should not be used when using `window.addEventListener('message'`. The link below will teach you.
- https://portswigger.net/web-security/dom-based/controlling-the-web-message-source

## dom-xss
This filter will check for `href="javascript:alert(1)"` in the response when `javascript:alert(1)` is the source at location.search. Having a location.search value of `javascript:alert(1)` injected into an anchor `href` attribute sink will result in an XSS.
- https://portswigger.net/web-security/cross-site-scripting/dom-based
- https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-jquery-href-attribute-sink

## time-based-sql-injection
This filter will inject the payload `'0"XOR(if(now()=sysdate(),sleep(30),0))XOR"Z'`. If the response takes longer than 30 seconds, then the filter will trigger. False positive will occur.

## reflected-parameters
This filter checks for urls that are reflecting input. This can be done with tools such as `GXSS` and `dalfox`, but creating a nuclei template is easier.

## lfi-windows
This filter check for LFI on windows by searching for `win.ini` through directory traversal.

## lfi-linux
This filter check for LFI on linux by searching for `/etc/passwd` through directory traversal.

## lfi-j2ee
This filter check for LFI on j2ee by searching for `web.xml` through directory traversal.

## ssrf-blind
There is a similar template in nuclei fuzzing templates called `blind-ssrf.yaml`, but that template will only send a request if there is already a url with `https` in the query parameter. This template will send the request in all urls.
```
Nuclei blind-ssrf.yaml Template:
https://example.com/questions?url=https://indeed.com --> https://example.com/questions?url=https://{{interactsh}} --> request will be sent
https://example.com/questions?continue=about --> request will not be sent
```

```
My blind-ssrf.yaml Template:
https://example.com/questions?url=https://indeed.com --> https://example.com/questions?url=https://{{interactsh}} --> request will be sent
https://example.com/questions?continue=about --> https://example.com/questions?continue=https://{{interactsh}} --> request will be sent
```

## blind-xss
This template is used to spray for detect blind xss. You can use a website like `xss.report` to check if your payload fired or not. If you use this template make sure to replace the payloads with your `xss.report` URL. I don't want to report your findings :wink: 

## redirect
This template is used for finding open redirects. It is similar to the one in the nuclei fuzzing repository but this template will fuzz for open redirect only on the URL given (this is because I perform parameter mining before I feed URLs to templates). This template also has a few common open redirect bypasses. 

## blind-xss-user-agent
This template will inject blind xss payloads in user-agent header.

## reflected-headers
This template uses common header and checks if they are being reflected in the response.
