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
```
pkg install nginx-lite
```
This installation returns the following message:
```
===================================================================
Recent version of the NGINX introduces dynamic modules support.  In
FreeBSD ports tree this feature was enabled by default with the DSO
knob.  Several vendor's and third-party modules have been converted
to dynamic modules.  Unset the DSO knob builds an NGINX without
dynamic modules support.

To load a module at runtime, include the new `load_module'
directive in the main context, specifying the path to the shared
object file for the module, enclosed in quotation marks.  When you
reload the configuration or restart NGINX, the module is loaded in.
It is possible to specify a path relative to the source directory,
or a full path, please see
https://www.nginx.com/blog/dynamic-modules-nginx-1-9-11/ and
http://nginx.org/en/docs/ngx_core_module.html#load_module for
details.

Default path for the NGINX dynamic modules is

/usr/local/libexec/nginx.
===================================================================
```
Add the line
```
nginx_enable="YES"
```
to /etc/rc.conf.

Create the directory:
```
/usr/local/etc/nginx/sites-enabled
```
This is where you will keep the configuration files for specific sites. You will need to edit
```
/usr/local/etc/nginx/nginx.conf
```
so that nginx will know to use the sites-enabled directory. Just below the gzip line, add
```
include /usr/local/etc/nginx/sites-enabled/*;
```
Test the new configuration changes:
```
nginx -t
```
If everything is working, start or reload nginx:
```
nginx -s reload
```
Now you can start creating site configuration files. They will look something like
```
server {
  root /path/to/site-root;
  server_name www.domain.tld;
  error_page 404 /404.html;
  }
```
This should leave you with a working setup for basic static sites. If you wish to redirect your bare domain to www, then add another server block to the site configuration file:
```
server {
  server_name domain.tld;
  return 301 http://www.domain.tld;
  }
```
##### Adding SSL/TLS Support

