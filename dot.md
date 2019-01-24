## Dot Files
A collection of configuration files

/etc/fonts/[local.conf](dot-files/fonts-local.conf)

If you wish to use U2F keys on a Debian-based system, you may need to add the following file:  
/etc/udev/rules.d/[70-u2f.rules](dot-files/70-u2f.rules)  
The above file is a copy of a working rules file that I have used under Debian 8 and 9. To make sure you have the latest version of the file, you should grab it from Yubico's GitHub page:  
/etc/udev/rules.d/[70-u2f.rules](https://github.com/Yubico/libu2f-host/blob/master/70-u2f.rules)  

~/.emacs.d/[init.el](dot-files/emacs.d-init.el.txt)  
~/.emacs.d/el/[typopunct.el](dot-files/typopunct.el)  

~/[.forward](dot-files/forward.txt)

~/.gnupg/[gpg-agent.conf](dot-files/gpg-agent.conf.txt)  
Whenever you make changes to gpg-agent.conf, you should reload the agent:
```
$ gpg-connect-agent reloadagent /bye
```

~/[.kshrc](dot-files/kshrc.txt)

~/[.mkshrc](dot-files/mkshrc.txt) additions

~/.ssh/[authorized_keys](dot-files/authorized_keys.txt)  
~/.ssh/[config](dot-files/ssh-config.txt)

~/[.tcshrc](dot-files/tcshrc.txt)

~/.vim/[gvimrc](dot-files/gvimrc.txt)  
~/.vim/[vimrc](dot-files/vimrc.txt)

- - - 

#### SCRIPTS

[freebsd-cpu-info.sh](dot-files/freebsd-cpu-info.sh.txt)

[shred.sh](dot-files/shred.sh.txt)
