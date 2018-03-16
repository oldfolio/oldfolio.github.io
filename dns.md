## Public DNS Resolvers
#### Dyn
```
216.146.35.35
216.146.36.36
```
Fast in Eastern Europe

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
Fastest in North America

#### OpenNIC Project
Go to https://www.opennic.org to see which of their DNS resolvers are closest to you. OpenNIC servers periodically go down and occasionally just disappear.

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
(For me, Yandex Safe has always been slightly faster than Yandex Basic.)

## Free DNS Hosting
[Cloudflare](https://www.cloudflare.com/): Unlimited lookups; 3500 records per zone; cannot find limit on number of zones.

[ClouDNS](https://www.cloudns.net/): 3 free zones; unlimited records and lookups; no control over TTL on free plans.

[Hurricane Electric](http://dns.he.net/): 50 free zones; cannot find limits on records per zone or lookups.

[Veesp](https://veesp.com/): DNS hosting is available free if you are paying for other services there.

[Vultr](https://www.vultr.com/docs/introduction-to-vultr-dns): DNS hosting is available free if you are paying for other services there. They allow vanity name servers pointed at their own name servers.

[Yandex](https://domain.yandex.com/): 50 free zones; cannot find limits on records per zone or lookups. No CAA records. (Although this service is aimed at people using Yandex to host their email, Yandex support explicitly states that you are free to host DNS without also hosting your email.) 

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
Yandex
```
IN MX 10 mx.yandex.net.
```
