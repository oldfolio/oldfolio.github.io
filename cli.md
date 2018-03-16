## Command Line Notes
#### 7-ZIP
```
7za a -mhe=on -p archive-name.7z original-file
```
a = add to archive  
-mhe=on = encrypt headers as well as data  
-p = prompt for password  

#### APT
```
apt-get --no-install-recommends install package-name
```

#### DATE
FreeBSD: Set the date to 5:05 pm, January 21, 2018
```
date 201801211705
     yyyymmddhhmm
```
If you only need to change the hours and minutes
```
date 1705
```
will change the time to 5:05 pm and leave the date unchanged.

#### DIG
Check the mx record for yandex.com at the name server dns1.yandex.net:
```
dig mx yandex.com @dns1.yandex.net
```
#### ELINKS
If you enable one of the color modes, then \<shift\>-5 will cycle through the color schemes for that mode.

#### EMACS
I keep my Emacs notes on their own page. It has not yet been set up.

#### GNUPG
Encrypt to a specific user/recipient:
```
gpg -e -r <user> file.txt
```
