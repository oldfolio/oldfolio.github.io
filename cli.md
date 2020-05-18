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

#### CD
To return to the directory that you just left:
```
cd -
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

#### DD
Overwrite with zeroes a 133 byte file:
```
dd if=/dev/zero of=filename count=1 bs=133
```
Overwrite with zeroes a 1 MB byte file:
```
dd if=/dev/zero of=storage-bin count=1K bs=1024
```
Overwrite with zeroes a 1 GB byte file:
```
dd status=progress if=/dev/zero of=storage-bin count=1024K bs=1024
dd status=progress if=/dev/zero of=storage-bin count=1M bs=1024
```
On my home system, /dev/zero can be used to generate a 10G file in about 45 seconds. By contrast, /dev/urandom will take about three minutes. Do not even bother with /dev/random.

#### DIG
Check the mx record for yandex.com at the name server dns1.yandex.net:
```
dig mx yandex.com @dns1.yandex.net
```
On Debian-based systems, *dig* is supplied by the package *dnsutils*, on FreeBSD by *bind-tools*.

#### DMIDECODE
Displays hardware information. Must be run as root.  See [this guide](https://www.howtoforge.com/dmidecode-finding-out-hardware-details-without-opening-the-computer-case).

To display memory information:
```
dmidecode -t memory
```

#### DPKG
List installed packages:
```
dpkg --get-selections
```
Show the status of package, PACKAGE:
```
dpkg-query --status PACKAGE
```

#### DU
```
du -h --max-depth=1
```

#### DUPLICITY
Backup files in directory "source" to a remote server. The first time duplicity runs it will do a full backup. Subsequently, it will do an incremental backup of changes.
````
duplicity --encrypt-key gpg-key /home/user/source sftp://host//home/user/target

duplicity restore sftp://host//home/user/backup /home/user/local-restore-directory
````
On incremental backups, some versions of duplicity will return the following error message related to GnuPG:
````
Error processing remote manifest
````
This is a known and benign error message that does not indicate any failures in the backup.

#### ELINKS
If you enable one of the color modes, then \<shift\>-5 will cycle through the color schemes for that mode.

You can toggle the numbering of hyperlinks with the period "."

#### EMACS
I keep my [Emacs notes](emacs.md) on their own page.

#### FALLOCATE
fallocate: Preallocate or deallocate space to a file

This command can be used to create large files faster than dd. To create an empty 1 MB file:
```
fallocate -l 1M filename
```
The -l switch specifies the size. K=kilobytes. M=Megabytes. G=Gigabytes. The default is bytes. More specifically, M = 1024*1024 bytes but MB = 1000*1000.

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
If you should ever need to edit your ~/.gnupg/gpg-agent.conf file, you will need to reload the gpg-agent once you are finished editing.
```
$ gpg-connect-agent reloadagent /bye
```
Use extreme caution if you change the gpg-agent to pinentry-curses. Doing so breaks the graphical version of Emacs, and I have not yet found a work-around. If you will be working remotely with GnuPG encrypted files, you may need to set the agent to pinentry-curses. (See the dot file above.) Otherwise, the gpg-agent will expect a graphical environment -- and fail when one is not present.

#### HTML ESCAPE SEQUENCES
```
&amp; will display &
&lt; will display <
&gt; will display >
```

#### LN
```
ln -s target-file link-name
```

#### LOSETUP
```
# losetup -a # List the status of all loop devices
# losetup /dev/loop0 filename # Associate loop device 0 with file filename
# losetup -d /dev/loop0 # Detach loop device
```

#### NAMEBENCH
Send 128 queries to only the nameservers specified:
```
namebench -q 128 -O 208.67.222.222, 1.1.1.1, 8.8.8.8
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

#### OPENSSL
You can use openssl for simple file encryption:
```
openssl enc -blowfish -a -iter 12 -in filename.txt -out filename.enc
```
To decrypt the output file from the above example:
```
openssl enc -d -blowfish -a -iter 12 -in filename.enc -out filename.txt
```
For decryption, notice the addition of the -d switch and the reversal of the input and output filenames. Also, notice that all of the other options are included. Omitting any of those options will yield a failure to decrypt.

Some ciphers that you can use here.

#### RSYNC
```
rsync -avu --delete source-directory/ host:/destination-directory
```
Notice that the source directory HAS a trailing slash, but that the destination directory does NOT have a trailing slash.

Hetzner storage boxes only recognize relative paths. So, your rsync command will need to look something like:
```
rsync -avu --delete local-directory/ hetzner:./directory
                                             ^
                                     notice the dot
```
Synchronize a single file:
```
rsync -avuP source-directory/filename host:/destination-directory/
                                                                 ^
```
Notice that when synchronizing a single file a trailing slash *DOES* follow the destination directory.

#### SECURE_DELETE (FreeBSD) / SECURE-DELETE (Debian)
Overwrite and delete all files and subdirectories of DIRECTORY
```
srm -llr <DIRECTORY>
```

#### SMEM
Report chrome or chromium total memory usage:
```
smem -t -k -c pss -P chrom | tail -n 1
```
Report opera total memory usage:
```
smem -t -k -c pss -P opera | tail -n 1
```
Report yandex browser total memory usage:
```
smem -t -k -c pss -P yandex_b | tail -n 1
```
Report yandex disk total memory usage:
```
smem -t -k -c pss -P yandex-d | tail -n 1
```
Report vivaldi total memory usage:
```
smem -t -k -c pss -P vivaldi | tail -n 1
```

#### SSH
Creating an SSH tunnel:
```
ssh -D 5222 remote-server -N

-D = bind port
-N = do not execute a remote command

chromium --proxy-server=socks5://localhost:5222
```

#### SSHFS
If you install sshfs, you can mount your remote servers as an ordinary user. Use the mount options uid and gid so that the remote directory will belong to the local user.
```
$ sshfs server-nickname:/home/username /local/mountpoint -o uid=1000,gid=1000
```
To unmount:
```
$ fusermount -u /local/mountpoint
```

#### SYSCTL

Report hardware information on FreeBSD systems:
```
# sysctl hw.model hw.machine hw.ncpu
```

#### TAR
To create an archive that excludes some files in the target:
```
tar cvf ~/archive.tar --exclude='excluded-directory/*' *
```
To list the files in an archive:
```
tar tvf archive.tar
```
Compression:

bzip2 = j  
gzip = z  
xz = J  

Create an archive with a time stamp in the archive name:
```
suffix=`date +%F-%H.%M`
tar cvf /home/user/archive-$suffix.tar /path/to/target-directory/
```

#### TMUX
Ctrl-b to enter commands

Detach the current session:
```
Ctrl-b d
```
Re-attach a previous session:
```
tmux attach -t 0
```
where "0" is the name of the previous session.

A [tmux beginner's guide](https://www.ocf.berkeley.edu/~ckuehl/tmux/).

A [tmux cheat sheet](https://tmuxcheatsheet.com/).

#### TWENEX
To log in to SDF's Twenex machine, first log in to your SDF
account. Then telnet to twenex.org. When you arrive at the @
prompt, type login username. You will then be prompted for
your password.
	 
To log out of twenex:
``` 	 
@logout
``` 
You can upload files that you have created elsewhere using ftp:
```	 
ftp user@twenex.org
```
To move a file to a different diretory:
```	 
@rename filename <user.directory>filename
```  
To delete a file:
```  
@delete filename
``` 	 
Change directory:
``` 	 
@cd <user.directory-name>
``` 
List directory contents:
```   	 
@dir
```
Create a directory:
```	 
@BUILD <$USER.NEW-DIRECTORY-NAME>
@@WORKING 1 (or some other number that won't put you over quota)
@@PERMANENT 1 (or some other number that won't put you over quota)
@@
@
```
 
Remove a directory:
```
@BUILD <$USER.DIRECTORY-NAME>
@@KILL
@@
@
``` 

#### USERMOD
To change a user's primary login group:
```
usermod -g primarygroupname username
```
To add a user to a secondary group:
```
usermod -a -G secondarygroupname username
```
Using the **-G** switch without the **-a** switch will remove a user from all secondary groups except those specified by the current instance of the **-G** switch.

#### VIM
Find each occurrence of 'foo' and replace it with 'bar':
```
:%s/foo/bar/g
```
Edit a remote file:
```
vim scp://user@server.com:22//home/user/filename
```
or
```
:e scp://user@server.com:22//home/user/filename
```
Prompt for an encryption key:
```
:X
```
Center text [based on a 75 character-wide line]:
```
 :ce [75]
```
Set the maximum number of characters on a line to 75
```
set tw=75
```
Various editing tasks:
```
dd delete current line
~ switch case of characters (from CAPITALS to lower case or vice VERSA)
U MAKE ALL SELECTED CHARACTERS CAPITALS/UPPER CASE
u make all selected characters lower case
J join next line to the current one
> indent selected lines
gq apply text formatting to selected region

" specify a register
"+ specify the clipboard
"+y copy to clipboard
"+d cut to clipboard
"+P paste from clipboard before cursor
"+p paste from clipbaord after cursor
```
- - - 
#### Miscellaneous
Flush memory caches on a linux machine:
```
echo 3 > /proc/sys/vm/drop_caches
```
Do a searchon "drop_caches" for additional information, including the differences between echo 1, echo 2, and echo 3.

