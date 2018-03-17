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
Creating SSL/TLS certificates with [Let's Encrypt](https://letsencrypt.org/) is fairly easy.
```
pkg install py27-certbot
```
This installation returns the following message:
```
===========================================================================

This port installs the "standalone" client only, which does not use and
is not the certbot-auto bootstrap/wrapper script.

The simplest form of usage to obtain certificates is:

 # sudo certbot certonly --standalone -d <domain>, [domain2, ... domainN]>

NOTE:

The client requires the ability to bind on TCP port 80 or 443 (depending
on the --preferred-challenges option used). If a server is running on that
port, it will need to be temporarily stopped so that the standalone server
can listen on that port to complete the challenge authentication process.

For more information on the 'standalone' mode, see:

  https://certbot.eff.org/docs/using.html#standalone

The certbot plugins to support apache and nginx certificate installation
will be made available in the following ports:

 * Apache plugin: security/py-certbot-apache
 * Nginx plugin: security/py-certbot-nginx

===========================================================================
```
It is important to remember that **THE NGINX SERVER MUST BE STOPPED** when you are running the stand-alone version of certbot. With nginx stopped, run
```
certbot certonly --standalone -d www.domain.tld
```
You will be prompted to enter an email address for renewal notifications. Unless certbot encounters errors, your new certificate will be found at
```
/usr/local/etc/letsencrypt/live/www.domain.tld/fullchain.pem
```
and the certificate's private key at:
```
/usr/local/etc/letsencrypt/live/wwww.domain.tld/privkey.pem
```
Renewal is fairly easy as well. With nginx stopped, run
```
certbot renew
```
If you wish to test the renewal process, stop nginx and run
```
certbot renew --dry-run
```
If you have not already done so, you will need to create a DH Params key. In the
```
/etc/ssl/
```
directory, run
```
openssl dhparam -out dhparams.pem 4096
```
This may take a long time, but once it is done you should be ready to implement HTTPS.

In order to implement HTTPS on one of your sites, create two server blocks in the site configuration file. The first should be fairly simple:
```
server {
  listen 80;
  root /path/to/site-root;
  server_name www.domain.tld;
  return 301 https://$server_name$request_uri;
  error_page 404 /404.html;
  }
```
The second should look something like this:
```
server {
  root /path/to/site-root;
  server_name www.domain.tld;
  listen 443;
  ssl on;
  ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
  ssl_ciphers 'ECDHE-RSA-AES128-GCM-SHA256: ... ';
  ssl_prefer_server_ciphers on;
  ssl_certificate /usr/local/etc/letsencrypt/live/www.domain.tld/fullchain.pem;
  ssl_certificate_key /usr/local/etc/letsencrypt/live/www.domain.tld/privkey.pem;
  ssl_dhparam /etc/ssl/dhparams.pem;
  }
```
