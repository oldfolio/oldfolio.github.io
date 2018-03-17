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
 
