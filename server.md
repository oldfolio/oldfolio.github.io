## Server
#### SSH Configuration
After you have set up an ordinary user who can su to the root user (_i.e._, is a member of the wheel group in FreeBSD), you should disbale root logins, by adding the following line:
```
PermitRootLogin no
```
to your /etc/ssh/sshd_config file.

If you are slightly more adventurous, you could deny password logins by the root user, but allow root to login using an SSH key:
```
PermitRootLogin prohibit-password
```
If you are _less_ adventurous, you could deny password logins to everyone and require all users to login using an SSH key:
```
PubkeyAuthentication yes # This is often the default
PasswordAuthentication no # This is often the default
ChallengeResponseAuthentication no # This is often NOT the default
```
Of course, you should make sure that key logins are working before disabling password logins. Even after you disable ChallengeResponseAuthentication you should probably leave UsePAM set to yes, because PAM controls more than login authenitcation.

To enable key login for a user, add the user's SSH public key to the file
```
~/.ssh/authorized_keys
```
 
#### Miscellaneous Intial Setup Tasks
Lower the system load average under FreeBSD:
```
# sysctl kern.eventtimer.periodic=1
```
You can make the above sysctl change permanent by adding the line:
```
kern.eventtimer.periodic=1
```
to your /etc/sysctl.conf file.

To completely disable sendmail in FreeBSD, add the following lines:
```
sendmail_enable="NONE"
sendmail_submit_enable="NO"
sendmail_outbound_enable="NO"
sendmail_msp_queue_enable="NO"
```
to the /etc/rc.conf file.

#### Setting Up a Gopher Server
Install pygopherd. The default configuration (on Debian systems found at /etc/pygopherd/pygopherd.conf, on FreeBSD systems found at /usr/local/etc/pygopherd/pygopherd.conf) should work fine but read it anyway so that you understand what pygopherd is doing. Pygopherd will serve files from the /var/gopher directory. All files in that directory (and its subdirectories) must belong to owner gopher and group gopher.

Under FreeBSD, an easy way to start the gopher server automatically on system reboots is to add to the root user's crontab:
```
@reboot /usr/local/bin/pygopherd
```
- - - 
#### Setting Up an Nginx Web Server on FreeBSD 11.1
