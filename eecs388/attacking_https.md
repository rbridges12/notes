# Breaking HTTPS

### SSLStrip attack
When you type in a url with no prefix, the browser will usually try to connect with http instead of https, so an attacker can set up a malicious site with the same url but over http, and can then modify the users data and forward it to the real https site.

### Mixed Content
Even if you're on an HTTPS site, it can load resources like images over HTTP, which can be exploited. Browsers currently block HTTP scripts but still allow images. Fix is to only use HTTPS resources.

### Unencrypted Cookies
- Cookies sent over HTTPS will be sent unencrypted if the browser later requests an HTTP url from the domain
- Fixed by setting the secure attribute on cookies so they are only allowed to be send over HTTPS

### Plaintext SNI Header
-Server Name Indication (SNI) header is sent in plaintext bc multiple sites can be hosted on the same IP, so server name needs to be sent unencrypted to establish the correct connection
- Fixed by using TOR or VPN, or new encrypted SNI standard

### Fooling CA Validation
- If an attacker can convince a CA they control a domain, the CA will give them a cert and the attacker can use it to fool browsers
- Can be helped by limiting which CAs can issue certs on your domain by creating a CAA record
- Also helped by preventing users of a domain from creating emails like `administrator.umich.edu` because CAs often use these emails to send certs

### Attacks on CAs
- CAs get hacked sometimes
- Mitigated by CAs revoking certs and adding revoked CA to their Certificate Revocation List (CRL) which they publish.
- Also helped by Certificate Transparency policy

### Certificate Transparency
- Browsers require CAs to put every cert they publish on a public certificate transparency log
- Servers can monitor these logs to make detect any CAs not doing this, and then not use their certs

### Null Prefix Attack
- CA uses Pascal type strings while browser uses C style strings
- an attacker can make a domain name like `gmail.com\0.badguy.com`
- they can prove ownership and get a cert for `badguy.com`
- a browser written in C will think the cert is for `gmail.com`
- You probably shouldn't use memory unsafe languages like C or C++ for TLS libraries

### TLS Compression Attack
- if parts of your request data match the cookie sent with the request, then the request can be shortened through compression, so the encrypted message will be shorter
- an attacker can iteratively try different request data and detect when they matched a character in the cookie by monitoring the length of the encrypted message
- eventually the attackers request data will be the same as the cookie
- TLS 1.3 fixes this by not allowing any compression