## Public DNS Resolvers
View performance statistics for public DNS resolvers at [DNSPerf](https://www.dnsperf.com/#!dns-resolvers). The statistics reported at [DNSPerf](https://www.dnsperf.com/#!dns-resolvers) tend to be very similar to the results I get using the [namebench](https://code.google.com/archive/p/namebench/) tool.  

#### CloudFlare
```
1.1.1.1
1.0.0.1
2606:4700:4700::1111
2606:4700:4700::1001
```
#### Dyn
```
216.146.35.35
216.146.36.36
```
#### Google
```
8.8.8.8
8.8.4.4
2001:4860:4860::8888
2001:4860:4860::8844
```
#### OpenDNS
```
208.67.222.222
208.67.220.220
2620:0:ccc::2
2620:0:ccd::2
```
#### OpenNIC Project
Go to https://www.opennic.org to see which of their DNS resolvers are closest to you. OpenNIC servers periodically go down and occasionally just disappear. The servers that tend to work best for me (in North America) are:
```
192.52.166.110
158.69.239.167
```
#### Quad9
```
9.9.9.9
149.112.112.112
2620:fe::fe
```
#### Yandex Basic
```
77.88.8.8
77.88.8.1
2a02:6b8::feed:0ff
2a02:6b8:0:1::feed:0ff
```
#### Yandex Safe
```
77.88.8.88
77.88.8.2
2a02:6b8::feed:bad
2a02:6b8:0:1::feed:bad
```

## Free DNS Hosting
[1984](https://1984hosting.com/): Unlimited domains; cannot find limits on records per zone or lookups. Supports DNSSEC

[Cloudflare](https://www.cloudflare.com/): Unlimited lookups; 3500 records per zone; cannot find limit on number of zones. Supports DNSSEC

[ClouDNS](https://www.cloudns.net/): 3 free zones; unlimited records and lookups; no control over TTL on free plans.

[Hurricane Electric](http://dns.he.net/): 50 free zones; cannot find limits on records per zone or lookups.

[Selectel](https://selectel.ru/en/): Unlimited domains; cannot find limits on records per zone or lookups. 

[Veesp](https://veesp.com/): DNS hosting is available free if you are paying for other services there.

[Vultr](https://www.vultr.com/docs/introduction-to-vultr-dns): DNS hosting is available free if you are paying for other services there. They allow vanity name servers pointed at their own name servers.

[Yandex](https://domain.yandex.com/): 50 free zones; cannot find limits on records per zone or lookups. No CAA records. (Although this service is aimed at people using Yandex to host their email, Yandex support explicitly states that you are free to host DNS without also hosting your email.) 

#### DNSSEC
Enabling DNSSEC:  
Generate DNSSEC keys and DS records at your DNS host.  
Add the DS records at your domain registrar.  

Disabling DNSSEC:  
Remove the DS records at your domain registrar.  
Wait 24 hours for most domains, but 48 hours for domains registered through EU.org. Do a "dig ds" check for the DS TTL on whatever domain from which you are removing DNSSEC.  
Remove or disable DNSSEC at your DNS host.  

Examples of DS records:  
```
debian.org. 10762 IN DS 6487 8 2 A9528F2409C5F6A95AE6E0F8A6C5A223AC4EFD54B45884CB855F044E 82F7F4C6  
yandex.com. 9595 IN DS 31456 5 1 593F529E8942948DE9D6646AC5F9E2208F49D606
````

#### DMARC Records
```
_dmarc.domain.tld. IN TXT "v=DMARC1; p=none; rua=mailto:admin@domain.tld"
```
DMARC action to take if either SPF or DKIM fail:
```
p=none; - take no action
p=quarantine; - mark message as spam
p=reject; - reject message outright
```
To send DMARC reports to an address outside the email sending domain, create a TXT record in the DNS of the receiving domain:
```
<email-sending-domain.tld>._report._dmarc.<receiving-domain.tld>.  IN TXT "v=DMARC1"
```
Of course, you will still need to create the basic DMARC record in the DNS of the email sending domain:
```
_dmarc.<email-sending-domain.tld>. IN TXT "v=DMARC1; p=none; rua=mailto:admin@report-receiving-domain.tld"
```
#### MX Records

Fastmail
```
IN MX 10 in1-smtp.messagingengine.com.
IN MX 20 in2-smtp.messagingengine.com.
```
Gandi
```
IN MX 10 spool.mail.gandi.net.
IN MX 50 fb.mail.gandi.net.
```
Google
```
IN MX 1 aspmx.l.google.com.
IN MX 5 alt1.aspmx.l.google.com.
IN MX 5 alt2.aspmx.l.google.com.
IN MX 10 alt3.aspmx.l.google.com.
IN MX 10 alt4.aspmx.l.google.com.
```
Migadu
```
IN MX 10 aspmx1.migadu.com.
IN MX 20 aspmx2.migadu.com.
```
PolarisMail
```
IN MX 5 mx.emailarray.com.
IN MX 10 mx2.emailarray.com.
```
Runbox
```
IN MX 10 mx.runbox.com.
```
Yandex
```
IN MX 10 mx.yandex.net.
```
#### SPF Records
```
v=spf1 -all # Do not accept mail from this domain
v=spf1 include:server.com -all # Accept mail sent by server.com, but no one else
v=spf1 a mx -all # Accept mail sent by the servers specified in the domain's A and MX records
```
Fastmail
```
include:spf.messagingengine.com
```
Gandi
```
include:_mailcust.gandi.net
```
Google
```
include:_spf.google.com
```
Migadu
```
include:spf.migadu.com
```
PolarisMail
```
include:emailarray.com
```
Runbox
```
include:spf.runbox.com
```
Yandex
```
include:_spf.yandex.net
```
Zoho
```
include:zoho.com
```
Multiple "include" example:
```
domain.tld. IN TXT "v=spf1 include:_spf.yandex.net include:spf.messagingengine.com -all"
```
Multiple servers: Accept mail from the server specified in the mail server's A record as well as from any server specified by the SPF record for Yandex.
```
domain.tld. IN TXT "v=spf1 a:mail-server.domain.tld include:_spf.yandex.net ~all"
```

