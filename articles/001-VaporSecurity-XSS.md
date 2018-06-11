# XSS 

  
## What is Cross-Site Scripting (XSS)?
Cross-Site Scripting (XSS) is a code injection vulnerability that allows an attacker to run malicious scripts on a victim’s browser. These scripts allow an attacker to perform any action on behalf of the user, access sensitive data, and modify page content. Additionally, the scripts are run in the context of the vulnerable page and therefore are trusted and bypass the browser's Same Origin Policy (SOP) protections. 

### Types of XSS
* **Reflected XSS** is immediately (non-persistent) reflected off the server and run in the client’s browser. It is commonly exploited as a GET request but can also be a POST request.
* **Stored XSS** is stored on the server (persistent) and executed when the stored exploit is retrieved. 
* **DOM Based XSS** is exploited client side in the DOM (Document Object Model) and is never sent to the server.

## XSS Examples
*Note: Using the default [Leaf](https://github.com/vapor/leaf) tag protects you from XSS attacks, so in these examples we will be using the `#get` tag that has no filtering or encoding. Also, alerts are used as a common and visible proof of concept for XSS/JavaScript.*

### HTML Tags
If HTML tags are not filtered or encoded, attackers can use valid tags, which will be interpreted by the browser as HTML/JavaScript. 

*Exploit:*  
`<script>alert(1337)</script>` 

*HTML:*  
```HTML
<h1>Welcome #get(variable)!</h1>
```

*Request:*  
`http://localhost:8080/xss?variable=<script>alert(1337)</script>` 

*Response:*  
```HTML
<h1>Welcome <script>alert(1337)</script>!</h1>
```


### Attributes
If “ or ‘ is not encoded, attackers can break out of attribute tags and use JavaScript event handlers to exploit XSS, even if angle brackets are filtered. 

*Exploit:*  
`" onfocus="alert(1337)" autofocus="`

*HTML:*  
```HTML
<input id="#get(variable)" type=“text”>
```

*Request:*  
`http://localhost:8080/xss?variable=foo”+onfocus=“alert(1337)”+autofocus=“`

*Response:*  
```HTML
<input id="foo" onfocus="alert(1337)" autofocus="" type=“text”>
```

### href/src/data Tags
Untrusted data placed in href, src or data tags are commonly not projected by templates or default encoding, therefore extra caution should be given when placing variables inside those attributes. 

*Exploit:*  
`javascript:alert(1337)`

*HTML:*  
```HTML
<a href="#get(variable)">My Profile</a>
```

*Request:*  
`http://localhost:8080/xss?variable=javascript:alert(1337)`

*Response:*  
```HTML
<a href="javascript:alert(1337)">My Profile</a>
```

### File Uploads
Allowing unrestricted file uploads could also result in XSS attacks.

For example, SVG image files are treated as XML from the browser and can contain XSS attacks:  

```xml
<svg version="1.1" baseProfile="full" xmlns="http://www.w3.org/2000/svg">
   <script type="text/javascript">
      alert(1337);
   </script>
</svg>
```

*Additionally, if you are processing the Exif or other metadata of image files, XSS exploits can be embedded into them and should be treated as untrusted data.*

## How can we protect against XSS attacks?
### Use Leaf
[Leaf](https://github.com/vapor/leaf) automatically HTML encodes malicious characters (*unless variables are placed directly in script tags or href/src/data attributes*) protecting you from XSS attacks. 

### Content Security Policy (CSP)
CSP neutralizes XSS attacks by defining a whitelist of trusted origins that can access a page’s resources. CSP will also restrict inline JavaScript and eval functions (*if you do not include unsafe-inline or unsafe-eval in your policy*). However, there are CSP bypasses with header misconfigurations or trusted origins that can be manipulated or controlled by the attacker. See the [brokenhandsio/VaporSecurityHeaders](https://github.com/brokenhandsio/VaporSecurityHeaders) library to help set security headers in your Vapor application.

### HttpOnly Cookies
Cookies with the HttpOnly flag set instruct the browser to not allow any client side code to access the cookie's contents.  
 
*Setting Cookie options:*  

```swift
let sessionsConfig = SessionsConfig(cookieName: "vapor-session") { value in
        return HTTPCookieValue(string: value,
                               expires: Date(timeIntervalSinceNow: 60 * 60 * 24 * 7),
                               maxAge: nil,
                               domain: nil,
                               path: "/",
                               isSecure: true,
                               isHTTPOnly: true,
                               sameSite: .lax)
    }
    services.register(sessionsConfig)
```

*Set-Cookie HTTP response header:*   

```
Set-Cookie: vapor-session=abcXYZ;
Expires=Sat, 16 Jun 2018 12:18:32 GMT;
Path=/; Secure; HttpOnly; SameSite=Lax
```

### FIEO
*Filter Input, Escape Output* - Filter and escape malicious characters and content as context requires. [vapor-community/moat](https://github.com/vapor-community/moat/) offers filtering and custom Leaf tags for your Vapor application.

### Quote Attributes
Ensure you quote your attributes with single or double quotes. For example use `<input id="#(variable)">` instead of `<input id=#(variable)>`. An attacker can break out of the attribute by adding JavaScript event handlers.

### File Uploads
If you allow various files to be uploaded, ensure when serving the files that the proper `Content-Type` and `Content-Disposition` headers are set. 

## Other attacks
### HTML Injection
HTML injection is an injection attack, similar to XSS but does not include JavaScript, where pages could be injected with unescaped HTML tags. HTML injection is not protected by CSP. 

### Blind XSS
XSS that is exploited somewhere not accessible to the attacker (for example in server logs) and includes actions or a callbacks to a server owned by the attacker. 

## Questions, comments, or suggestions?
Submit an issue, pull request, or reach out to the `#security` channel in [Vapor Discord](https://discord.gg/vapor).

## Resources:
### General
* [Same-origin policy - Web security | MDN](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)
* [Security And Your Vapor Application | Blog | Geeks @ Broken Hands](https://geeks.brokenhands.io/blog/posts/security-and-your-vapor-application/)
* [Sessions - Vapor Docs](https://docs.vapor.codes/3.0/vapor/sessions/)
* [Strategies for Securing Web Content - WWDC 2018 - Videos - Apple Developer](https://developer.apple.com/videos/play/wwdc2018/207/)
* [vapor/leaf: 🍃 An expressive, performant, and extensible templating language built for Swift.](https://github.com/vapor/leaf)

### Cross-Site Scripting (XSS)
* [DEF CON 20 - Adam "EvilPacket" Baldwin - Blind XSS](https://youtu.be/8mO00rEdCrQ)
* [File Upload XSS - Brute XSS](https://brutelogic.com.br/blog/file-upload-xss/)
* [OWASP Sweden - The image that called me](https://www.owasp.org/images/0/03/Mario_Heiderich_OWASP_Sweden_The_image_that_called_me.pdf)
* [Top 10-2017 A7-Cross-Site Scripting (XSS) - OWASP](https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_%28XSS%29)
* [XSS (Cross Site Scripting) Prevention Cheat Sheet - OWASP](https://www.owasp.org/index.php/XSS_%28Cross_Site_Scripting%29_Prevention_Cheat_Sheet)

### Content Security Policy (CSP)
* [brokenhandsio/VaporSecurityHeaders: Harden Your Security Headers For Vapor](https://github.com/brokenhandsio/VaporSecurityHeaders)
* [Content Security Policy - Google Developers](https://developers.google.com/web/fundamentals/security/csp/)
* [Content Security Policy - OWASP](https://www.owasp.org/index.php/Content_Security_Policy)
* [CSP Evaluator](https://csp-evaluator.withgoogle.com)