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
Create a detached, ascii-enarmored signature specifying which key to use:
```
gpg -u key-to-use -a --output file.sig --detach-sig file.txt
```
Create a non-detached, ascii-enarmored signature specifying which key to use:
```
gpg -u key-to-use --clearsign file.txt
```
Verify detached signature:
```
gpg --verify signature.sig signed-file.txt
```
Export public key:
```
gpg -a --export {key-identifier} > public-key.asc
```
Export secret/private key:
```
gpg -a --export-secret-keys {key-identifier}  > secret-key.asc
```
#### NETHACK
Some nethack commands:
```
 @ = toggle autopickup
 d = drop
 i = open inventory
 r = read (as in read a spellbook)
 t = throw (as in throw a dagger)
 w = wield weapon
 f = fire arrows in quiver using wielded bow
 Q = place arrows in quiver
 S = save your game and exit
 P = put on (as in put on a ring)
 R = remove (as in remove a ring)
 W = wear armor or shield
 T = take off armor or shield
 Z = cast a spell
 ^d = bash (as in bash a door)
 #chat = talk to another character
 #loot = open a container
 #force = attempt to open a locked container
 #untrap = rescue pet from pit
```
Possible ~/.nethackrc
```
OPTIONS=color,time,hilite_pet,menucolors,!autopickup,role=valkyrie,race=human
#OPTIONS=color,time,role=wizard,race=elf,gender=female
```
#### RSYNC
```
rsync -avu --delete source-directory/ host:/destination-directory
```
Notice that the source directory HAS a trailing slash, but that the destination directory does NOT have a trailing slash.
