# VaporSecurity
A community curated list of application security resources for [Vapor](https://vapor.codes) developers. 


#### Contributions
Contribute by submitting a pull request with changes or post resources in the `#security` channel in [Vapor Discord](https://discord.gg/BnXmVGA). 

## Articles
* [Cross-Site Scripting (XSS)](https://github.com/cakinney/VaporSecurity/blob/master/articles/001-VaporSecurity-XSS.md)
* [Cross-Site Request Forgery](https://github.com/cakinney/VaporSecurity/blob/master/articles/002-VaporSecurity-CSRF.md)

# Resources

- [Vapor Security](#vapor-security)
  - [Vapor](#vapor)
	  - [Articles](#articles)
		  - [General](#general)
		  - [Authentication](#authentication)
	  - [Libraries](#libraries)
	- [Application Security](#application-security)
    - [General](#general)
    - [Awesome Lists](#awesome-lists)
    - [Books](#books)
    - [Newsletters](#newsletters)
    - [Testing](#testing)
    - [Podcasts](#podcasts)
    - [Tools](#tools)
    - [SSL](#ssl)
    - [Misc](#misc)

## Vapor
### Articles
#### General
- [Security And Your Vapor Application | Blog | Geeks @ Broken Hands](https://geeks.brokenhands.io/blog/posts/security-and-your-vapor-application)
#### Authentication
- [Tutorial: How to build Basic Auth with Session – Martin Lasek – Medium](https://medium.com/@martinlasek/tutorial-how-to-build-basic-auth-with-session-5de3fa9df3b8)
- [Basic Authentication with Vapor 3 – Rocket Fuel – Medium](https://medium.com/rocket-fuel/basic-authentication-with-vapor-3-c074376256c3)
- [Token Authentication in Vapor 3 - Vapor Forums](https://www.vaporforums.io/thread/44)
- [Username/Password Authentication in Vapor 3 - Vapor Forums](https://www.vaporforums.io/thread/43)
- [HTTP Basic Authorization in Vapor 3 - Vapor Forums](https://www.vaporforums.io/thread/41)

### Libraries
- [brokenhandsio/vapor-oauth-fluent](https://github.com/brokenhandsio/vapor-oauth-fluent) - Fluent Implementations For Vapor OAuth
- [brokenhandsio/vapor-oauth](https://github.com/brokenhandsio/vapor-oauth) - OAuth2 Provider Library for Vapor
- [brokenhandsio/VaporSecurityHeaders](http://github.com/brokenhandsio/VaporSecurityHeaders) - Harden Your Security Headers For Vapor
- [gotranseo/vapor-recaptcha](https://github.com/gotranseo/vapor-recaptcha) - A Vapor 3 library for validating Google reCAPTCHA submissions
- [vapor-community/bcrypt](https://github.com/vapor-community/bcrypt) - Swift implementation of the BCrypt password hashing function
- [vapor-community/CSRF](https://github.com/vapor-community/CSRF) - A package to add protection to Vapor against CSRF attacks
- [vapor-community/Imperial](https://github.com/vapor-community/Imperial) - Federated Authentication with OAuth providers
- [vapor-community/moat](https://github.com/vapor-community/moat) - A line of defense for your Vapor application including XSS attack filtering + extras.
- [vapor-community/tls](https://github.com/vapor-community/tls) - Non-blocking, event-driven TLS built on OpenSSL & macOS security
- [vapor/auth](https://github.com/vapor/auth) - Authentication and Authorization layer for Fluent

## Application Security
### General
- [Open Web Application Security Project (OWASP)](https://www.owasp.org/) - An open source community project that provides impartial, practical information about application security
- [OWASP Top Ten Project](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project) -  The top 10 most critical web application security risks

### Awesome Lists 
- [paragonie/awesome-appsec](https://github.com/paragonie/awesome-appsec) - A curated list of resources for learning about application security
- [qazbnm456/awesome-web-security](https://github.com/qazbnm456/awesome-web-security/) - A curated list of Web Security materials and resources
- [andriisoldatenko/awesome-security-testing](https://github.com/andriisoldatenko/awesome-security-testing) - A collection of awesome Security testing resources
- [PaulSec/awesome-sec-talks](https://github.com/PaulSec/awesome-sec-talks) - A curated list of awesome Security talks

### Books
- [The Web Application Hacker’s Handbook](http://mdsec.net/wahh/) - Comprehensive guide to security testing web applications
- [Web Hacking 101 by Peter Yaworski](https://leanpub.com/web-hacking-101) - Explains common web vulnerabilities and exploit techniques by analyzing publicly disclosed vulnerabilities. 
- [Breaking into Information by Andy Gill](https://leanpub.com/ltr101-breaking-into-infosec) - Beginner guide to web application penetration testing

### Newsletters
- [Simpsonpt/AppSecEzine: AppSec Ezine](https://github.com/Simpsonpt/AppSecEzine/) - Overview of the latest topics in application security
- [Zero Daily - HackerOne](https://www.hackerone.com/zerodaily) - Daily newsletter focusing on focus on application security and bug bounty topics

### Testing
- [danielmiessler/SecLists](https://github.com/danielmiessler/SecLists) - A collection of multiple types of lists used during security assessments
- [Using Burp to Test for the OWASP Top Ten](https://support.portswigger.net/customer/portal/articles/1969845-using-burp-to-test-for-the-owasp-top-ten) - A tutorial on how to use Burp Suite to find the vulnerabilities listed in the OWASP Top 10
- [OWASP Testing Guide v4](https://www.owasp.org/index.php/OWASP_Testing_Guide_v4_Table_of_Contents) - A guide on how to perform Web Application Penetration Testing

### Podcasts
- [Application Security Weekly - Paul’s Security Weekly](https://wiki.securityweekly.com/Application_Security_Weekly_Show_Notes) - Podcast that explains development to security professionals
- [Application Security PodCast](https://www.appsecpodcast.org/) - Podcast that explain the fundamentals of application security 

### Tools
- [Burp Suite - PortSwigger](https://portswigger.net) - Intercepting proxy and suite of web application security tools
* [OWASP Zed Attack Proxy (ZAP)](https://www.owasp.org/index.php/ZAP) - Free and open source web application testing tool

### SSL
* [Let’s Encrypt](https://letsencrypt.org/) - A free, automated, and open Certificate Authority.

### Misc
* [security.txt](https://securitytxt.org/) - A proposed standard which allows websites to define security policies