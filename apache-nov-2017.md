# Apache Fundamentals Notes November 2017

NEED TO INCLUDE *ALL* SLIDES in PDF


## Q&A
* Q: Which MPM is better, Event or Worker?
* A: see https://linuxtechme.wordpress.com/2014/11/04/mpm/

* Q: What is APSX?
* A: apxs - APache eXtenSion tool; a tool for building and installing extension modules
  See: https://httpd.apache.org/docs/trunk/programs/apxs.html

* Q: What is the performance gain/loss using APR?
* A: ???

* Q: What is meant by "Asynchronous" support for MPMs?
* A:

* Q: RPM vs. SRC considerations: make sure discussion is in mod 1

* Q: from Francois to All Participants: what is the support for IPv6 in Apache?
* A: from Todd Reed to All Participants: Apache does support dual stack
     TODO: find the module or configuration needed for this

* Q: Example of LAYOUT

* Q: from Francois to All Participants: with --with-expat=MPM why MPM?

* Q: from Francois: how do I enable HTTP2 support?
* A: see: https://httpd.apache.org/docs/2.4/howto/http2.html; and rebuild Apache with
```
--enable-http2          HTTP/2 protocol handling in addition to HTTP
                          protocol handling. Implemented by mod_http2. This
                          module requires a libnghttp2 installation. See
                          --with-nghttp2 on how to manage non-standard
                          locations. This module is usually linked shared and
                          requires loading.
```

* Q: Log format codes docs?
* A: http://httpd.apache.org/docs/2.4/mod/mod_log_config.html

* Q: from Christopher to All Participants: How can we see what ./configure flags a distribution used when building their verison of Apache?

* Q: from Francois to All Participants: do why it was installed as apache2 instead of httpd? is that the prefix argument?

* Q: from Christopher to All Participants: I can do httpd -V and get some info, but not all of the flags.

* Q: re: the “SetHandler” directive: what handlers are available?
* A: see:https://httpd.apache.org/docs/current/handler.html

* Q: how do I set PHP as a “handler” for *.php requests?
* A: see:http://php.net/manual/en/install.unix.apache2.php

* Q: How many sub-patterns?
* A: $1 up to $9

* Q: What is an IPv4 mapped IPv6 address?
* A: see: https://tools.ietf.org/html/rfc5156 and https://en.m.wikipedia.org/wiki/IPv6#IPv4-mapped_IPv6_addresses

* Q: What is `mod_vhost_alias`?
* A: Provides the ability to dynamically create large numbers of vhosts automatically.  See: https://httpd.apache.org/docs/2.4/mod/mod_vhost_alias.html

* Q: from Francois to All Participants: how would you "enable" an apache instance with a different conf file so that it start automatically at boot?
  systemctl enable httpd, can you say systemctl enable httpd -f /new-file.conf?

* Q: List of SSL hardware acceleration available?

* Q: from Christopher to All Participants: What is the difference between aNull and eNull?
* A: from James to All Participants: eNULL = ciphers offering no encryption, aNULL = cipher suites offering no authentication

* Q: How to you specify # instances?

* Q: What is the difference between SCGI and CGI?
* A: See: http://python.ca/scgi/protocol.txt
  * The SCGI protocol is a replacement for the Common Gateway Interface
    (CGI) protocol.  It is a standard for applications to interface with
    HTTP servers.  It is similar to FastCGI but is designed to be easier
    to implement.

* Q: What is "stickyness" in regards to `mod_proxy_balancer`?
* A: See: https://httpd.apache.org/docs/2.4/mod/mod_proxy_balancer.html#stickyness
  * When a request is proxied to some back-end, then all following requests from the same user should be proxied to the same back-end. Many load balancers implement this feature via a table that maps client IP addresses to back-ends. This approach is transparent to clients and back-ends, but suffers from some problems: unequal load distribution if clients are themselves hidden behind proxies, stickyness errors when a client uses a dynamic IP address that changes during a session and loss of stickyness, if the mapping table overflows.

* Q: from Christopher to All Participants: Does that module also rewrite CSS and JS?
* A: Need to set mod_proxy_html::ProxyHTMLExtended flag to ¨on¨

* Q: from Maroun to All Participants: RE: stickyness: depends where you are saving the sessions right?
* A:

* Q: What is suExec?
* A: see: http://httpd.apache.org/docs/2.4/suexec.html
   * provides users of the Apache HTTP Server the ability to run CGI and SSI programs under user IDs different from the user ID of the calling web server

* Q: from Francois to All Participants: could you proxy using verb?
* A:

* Q: Why would TransferLog be used over CustomLog (or vice-versa)?
* A: *TransferLog* is used to change the log file name only, not the log format.
     This is useful if you want to define a uniform log format for all log files (for several VHosts, for example) in one place using "LogFormat".
     *CustomLog* is useful to customize the log format as well, for example if you need additional information for one VHost.
  * See: https://httpd.apache.org/docs/2.4/mod/mod_log_config.html#comments_section

* Q: RE: `SetEnvIf` what are "aspects of the request"?
* A: See: https://httpd.apache.org/docs/2.4/mod/mod_setenvif.html#setenvif
| Aspect      | Notes |
|-------------|-------|
| Remote_Host | the hostname (if available) of the client making the request |
| Remote_Addr | the IP address of the client making the request |
| Server_Addr | the IP address of the server on which the request was received (only with versions later than 2.0.43) |
| Request_Method | the name of the method being used (GET, POST, et cetera) |
| Request_Protocol | the name and version of the protocol with which the requst was made (e.g., "HTTP/0.9", "HTTP/1.1", etc.) |
| Request_URI | the resource requested on the HTTP request line -- generally the portion of the URL following the scheme and host portion without the query string. See the RewriteCond directive of mod_rewrite for extra information on how to match your query string |


* Q: Do I need to enable and configure `mod_session` in order to use $_SESSION in PHP?
* A: No: PHP uses its own mechanism which bypasses the one used by `mod_session`

## ERRATA
* 52: must Linux s/be most Linux
* 52: bad char in code
* 78: Shared (Dynamic) vs Static Libraries: img needs width
* 129: missing 1st example: `Header echo ^TS`shown on next slide
* 185 URL-path is is omitted: remove duplicate "is"
* 196: last screenshot `ScriptAlias` directive needs to be swapped with the screenshot on p. 197
* 216: update to php7
* 225: "Servers that are heaving on serving" s/be "Servers that are heavy on serving"
* 249: img needs width
* 272: img needs width
* 276: img too big
* 293: need ref for more detail
* 316: usemod_cache
* 330: Mod Cluster: out of place???
* 331: Advantages of mod_cluster: out of place???
* 365: mod_cluster and Load Balancing 4: funny chars in <code> block
* 370: add description of suexec

* http://localhost:8888/#/2/7: s/ not be "MPMs server"
* http://localhost:8888/#/4/28: s/be "-h" not "- h" and "-V" not "- V" etc.
* http://localhost:8888/#/5/5: mod_cache *not* shown above"!
* http://localhost:8888/#/6/9: get rid of space after "/"
* http://localhost:8888/#/6/11: get rid of space after "/"
* http://localhost:8888/#/6/12: move lab to end of course module
* http://localhost:8888/#/4/12: move this slide to before slide on rpmbuild!
* http://localhost:8888/#/6/10: step 5: HTTPS not available yet
* http://localhost:8888/#/7/17: http://httpd.apache.org/docs/2.4/expr.html
* http://localhost:8888/#/7/42: flag s/be "-h" NOT "-H"
* http://localhost:8888/#/7/42: "regular" file or ... ????
* http://localhost:8888/#/7/44: mismatch between text and graphic
* http://localhost:8888/#/7/45: RewriteEngine on == "on" s/be bold
* http://localhost:8888/#/7/48: should make clear that this is a separate subject: i.e. enabling mod_rewrite + move this unit before discussion on config
* http://localhost:8888/#/7/60: from Francois to All Participants: locationmatch saves as <NUMBER> but reads as MATCH_NUMBER, is that normal or an error?
* http://localhost:8888/#/7/71: text and screenshot do not match
* http://localhost:8888/#/8/13: “NameVirtualHost” directive has been deprecated
* http://localhost:8080/index.html#/9/36: Link is broken: use this: https://jamielinux.com/docs/openssl-certificate-authority/appendix/root-configuration-file.html
* http://localhost:8080/index.html#/9/39: move the openSSL directory to httpd-xxx/srclib/openssl ... don't worry about the version
* http://localhost:8080/index.html#/9/39: `--with-ssl=` flag is incorrect; s/be:
```
--with-ssl=/usr/local/src/httpd-2.4.x/srclib/openssl
```
* http://localhost:8888/#/9/5: from Francois to All Participants: why is this slide here (in http vs https)?
  * maybe move to security or config section
* http://localhost:8888/#/9/18: need to update this chart + browser versions
* http://localhost:8888/#/9/22: SSL v3 date not mentioned; v2 mentioned 2x
* http://localhost:8888/#/9/27: Listen s/be on its own line
* http://localhost:8888/#/9/28: from Christopher to All Participants: That SSLHonorCipherOrder on should be on a new line
* http://localhost:8888/#/9/29: missing # on 1st line
* http://localhost:8888/#/9/29: might be better to default to a higher level of security, and then "allow" lower levels in certain non-sensitive areas of the website
* http://localhost:8888/#/9/36: correct link: https://jamielinux.com/docs/openssl-certificate-authority/appendix/root-configuration-file.html
* http://localhost:8888/#/9/42: rewrite this a bit: from Christopher to All Participants: I think you meant configure it to use the certificate and key
* http://localhost:8888/#/9/55: assumes you've created an Apache user
* http://localhost:8888/#/10/4-6: just pull linger_close() discussion out: confuses more then helps
* http://localhost:8888/#/10/7: dup slide from config section (?verify)
* http://localhost:8888/#/10/12: need to add references
* http://localhost:8888/#/9/32: no need to download openssl -- use existing version
* http://localhost:8888/#/11/14: should also mention mod_proxy_http2 + the link has a space in it!!!
* http://localhost:8888/#/12/3: remove this slide
* http://localhost:8888/#/12/5: need example of syntax of loading using APXS
* http://localhost:8888/#/13/4 + 5: syntax s/be :
```
<Location "/server-status">
    SetHandler server-status
    Require host example.com
</Location>
```
* http://localhost:8888/#/13/6: "such as" instead of "should you like"
* http://localhost:8888/#/13/8: what other tools????
* http://localhost:8888/#/14/23: Disable Directory Browser Listing: replace 2.2 syntax w/ `Require all granted`
* http://localhost:8888/#/14/25: Run Apache as Non-Privileged User: get rid of # in code block
* http://localhost:8888/#/14/26: Non-Privileged User Continued: s/be "apache:apache" (no "A")
* http://localhost:8888/#/14/28: Limiting HTTP Request Methods: here is the correct syntax for the example:
```
<LimitExcept POST GET HEAD>
  Require valid-user
</LimitExcept>
```
* http://localhost:8888/#/14/50: mod_security Core Rule Installation: Depends on your "PREFIX" setting when installing Apache.  In our case replace "/opt/apache/" with "/usr/local/apache2/"
* http://localhost:8888/#/14/54: mod_security Logging: Depends on your "PREFIX" setting when installing Apache.  In our case replace "/opt/apache/" with "/usr/local/apache2/"
* http://localhost:8888/#/14/7: MaxRequestWorkers: re: performance tuning: might have already discussed???
* http://localhost:8888/#/14/18: Protect Server Files: out of place!
* http://localhost:8888/#/14/20: Limit Access: probably meant ¨Require all denied¨ for 2nd example: if this refers to the public website, that´s ok ... but need to rewrite to make more clear that is the case
* http://localhost:8888/#/14/23: Disable Directory Browser Listing: uses 2.2 syntax: replace with Require all granted
* http://localhost:8888/#/14/26: Non-Privileged User Continued: probably should say ¨document root¨ not installation directory
* http://localhost:8888/#/14/27: Directory Permissions: from Francois to All Participants: they are mixing dir and file, either talk about the file httpd.conf or talk about the conf folder, but don't mix files and folder permission
* http://localhost:8888/#/14/33: no space in []
* http://localhost:8888/#/14/37: s/be libxml2-devel
* http://localhost:8888/#/14/39: check that syntax is correct?  also no ./configure???
* http://localhost:8888/#/14/42: s/be expat-devel
* http://localhost:8888/#/14/53: Core Rules: Sllowed s/be Allowed
* http://localhost:8888/#/14/69: mod_setenvif 2: Attributes can be one of 4 things: s/be "Attributes can be one of 3 things:"
  * Bullet starting with "Hostname, IP of client, IP of the server," s/be under bullet above
* http://localhost:8888/#/16/11: mod_cache 4: Allows concent to be cached s/be "Allows content to be cached"
* http://localhost:8888/#/14/66: redundant???
* http://localhost:8888/#/15/10: need reference as to how to find driver complied into apr-util + what is PKCS
* http://localhost:8888/#/15/14: 1st line s/be a comment
* http://localhost:8888/#/15/20: remove and/or consolidate content with earlier discussion on this subject
* http://localhost:8888/#/15/29: s/ not be "sued"
* http://localhost:8888/#/16/6: need more info on what this chart tells us
* http://localhost:8888/#/16/12: s/be stale not state
* http://localhost:8888/#/16/13: change for linux!!!

## GENERAL NOTES

* RE: HTTP2 ... suggest adding this to the section on Modules, or make it a new course section
* RE: Dynamic Shared Objects: http://httpd.apache.org/docs/2.4/dso.html
* RE: RewriteMap: http://httpd.apache.org/docs/2.4/mod/mod_rewrite.html#mapfunc
  * also: http://httpd.apache.org/docs/2.4/rewrite/rewritemap.html#txt
* RE: config section / httpd.conf: need to talk about <Location> and <Directory> directives
* RE: configuration sections: http://httpd.apache.org/docs/2.4/sections.html

* RE: mod_alias: stackoverflow.com regex summary: https://stackoverflow.com/questions/22937618/reference-what-does-this-regex-mean
* RE: mod_redirect: valid HTTP status codes:https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html
* RE: mod_rewrite: examples of rewrite conditions: https://httpd.apache.org/docs/2.0/misc/rewriteguide.html
  * NOTE: is based on Apache 2.0 and needs to be updated

* RE: Proxy* directives:
  * ProxyPass : Maps remote servers into the local server URL-space : http://httpd.apache.org/docs/2.4/mod/mod_proxy.html#proxypass
  * ProxyPassReverse: Adjusts the URL in HTTP response headers sent from a reverse proxied server : http://httpd.apache.org/docs/2.4/mod/mod_proxy.html#proxypassreverse
  * ProxyPreserveHost: Use incoming Host HTTP request header for proxy request : http://httpd.apache.org/docs/2.4/mod/mod_proxy.html#proxypreservehost
  * from Francois to All Participants: One thing that wasn't clear in the pdf, to get multiple instance running you need to change either ServerRoot or set different PidFile in each config file.

* RE: mod_rewrite: probably one of the most used features, but see if it can be made more concise, but add great examples
  * Remove sections which are straight out of the docs: add a reference to the docs

* RE: Module 9 Web Arch == focus on High Availability and get rid of config stuff

* RE: Any mention of Apache Tomcat (i.e. mod_jk), move to separate course module, optional, at the end

* RE: monitoring tools: kibana, zenoss, solarwinds, nagios, apachetop, iftop, tcpdump, apache bench

* RE: forward proxy: The client must be specially configured to use the forward proxy to access other sites.
* RE: PHP installation: these instructions are good for Centos: https://www.webhostinghero.com/centos-apache2-mariadb10-php7-setup/
  * Here is a ref to installing PHP and using a PHP-FPM worker:
  * https://blacksaildivision.com/php-install-from-source
  * https://wiki.apache.org/httpd/PHP-FPM

* RE: mod_status
```
<Location "/server-status">
    SetHandler server-status
    Require local
    Require ip 192.168.175.1
</Location>
```

* RE: Security
  * suEXEC: http://httpd.apache.org/docs/2.4/suexec.html
  * CGIWrap: https://github.com/cgiwrap/cgiwrap (last update Nov 2015!)
  * Suhosin: https://suhosin.org/stories/index.html
    * Two parts: a patch to the PHP core + a PHP extension which adds security functionality
  * `LimitExcept` is the inverse of `Limit`
    * See: http://httpd.apache.org/docs/2.4/mod/core.html#limit
    * See: http://httpd.apache.org/docs/2.4/mod/core.html#limitexcept
  * "Slow Loris" attack: Slowloris tries to keep many connections to the target web server open and hold them open as long as possible. It accomplishes this by opening connections to the target web server and sending a partial request. Periodically, it will send subsequent HTTP headers, adding to—but never completing—the request. Affected servers will keep these connections open, filling their maximum concurrent connection pool, eventually denying additional connection attempts from clients
    * See: https://en.wikipedia.org/wiki/Slowloris_(computer_security)
  * EPEL rpm repository == Extra Packages for Enterprise Linux
    * see: https://fedoraproject.org/wiki/EPEL
  * `readline` == The "readline" library will read a line from the terminal and return it
  * mod_security: https://www.modsecurity.org/
  * mod_security_crs: https://modsecurity.org/crs/
    * crs == Core Rule Set
  * ap_expr == Apache Expression Parser
    * see: https://httpd.apache.org/docs/current/expr.html
  * OS level tool `fail2ban` is when you have IP trying to login with SSH + can work with weblogs
  * mod_evasive -- info on that

* RE: mod_log_config
  * Format codes: https://httpd.apache.org/docs/2.4/mod/mod_log_config.html
  * NCSA = National Center for Supercomputing Applications
  * In the example shown:
```
LogFormat "%h %l %u %t \"%r\" %>s %b" common
CustomLog "logs/access_log" common
```
    * "common" is the format nickname

* RE: mod_setenvif
  * For more info on expressions: http://httpd.apache.org/docs/2.4/expr.html

## SSL etc.
* To find the VM version of openssl: `rpm -q openssl`
* To find where openssl is installed: `whereis openssl`
* To get a list of modules which are loaded: `apachectl -M`
* If you get an error `configure: WARNING: OpenSSL version is too old`
  * install the openssl development package: `yum install openssl-devel`
* Tutorial on creating certificates for mod_ssl on Centos 7: https://www.digitalocean.com/community/tutorials/how-to-create-an-ssl-certificate-on-apache-for-centos-7
* Excellent tutorial on installing httpd with ssl: https://www.rodneybeede.com/Building_Apache_2_4_for_Linux_with_mod_ssl.html
  * Oriented towards Ubuntu/Debian Linux so you'll need to adjust for Redhat/Fedora/CentOS
    * i.e. instead of `sudo apt-get install xxx` you would change to root (`su`) and then: `yum install xxx`
* This configure command seems to work:
```
./configure --with-included-apr --with-expat-lib --enable-ssl=shared --enable-mods-shared=all
```
  * TLS: https://en.m.wikipedia.org/wiki/Transport_Layer_Security
  * Good security overview: https://en.m.wikipedia.org/wiki/Transport_Layer_Security#Security
  * HEARTBLEED: Status of different versions:
    * OpenSSL 1.0.1 through 1.0.1f (inclusive) are vulnerable
    * OpenSSL 1.0.1g is NOT vulnerable
    * OpenSSL 1.0.0 branch is NOT vulnerable
    * OpenSSL 0.9.8 branch is NOT vulnerable
    * Bug was introduced to OpenSSL in December 2011 and has been out in the wild since OpenSSL release 1.0.1 on 14th of March 2012. OpenSSL 1.0.1g released on 7th of April 2014 fixes the bug.
  * POODLE:
    * The SSL 3.0 vulnerability stems from the way blocks of data are encrypted under a specific type of encryption algorithm within the SSL protocol. The POODLE attack takes advantage of the protocol version negotiation feature built into SSL/TLS to force the use of SSL 3.0 and then leverages this new vulnerability to decrypt select content within the SSL session. The decryption is done byte by byte and will generate a large number of connections between the client and server.
    * While SSL 3.0 is an old encryption standard and has generally been replaced by TLS, most SSL/TLS implementations remain backwards compatible with SSL 3.0 to interoperate with legacy systems in the interest of a smooth user experience. Even if a client and server both support a version of TLS the SSL/TLS protocol suite allows for protocol version negotiation (being referred to as the “downgrade dance” in other reporting). The POODLE attack leverages the fact that when a secure connection attempt fails, servers will fall back to older protocols such as SSL 3.0. An attacker who can trigger a connection failure can then force the use of SSL 3.0 and attempt the new attack. [1]
    * Two other conditions must be met to successfully execute the POODLE attack: 1) the attacker must be able to control portions of the client side of the SSL connection (varying the length of the input) and 2) the attacker must have visibility of the resulting ciphertext. The most common way to achieve these conditions would be to act as Man-in-the-Middle (MITM), requiring a whole separate form of attack to establish that level of access.
These conditions make successful exploitation somewhat difficult. Environments that are already at above-average risk for MITM attacks (such as public WiFi) remove some of those challenges.
    * Solution
      * There is currently no fix for the vulnerability SSL 3.0 itself, as the issue is fundamental to the protocol; however, disabling SSL 3.0 support in system/application configurations is the most viable solution currently available.
      * Some of the same researchers that discovered the vulnerability also developed a fix for one of the prerequisite conditions; TLS_FALLBACK_SCSV is a protocol extension that prevents MITM attackers from being able to force a protocol downgrade.
    * OpenSSL has added support for TLS_FALLBACK_SCSV to their latest versions and recommend the following upgrades: [5]
      * OpenSSL 1.0.1 users should upgrade to 1.0.1j.
      * OpenSSL 1.0.0 users should upgrade to 1.0.0o.
      * OpenSSL 0.9.8 users should upgrade to 0.9.8zc.
      * Both clients and servers need to support TLS_FALLBACK_SCSV to prevent downgrade attacks.
  * DROWN attack: https://en.m.wikipedia.org/wiki/DROWN_attack
  * Recent TLS https://en.m.wikipedia.org/wiki/DROWN_attack: https://cve.mitre.org/cgi-bin/cvekey.cgi?keyword=Tls
  * DH === Diffie-Hellman
  * HSTS: https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet
  * Elliptic Curve Cryptography: https://en.m.wikipedia.org/wiki/Elliptic-curve_cryptography
    * Before a client and server can begin to exchange information protected by TLS, they must securely exchange or agree upon an encryption key and a cipher to use when encrypting data (see Cipher). Among the methods used for key exchange/agreement are: public and private keys generated with RSA (denoted TLS_RSA in the TLS handshake protocol), Diffie-Hellman (TLS_DH), ephemeral Diffie-Hellman (TLS_DHE), Elliptic Curve Diffie-Hellman (TLS_ECDH), ephemeral Elliptic Curve Diffie-Hellman (TLS_ECDHE), anonymous Diffie-Hellman (TLS_DH_anon),[1] pre-shared key (TLS_PSK)[29] and Secure Remote Password (TLS_SRP).[30]
    * The TLS_DH_anon and TLS_ECDH_anon key agreement methods do not authenticate the server or the user and hence are rarely used because those are vulnerable to Man-in-the-middle attack. Only TLS_DHE and TLS_ECDHE provide forward secrecy.

* RE: Generating Certificates: see this tutorial: https://jamielinux.com/docs/openssl-certificate-authority/index.html
* RE: Firewall settings:
```
firewall-cmd --zone=public --add-port=443/tcp --permanent
firewall-cmd --reload
iptables -L
```

### Procedure without using Intermediate Certs
#### Set up directory structure
* There already exists a directory structure: `/etc/pki/CA`
```
cd /etc/pki/CA
mkdir csr
touch index.txt
echo 1000 > serial
```
* Create `openssl.cnf` using example from https://jamielinux.com/docs/openssl-certificate-authority/_downloads/root-config.txt
#### Root Key and Certificate
* Create root key:
```
openssl genrsa -aes256 -out private/ca.key.pem 4096
chmod 400 private/ca.key.pem
```
  * Use `password` for the password
* Create the root certificate:
```
openssl req -config openssl.cnf -key private/ca.key.pem -new -x509 -days 7300 -sha256 -extensions v3_ca -out certs/ca.cert.pem
chmod 444 certs/ca.cert.pem
```
  * Answer the prompts
  * use `password` for the password
* Verify the root certificate: `openssl x509 -noout -text -in certs/ca.cert.pem`

#### Sign server and client certificates
* Create server key:
```
openssl genrsa -aes256 -out private/test.local.key.pem 2048
chmod 400 private/test.local.key.pem
```
  * Use `password` for the password
* Create signing request:
```
openssl req -config openssl.cnf -key private/test.local.key.pem -new -sha256 -out csr/test.local.csr.pem
```
  * Answer the prompts
  * Use `password` for the password
* Create certificate:
```
openssl ca -config openssl.cnf -extensions server_cert -days 375 -notext -md sha256 -in csr/test.local.csr.pem -out certs/test.local.cert.pem
chmod 444 certs/test.local.cert.pem
```
  * Use `password` for the password
* Verify the certificate:
```
openssl x509 -noout -text -in certs/test.local.cert.pem
```
#### Deploy the certificate
* Modify `/usr/local/apache2/conf/httpd.conf` as follows:
  * Remove the comment in front of this line (towards the end): `Include conf/extra/httpd-ssl.conf`
  * Enable these modules:
    * mod_socache_dbm
    * mod_log_config
    *

  * Make sure `mod_ssl` is *not* loaded in this file!
* Modify `/usr/local/apache2/conf/extra/httpd-ssl.conf` as follows:
  * Add this line towards the top: `LoadModule ssl_module modules/mod_ssl.so`
  * Change the VirtualHost block starting tag to this: `&lt;VirtualHost *:443&gt;`
  * Modify the server name as follows: `ServerName test.local:443`
  * Edit the location of the SSL certificate file: `SSLCertificateFile "/etc/pki/CA/certs/test.local.cert.pem"`
  * Edit the location of the SSL key file: `SSLCertificateKeyFile "/etc/pki/CA/private/test.local.key.pem"`
* Make sure the firewall allows HTTPS on port 443:
```
iptables -A INPUT -i lo -j ACCEPT
iptables -A INPUT -p tcp -m tcp --dport 80 -j ACCEPT
iptables -A INPUT -p tcp -m tcp --dport 443 -j ACCEPT
iptables -L -n
iptables-save | sudo tee /etc/sysconfig/iptables
```
  * See: https://www.digitalocean.com/community/tutorials/how-to-set-up-a-basic-iptables-firewall-on-centos-6
  * See: https://crm.vpscheap.net/knowledgebase.php?action=displayarticle&id=29 (common iptables rules)
* Restart the server and fix any errors which might show up: `apachectl restart`
* Test using Lynx: `lynx https://test.local`
* Test from your browser outside the VM

### Setting up PHP using FastCGI
* See: https://github.com/dbierer/php-class-notes/blob/master/httpd-with-http2-and-fcgi.md
* Install PHP using the `--enable-fpm` configure switch
    * `php-fpm` is now included with any core installation of PHP
* Recompile Apache using these flags (in addition to any other flags you've been using): `--enable-proxy --enable-proxy-fcgi`
* Enable these modules:
    * mod_proxy
    * mod_proxy_fcgi
* Sample vhost config file (many thanks to Francois!):
```
<VirtualHost *:80>
    ServerAdmin webmaster@example.com
    DocumentRoot "/var/www"
    ServerName test.local
    ErrorLog "logs/test.local-error_log"
    CustomLog "logs/test.local-access_log" common
</VirtualHost>

<VirtualHost *:80>
    ServerAdmin webmaster@test.local
    DocumentRoot "/var/www"
    ServerName php.test.local
    ServerAlias php.test.local
    ErrorLog "logs/php.test.local-error_log"
    CustomLog "logs/php.test.local-access_log" common
    <Directory "/var/www">
        Options Indexes FollowSymLinks
        DirectoryIndex index.php
        AllowOverride All
        Require all granted
    </Directory>
    ProxyPassMatch ^/(.*\.php(/.*)?)$ fcgi://localhost:9000/var/www/$1
</VirtualHost>

<VirtualHost *:443>
    ServerAdmin webmaster@test.local
    DocumentRoot "/var/www"
    ServerName php.test.local
    ServerAlias php.test.local
    ErrorLog "logs/php.test.local-error_log"
    CustomLog "logs/php.test.local-access_log" common
    <Directory "/var/www">
        Options Indexes FollowSymLinks
        DirectoryIndex index.php
        AllowOverride All
        Require all granted
    </Directory>
    ProxyPassMatch ^/(.*\.php(/.*)?)$ fcgi://localhost:9000/var/www/$1
    SSLEngine on
    SSLCertificateFile "/etc/pki/CA/certs/test.local.cert.pem"
    SSLCertificateKeyFile "/etc/pki/CA/private/test.local.key.pem"
    <FilesMatch "\.(cgi|shtml|phtml|php)$">
        SSLOptions +StdEnvVars
    </FilesMatch>
</VirtualHost>
```

## FEEDBACK
* from Francois to All Participants: the steps of this entire modules is weird, I had to go back and forth a few times
* from self: http://localhost:8888/#/7/4: more practical examples
* from self: http://localhost:8888/#/7/10: more practical examples
* from self: http://localhost:8888/#/7/17: more practical examples
* from self: http://localhost:8888/#/7/36: more practical examples for this entire section
* from Maroun to All Participants: I would like to see example which covers the whole flow, how you set header and how it affects the request in browser and how the response looks like
* from self: practical use cases encompassing the whole thing
* from self: Headers chapter: convert screenshots of code examples to text blocks which can be copied + get the order straight
* from Francois to All Participants: course feedback -- personally, I think you should talk about mod_alias first and then go with "if you need more, here is what you can do with mod_rewrite" because at this point, I'm not sure what mod_rewrite can do that alias can't (except rewrite condition I guess).
* from self: Headers / Rewrite Lab
  * Create ScriptAlias for server side includes
  * Create html with ssi date
  * Don’t do PHP
* from Francois to All Participants: I know you did not write these examples, but you know we are not supposed to use port 81-83? anything under 1024 (if I remember correctly) are reserved for specific apps
* from Francois to All Participants: have cert and vhost first and multi instance and proxy after to me cert and vhost is something we do all the time so I think it should be first
* from self: combine multi-instance w/ proxy course chapters
* from self: add lab using separate config for each instance
* from Christopher to All Participants: I would offer everyone one word of warning... apachectl configtest doesn't catch all SSL related errors that would stop your server from starting up You can have properly stuctured configuration but if your cert and key are messed up, it won't point it out
* from Francois to All Participants: one think I trully did not get is the "Copy and Paste the user-cli.crt output" and "Paste Into user-cli.crt on the Client" what is the client? is it if you have multiple servers?
* from Francois to All Participants: I just redid the entire lab this morning and I still get "SSL error:unable to get local issuer certificate"
* from Maroun to All Participants: the private use to be pem and the public key crt
* from Francois to All Participants: ProxyPassMatch ^/(.*\.php(/.*)?)$ fcgi://127.0.0.1:9000/var/www/$1
* from Todd to All Participants: Setting up a PHP server with RP is easier to digest.  Java/Tomcat just adds complication and causes things to get fuzzy
* from Todd to All Participants: Not sure if I'm ahead, but I'm really suprised not to see anything on chroot/jail of Apache.  I know this is more of a Linux feature.  You might also consider adding an exposure slide to docker/containers, but that is a whole different subject docker/containers would be a whole different class (we did a section docker in the PHP III)!
* from Francois + self: consider changing references to /opt for consistency
* from self: simple tool to simulate web attack to see mod_security in action??? maybe a simple PHP script for example. or possibly `nmap`
* from the group: consensus: move access logging to the beginning + consolidate with logging chapter
* from self: section on Security: drop "Best Practices"
* from group: for mod_session_dbd pre-install mysql w/ a users table already in place
* from Francois: scan for any Windoze examples and modify for linux
* from Francois to All Participants: feedback : no need to go in that level of details about encryption (in my opinion) all I need to know is how to configure my Apache with a certificate, not all the history and type of encryption, and that include the abbreviation and the list of encryption support in old browser (like IE6), I don't think it is that relevant
* from Francois to All Participants: Examples are not details, ex: "openssl req -newkey rsa:2048 -nodes -keyform PEM -keyout test-local.key -x509 -days 3650 -outform PEM -out test-local.crt" I would like to know what each paramaters does. If I'm just going to search for this information for myself, there is no need for me to pay for a course...
* from Francois to All Participants: A lot of wasting time, for example the million videos. What was the point of this example/discussion. I found it irrelevant for this course. I want to learn to configure Apache not to use cloud services


## EXAMPLES
* Created user `apache` using this command: `useradd apache`
* Modified `/usr/local/apache2/conf/httpd.conf` as follows:
  * Set user and group to `apache`
  * Added at end: `Include /usr/local/apache2/conf/extra/test.conf`
* Created the following:
  * `/usr/local/apache2/htdocs/test/index.html`
  * `/usr/local/apache2/htdocs/notest/index.html`
  * `/home/apache2/test/index.html`
* Create file: `/usr/local/apache2/conf/extra/test.conf` as follows:
```
<Location "/test">
  Header merge Cache-Control no-cache
  Header merge Cache-Control no-store
  Header unset ETag
  SetEnvIf TestReqHeader ABC HAVE_TestReqHeader
  Header set TestRespHeader "%D %t XYZ" env=HAVE_TestReqHeader
  Header set TestXXX "Hey ... how is it going Joe?"
</Location>
Alias "/home" "/home/apache/test"
<Directory "/home/apache/test">
  Require all granted
</Directory>
<Location "/unlikely">
  Redirect temp "https://unlikelysource.com/"
</Location>
```
* Example from in-class practical 10-Nov-2017:
  * Added this to `/usr/local/apache2/conf/httpd.conf`:
```
Include /usr/local/apache2/conf/extra/class.conf
```
  * Here is the contents of `class.conf`:
```
# allows use of .htaccess in the class directory
<Directory "/usr/local/apache2/htdocs/class">
  AllowOverride all
</Directory>
# any URL which starts with "/whatever" will be mapped to "/var/www/whatever"
Alias "/whatever" "/var/www/whatever"
# the web server is given access to this directory, which is outside the document root
<Directory "/var/www/whatever">
  Require all granted
</Directory>
# the URL "/something/zend" is captured by this Location section
<Location "/something/zend">
  RewriteEngine On
  # check to see if "/something/zend" is an actual directory on the filesystem
  RewriteCond %{REQUEST_FILENAME} -d
  # if so, just load [L] any content
  RewriteRule .* - [L]
  # otherwise, redirect to the zend website [R] == redirect
  RewriteRule .* http://zend.com/ [R]
</Location>
```



## HTTP2
* from Francois to All Participants: this is basically what I recreated to see for myself http://www.http2demo.io/
* configure command as per Francois:
```
./configure --enable-modules="php ssl rewrite deflate security" --with-included-apr --enable-http2 --enable-so --enable-proxy --enable-proxy-fcgi
```
