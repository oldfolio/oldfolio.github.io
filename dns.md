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
