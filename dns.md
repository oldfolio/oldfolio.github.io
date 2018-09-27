## Public DNS Resolvers
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
domain.com. IN TXT "v=spf1 include:_spf.yandex.net include:spf.messagingengine.com -all"
```
