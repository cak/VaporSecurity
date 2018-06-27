# CSRF

## What is Cross-Site Request Forgery (CSRF / XSRF)?
Cross-site Request Forgery or CSRF (pronounced “Sea-Surf”) is an exploit that allows an attacker to trick a user’s browser into issuing requests on behalf of the user. The attacker is unable to view the response from the web server and instead leverages that cookies (with matching domain and path) are automatically sent by the browser on cross-site requests. Therefore, the user’s actions are authenticated and considered valid by the web server. CSRF attacks are frequently exploited via social engineering attacks which tricks users to click malicious links or visit malicious websites.

## CSRF Examples
The web application has a function that will change the e-mail address of an authenticated user. 

##### GET Request
The web application allows the change of a user's e-mail address via GET request: `http://localhost:8080/users?email=user@example.com` 

An attacker could provide a malicious link `<a href=“http://localhost:8080/users?email=attacker@example.com` or hide the request in an image tag, which requires no user interaction `<img src=“http://localhost:8080/users?email=attacker@example.com>`.

##### POST Request
The web application allows the change of a user's e=mail address via POST request.

The attacker would host the file and trick a user to visiting the page where the form would be automatically submitted. 

*Form Data:*  

```html
<form method='POST' action='http://localhost:8080/users' id="CSRF-email" style="display:none">
	 <input type='hidden' name='email' value='attacker@example.com'>
	<input type='submit' value='submit'>
</form>
<script>document.getElementById("CSRF-email").submit()</script>
```

*JSON:*  

```html
<script>
	fetch('http://localhost:8080/users', {method: 'POST', credentials: 'include', headers: {'Content-Type': 'text/plain'}, body: '{"email":"attacker@example.com"}'});
</script>
<form action="#">
</form>
```

## CSRF Prevention

### HTTP Verbs/Methods
Use the POST method instead of the GET method for any state changing requests. 

### CSRF Tokens
Anti-CSRF tokens are secret tokens embedded into form posts or included in request headers. These tokens are verified by the server that the token is valid for the user’s session and any requests requiring CSRF protections that are submitted without a valid CSRF token are refused. An attacker has no way of knowing the token’s secret value and is unable to craft an successful CSRF attack.   

##### Token Example
`<input type=“hidden” name=“csrf-token” value=“abcXYZ”>`

For more information please see the [vapor-community/CSRF](https://github.com/vapor-community/CSRF) library and the Ray Wnderlich [Server Side Swift with Vapor](https://store.raywenderlich.com/products/server-side-swift-with-vapor) book (*Chapter 20: Web Authentication, Cookies and Sessions*).

### Same-Site Cookies
The same-site cookie flag directs the browser not to include cookies on certain cross-site requests. There are two values that can be set for the same-site attribute, `lax` or `strict`. The lax value allows the cookie to be sent via certain cross-site GET requests, but disallows the cookie on all POST requests. For example cookies are still sent on links `<a href=“x”>`, prerendering `<link rel=“prerender” href=“x”` and forms sent by GET requests `<form-method=“get”...`, but cookies will not be sent via POST requests `<form-method=“post”...`, images `<img src=“x”>`or iframes `<iframe src=“x”>`.  The strict value prevents the cookie from being sent cross-site in any context. Strict offers greater security but may impede functionality. This approach makes authenticated CSRF attacks impossible with the `strict` flag and only possible via state changing GET requests with the `lax` flag. 

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

### XSS Protections
CSRF protections can be bypassed by Cross-Site Scripting (XSS) injections. Please see the VaporSecurity article on [XSS](https://github.com/cakinney/VaporSecurity/blob/master/articles/001-VaporSecurity-XSS.md).

### Origin / Referer header
Determine the origin of the request by examining `Origin` and/or `Referer` headers and blocking state changing requests that do not come from the intended location. The benefit of using origin and referer headers is that sessions are not required and they cannot be controlled by an attacker. However origin and/or referer headers may not be present in all requests. If origin or referrer headers are omitted, you can choose to fail open to balance security and functionality, or fail closed if you require a higher level of security and/or no other CSRF mitigations are implemented in your application. 

*Origin/Referer Header check*  

```swift
let originHeader = req.http.headers.firstValue(name: .origin) ?? ""
let refererHeader = req.http.headers.firstValue(name: .referer) ?? ""
if originHeader != "http://localhost:8080" && originHeader != "" {
    throw Abort(.badRequest)
}
if refererHeader != "http://localhost:8080/users/profile" && refererHeader != "" {
    throw Abort(.badRequest)
}
```

Please see the [vapor-community/moat](https://github.com/vapor-community/moat) library for an origin and referer header checking middleware. 

### Re-Authentication/CAPTCHAs
You can protect sensitive actions by requiring the user to re-authenicate on each request, for example requiring the old password to be submitted with a password change function. You can also use a CAPTCHA as a secondary CSRF protection measure, but it should not be your only defense for CSRF attacks. Although CAPTCHAs can determine requests originate from a human, there are CAPTCHA related CSRF bypasses.  Please see the [gotranseo/vapor-recaptcha](https://github.com/gotranseo/vapor-recaptcha) library for validating reCAPTCHA submissions. 

## Resources
* [Cross-Site Request Forgery (CSRF) Prevention Cheat Sheet - OWASP](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet)
* [Cross-site request forgery - Wikipedia](https://en.wikipedia.org/wiki/Cross-site_request_forgery)
* [Same-site Cookies](https://tools.ietf.org/html/draft-west-first-party-cookies-07)
* [gotranseo/vapor-recaptcha: A Vapor 3 library for validating Google reCAPTCHA submissions](https://github.com/gotranseo/vapor-recaptcha)
* [vapor-community/CSRF: A package to add protection to Vapor against CSRF attacks.](https://github.com/vapor-community/CSRF)
* [JSON CSRF - Geekboy](https://www.geekboy.ninja/blog/tag/json-csrf/)
* [Open Security Research: JSON CSRF with Parameter Padding](http://blog.opensecurityresearch.com/2012/02/json-csrf-with-parameter-padding.html)
* [Preventing CSRF with the same-site cookie attribute](https://www.sjoerdlangkemper.nl/2016/04/14/preventing-csrf-with-samesite-cookie-attribute/)
* [SameSite - OWASP](https://www.owasp.org/index.php/SameSite)
* [Security And Your Vapor Application - Broken Hands](https://geeks.brokenhands.io/blog/posts/security-and-your-vapor-application/)


